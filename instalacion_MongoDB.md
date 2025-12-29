# Instalación de MongoDB Community Server en una máquina virtual

## 1. Instalación en máquina virtual

Para un entorno de pruebas o aprendizaje, se recomienda utilizar una máquina virtual ligera y gratuita como por ejemplo Ubuntu server:

### Ubuntu Server 
- **Ventajas:**
  - Gratuito y de código abierto.
  - Consumo de recursos bajo.
  - Gran comunidad y abundante documentación.
- **Descarga:** https://ubuntu.com/download/server , ubuntu server 22.04.5 LTS(jammy Jellyfish)  

Para crear la máquina virtual utilizamos VMware Workstation Pro 17 instalado en los pc de clase.

## 2. Instalación de MongoDB Community Server en Ubuntu

### Paso 1: Actualizar el sistema
```bash
sudo apt update
sudo apt upgrade
```

### Paso 2: Importar la clave pública de MongoDB
```bash
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg
```

### Paso 3: Crear el archivo de lista de MongoDB
Para Ubuntu 22.04:
```bash
echo "deb [arch=amd64,arm64] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

### Paso 4: Instalar MongoDB
```bash
sudo apt update
sudo apt install -y mongodb-org
```

### Paso 5: Iniciar y habilitar el servicio
```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

### Paso 6: Verificar el estado
```bash
sudo systemctl status mongod
```

## 3. Alternativa: Usar MongoDB Atlas (nube)

Si prefieres no gestionar la instalación y mantenimiento, puedes usar **MongoDB Atlas**, el servicio gestionado en la nube de MongoDB. Ofrece un plan gratuito y es ideal para desarrollo y pruebas.

- **Ventajas:**
  - No requiere instalación local.
  - Acceso desde cualquier lugar.
  - Escalabilidad y backups automáticos.

- **Enlace:** https://www.mongodb.com/atlas

## 4. ¿Qué opción elegir?

- **Máquina virtual local:**
  - Ideal para aprender, experimentar y tener control total del entorno.
  - Requiere más configuración inicial.

- **MongoDB Atlas (AWS, Azure, Google Cloud):**
  - Ideal para proyectos reales, trabajo en equipo o si necesitas acceso remoto.
  - Más sencillo y rápido de poner en marcha.

**Recomendación:** Para aprender y practicar, usa una máquina virtual ligera con Ubuntu Server. Para proyectos colaborativos o acceso remoto, considera MongoDB Atlas.

## 5. Tutorial básico: Probar MongoDB tras la instalación

### Acceder al shell de MongoDB
Para interactuar con MongoDB desde la terminal, usa el comando:
```bash
mongosh
```
Esto abrirá el shell interactivo de MongoDB.

### Consultar las bases de datos existentes
En el shell de MongoDB, ejecuta:
```js
show dbs
```
Esto mostrará las bases de datos de prueba (por defecto suelen ser `admin`, `local` y `config`).

### Crear una base de datos y una colección de prueba
En el shell:
```js
use testdb
db.usuarios.insertOne({nombre: "Ana", edad: 25})
show dbs
db.usuarios.find()
```
Esto crea una base de datos llamada `testdb`, una colección `usuarios` y añade un documento. Puedes consultar los datos insertados con `db.usuarios.find()`.

### Salir del shell
Escribe:
```js
exit
```

### Verificar el puerto de escucha
Por defecto, MongoDB escucha en el puerto **27017**. Puedes comprobarlo con:

```bash
 sudo ss -tulnp | grep mongod
```

Con estos pasos puedes verificar que MongoDB está funcionando correctamente y empezar a trabajar con bases de datos y colecciones.

## 6. Configurar MongoDB para conexiones externas

Si necesitas acceder a MongoDB desde fuera de la máquina virtual (por ejemplo, desde tu PC anfitrión o desde otra máquina), sigue estos pasos:

### 1. Editar la configuración de MongoDB
Abre el archivo de configuración:
```bash
sudo nano /etc/mongod.conf
```
Busca la línea:
```
bindIp: 127.0.0.1
```
Cámbiala por:
```
bindIp: 0.0.0.0
```
Guarda y cierra el archivo (Ctrl+O, Enter, Ctrl+X).

### 2. Reiniciar el servicio de MongoDB
```bash
sudo systemctl restart mongod
```

### 3. Configurar la red de la máquina virtual

- En VMware: selecciona “Bridged” o “NAT” y configura el reenvío de puertos si es necesario.

### 4. Permitir el puerto 27017 en el firewall (ufw)
Si tienes ufw activado, ejecuta:
```bash
sudo ufw allow 27017/tcp
sudo ufw reload
```
Puedes comprobar si ufw está activo con:
```bash
sudo ufw status
```

### 5. Obtener la IP de la VM
```bash
ip a
```
Usa esa IP en tu aplicación Java o cliente MongoDB en vez de localhost.


## 7. Ejemplo de conexión a MongoDB desde Java

Para conectar una aplicación Java con MongoDB, necesitas el driver oficial. Puedes añadirlo a tu proyecto Maven con:
```xml
<dependency>
  <groupId>org.mongodb</groupId>
  <artifactId>mongodb-driver-sync</artifactId>
  <version>5.6.2</version>
</dependency>
```

### Código de ejemplo
```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoDatabase;

public class ConexionMongoDB {
    public static void main(String[] args) {
        // Conexión al servidor local (puerto por defecto 27017)
        // cambiar localhost por la ip del servidor
        MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017");
        MongoDatabase database = mongoClient.getDatabase("testdb");

        System.out.println("Conexión exitosa a la base de datos: " + database.getName());

        // Cerrar la conexión
        mongoClient.close();
    }
}
```

Este ejemplo conecta con la base de datos `testdb` en el servidor local. Si la base de datos no existe, MongoDB la crea automáticamente al insertar datos.

