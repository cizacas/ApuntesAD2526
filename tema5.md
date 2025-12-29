
# UNIDAD 5. Acceso a Bases de datos documentales 

## 1. Introducción a las bases de datos documentales nativas

Las bases de datos documentales nativas son sistemas de gestión de bases de datos NoSQL diseñados para almacenar, recuperar y gestionar datos en formato de documento, generalmente en JSON o BSON. MongoDB es el ejemplo más representativo de este tipo de bases de datos.

### Características principales

- **Modelo flexible:** Los documentos pueden tener estructuras diferentes dentro de la misma colección. Almacenamiento basado en parejas clave-valor
- **Escalabilidad horizontal:** Fácil de escalar distribuyendo los datos en varios servidores.
- **Alto rendimiento:** Optimizada para operaciones de lectura y escritura rápidas.
- **Sin esquema fijo:** No requiere definir un esquema antes de almacenar datos.

### Ventajas

- **Flexibilidad en el modelado de datos:** Permite almacenar datos complejos y anidados.
- **Escalabilidad:** Soporta grandes volúmenes de datos y usuarios concurrentes.
- **Desarrollo ágil:** Facilita cambios en la estructura de los datos sin migraciones complejas.
- **Integración con lenguajes modernos:** Dispone de drivers para la mayoría de lenguajes de programación.

### Inconvenientes

- **Consistencia eventual:** En entornos distribuidos, puede haber retrasos en la sincronización de datos.
- **Menor soporte para transacciones complejas:** Aunque MongoDB ha mejorado en este aspecto, no iguala a las bases de datos relacionales.
- **No apta para todas las aplicaciones:** No es la mejor opción para sistemas con relaciones complejas y transacciones estrictas.


### Sistemas de bases de datos NoSQL

Actualmente existen varios sistemas de bases de datos NoSQL ampliamente utilizados:

- **MongoDB**: Muy popular en aplicaciones web modernas, utilizado por empresas como Adobe, eBay y LinkedIn. Desarrollado por MongoDB Inc.
- **CouchDB**: Definido por Apache Software Foundation, usado en aplicaciones que requieren replicación y sincronización sencilla.
- **Cassandra**: Desarrollado inicialmente por Facebook y ahora gestionado por Apache. Utilizado por empresas como Netflix, Instagram y Reddit para grandes volúmenes de datos distribuidos.
- **Redis**: Definido por Redis Labs, ampliamente usado como base de datos en memoria para caching y mensajería en empresas como Twitter y GitHub.
- **Amazon DynamoDB**: Servicio NoSQL gestionado por Amazon Web Services, utilizado por empresas que requieren alta disponibilidad y escalabilidad, como Samsung y Toyota.
- **Firebase Realtime Database**: Definido por Google, muy usado en aplicaciones móviles y web en tiempo real.

Cada uno de estos sistemas ha sido adoptado por grandes compañías y comunidades de desarrollo debido a sus características específicas y ventajas en distintos escenarios.

### Característics de BD MongoDB

MongoDB es una base de datos documental NoSQL ampliamente utilizada por su flexibilidad y escalabilidad. A continuación se detallan sus principales características y formas de instalación:

#### Características principales de MongoDB

- Almacenamiento de datos en documentos BSON (similar a JSON).
- Esquema flexible: los documentos pueden tener diferentes estructuras.
- Escalabilidad horizontal mediante sharding.
- Replicación automática para alta disponibilidad.
- Consultas avanzadas y agregaciones.
- Soporte para índices y búsquedas eficientes.
- Integración con múltiples lenguajes de programación (Java, Python, Node.js, etc.).
- Transacciones multi-documento (desde la versión 4.0).

#### Instalación de MongoDB

MongoDB puede instalarse de varias formas según el entorno:

- **Instalación local:**
  - Descarga desde la web oficial: https://www.mongodb.com/try/download/community
  - Instaladores disponibles para Windows, macOS y Linux.
  - En Windows, se recomienda usar el instalador MSI y el servicio MongoDB como Windows Service.

- **Instalación en la nube:**
  - MongoDB Atlas: servicio gestionado en la nube, permite crear clústeres en AWS, Azure o Google Cloud. https://www.mongodb.com/atlas

- **Instalación mediante Docker:**
  - Usar la imagen oficial: `docker pull mongo`
  - Permite ejecutar MongoDB en contenedores para desarrollo y pruebas.

- **Instalación en servidores Linux:**
  - Mediante gestor de paquetes (apt, yum) siguiendo la documentación oficial.

La elección de la instalación depende de las necesidades del proyecto y del entorno de trabajo (desarrollo, producción, pruebas, etc.).


## 2. Establecimiento y cierre de conexiones

Para interactuar con MongoDB desde una aplicación, es necesario establecer una conexión utilizando un driver específico. Ejemplo en Java usando el driver oficial:

```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoDatabase;

public class ConexionMongoDB {
    public static void main(String[] args) {
        // Establecer conexión
        MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017");
        MongoDatabase database = mongoClient.getDatabase("miBaseDeDatos");

        // ... operaciones ...

        // Cerrar conexión
        mongoClient.close();
    }
}
```

## 3. Colecciones y documentos

- **Colección:** Es equivalente a una tabla en bases de datos relacionales, pero sin esquema fijo.
- **Documento:** Es una estructura de datos en formato BSON (similar a JSON), que representa una fila o registro.

Ejemplo de documento en MongoDB:

```json
{
  "nombre": "Juan",
  "edad": 30,
  "direccion": {
    "calle": "Mayor",
    "ciudad": "Madrid"
  }
}
```


## 4. Creación y borrado de colecciones

### Clases y métodos

- **Crear colección:**
  ```java
  database.createCollection("usuarios");
  ```

- **Eliminar colección:**
  ```java
  database.getCollection("usuarios").drop();
  ```


## 5. Añadir, modificar y eliminar documentos

### Clases y métodos

- **Añadir documento:**
  ```java
  MongoCollection<Document> coleccion = database.getCollection("usuarios");
  Document doc = new Document("nombre", "Ana").append("edad", 25);
  coleccion.insertOne(doc);
  ```
Automáticamente a todos los documentos guardados se le asigna un atributo `_id`de tipo `ObjectId`,  pero podemos alterar ese comportamiento y asignar un valor a ese atributo, por ejemplo:

```java
// Ejemplo en Java usando la colección 'usuarios':

import org.bson.Document;
import com.mongodb.client.MongoCollection;
import java.util.Arrays;

// insertar un documento
MongoCollection<Document> coleccion = database.getCollection("usuarios");
Document doc = new Document("_id", "usuario01")
    .append("nombre", "Ana")
    .append("edad", 25);
coleccion.insertOne(doc);

// También puedes insertar varios documentos a la vez usando insertMany

coleccion.insertMany(Arrays.asList(
  new Document("_id", "usuario02").append("nombre", "Luis").append("edad", 30),
  new Document("_id", "usuario03").append("nombre", "Marta").append("edad", 28)
));

// En este ejemplo, se insertan dos documentos con _id asignados manualmente en la colección 'usuarios'.
```
En este ejemplo, el campo `_id` se asigna manualmente en lugar de un `ObjectId` generado automáticamente.

```js
// Ejemplo en el shell de MongoDB usando insertMany con los documentos entre llaves:
db.usuarios.insertMany([
  {
    _id: "usuario04",
    nombre: "Carlos",
    edad: 22
  },
  {
    _id: "usuario05",
    nombre: "Lucía",
    edad: 27
  }
])
```
En este ejemplo, insertMany recibe un array de documentos, cada uno envuelto entre llaves.


- **Modificar documento:**
  ```java
  import com.mongodb.client.model.Filters;
  import com.mongodb.client.model.Updates;
  
  // Ejemplo de updateOne para modificar un documento
  coleccion.updateOne(
      Filters.eq("nombre", "Ana"),
      Updates.set("edad", 26)
  );

  // Ejemplo de updateMany para modificar varios documentos: 
  // Modificar la edad a 35 para todos los usuarios con edad menor que 30
  coleccion.updateMany(
    Filters.lt("edad", 30),
    Updates.set("edad", 35)
  );

  // En este ejemplo, todos los documentos de la colección 'usuarios' con edad menor que 30 tendrán ahora edad 35.
  ```

- **Eliminar documento:**
Elimina solo e primero de los eliminables
  ```java
  coleccion.deleteOne(Filters.eq("nombre", "Ana"));
  ```

- **Eliminar varios documentos (deleteMany):**
  ```java
  // Eliminar todos los usuarios con edad igual a 35
  coleccion.deleteMany(Filters.eq("edad", 35));
  ```
  En este ejemplo, se eliminan todos los documentos de la colección 'usuarios' donde la edad es 35.

## 6. Lenguajes de consulta y realización de consultas

MongoDB utiliza su propio lenguaje de consultas basado en JSON.

### Consultar documentos

El método `find` de MongoDB puede usarse con tres sintaxis principales:

| Sintaxis                                 | Explicación                                                      |
|------------------------------------------|------------------------------------------------------------------|
| find()                                   | Devuelve todos los documentos de la colección                    |
| find(filtro)                             | Devuelve los documentos que cumplen el filtro especificado       |
| find(filtro, proyección)                 | Devuelve los documentos que cumplen el filtro, mostrando solo los campos indicados en la proyección |

Ejemplos:
```js
db.usuarios.find() // Todos los documentos
db.usuarios.find({edad: 25}) // Documentos con edad 25
db.usuarios.find({edad: 25}, {nombre: 1, edad: 1, _id: 0}) // Solo nombre y edad, sin _id
```

  ```java
  FindIterable<Document> docs = coleccion.find();
  for (Document d : docs) {
      System.out.println(d.toJson());
  }
  ```

A los resultados obtenidos con find se le pueden aplicar varios métodos modificadores.

| Método         | Descripción                                                      |
|---------------|------------------------------------------------------------------|
| limit(n)      | Limita el número de documentos devueltos a n                     |
| skip(n)       | Omite los primeros n documentos del resultado                    |
| sort(campo)   | Ordena los resultados según el campo especificado                |
| count()       | Devuelve el número de documentos que cumplen la consulta         |
| pretty()      | Muestra los resultados en formato legible (solo en el shell)     |
| projection    | Permite seleccionar los campos que se desean mostrar             |
| forEach(func) | Aplica una función a cada documento del resultado                |

Ejemplo de uso en el shell:
```js
db.usuarios.find({edad: {$gt: 20}}).sort({nombre: 1}).limit(5).pretty()
```
Explicación paso a paso:
* db.usuarios.find({edad: {$gt: 20}})
Busca todos los documentos en la colección usuarios donde el campo edad es mayor que 20.
* .sort({nombre: 1})
Ordena los resultados por el campo nombre en orden ascendente (de la A a la Z).
* .limit(5)
Limita el resultado a los primeros 5 documentos que cumplen la consulta.
* .pretty()
Muestra los documentos en un formato más legible en el shell de MongoDB.

Resumen:
El comando busca los usuarios con edad mayor a 20, los ordena alfabéticamente por nombre, muestra solo los primeros 5 y los presenta de forma legible.

  ```java
  coleccion.find(Filters.eq("edad", 30));
  ```

### Operadores lógicos 
| Operador | Descripción                  | Ejemplo de uso                      |
|----------|------------------------------|-------------------------------------|
| $and     | Y lógico                     | Filters.and(f1, f2)                 |
| $or      | O lógico                     | Filters.or(f1, f2)                  |
| $not     | Negación                     | Filters.not(Filters.eq("edad", 20))|
| $nor     | Ni uno ni otro               | Filters.nor(f1, f2)                 |

### Operadores de comparación
| Operador | Descripción                  | Ejemplo de uso                      |
|----------|------------------------------|-------------------------------------|
| $eq      | Igual a                      | Filters.eq("edad", 20)              |
| $ne      | Distinto de                  | Filters.ne("edad", 20)              |
| $gt      | Mayor que                    | Filters.gt("edad", 20)              |
| $gte     | Mayor o igual que            | Filters.gte("edad", 20)             |
| $lt      | Menor que                    | Filters.lt("edad", 20)              |
| $lte     | Menor o igual que            | Filters.lte("edad", 20)             |
| $in      | En un conjunto de valores    | Filters.in("edad", Arrays.asList(20, 25)) |
| $nin     | No está en un conjunto       | Filters.nin("edad", Arrays.asList(20, 25)) |

#### Ejemplo combinado:
```java
// Buscar usuarios con edad mayor de 25 Y nombre igual a "Ana"
coleccion.find(Filters.and(
  Filters.gt("edad", 25),
  Filters.eq("nombre", "Ana")
));

// Buscar usuarios con edad menor de 20 O nombre igual a "Luis"
coleccion.find(Filters.or(
  Filters.lt("edad", 20),
  Filters.eq("nombre", "Luis")
));
```

### Selectores de elemento en MongoDB
| Selector      | Descripción                                         | Ejemplo de uso                                 |
|--------------|-----------------------------------------------------|------------------------------------------------|
| $exists      | Comprueba si un campo existe en el documento        | Filters.exists("email")                       |
| $type        | Comprueba el tipo de dato de un campo               | Filters.type("edad", "int")                  |
| $regex       | Busca coincidencias con una expresión regular       | Filters.regex("nombre", "^A")                |
| $expr        | Permite usar expresiones de agregación en consultas | Filters.expr(Document.parse("{ $gt: [ '$edad', 20 ] }")) |
| $text       | Realiza búsquedas de texto completo                  | Filters.text("palabra clave")                   |

#### Ejemplo de cada selector:
```java
// $exists: Buscar usuarios que tengan el campo 'email'
coleccion.find(Filters.exists("email"));

// $type: Buscar usuarios donde 'edad' es de tipo entero
coleccion.find(Filters.type("edad", "int"));

// $regex: Buscar usuarios cuyo nombre empieza por 'A'
coleccion.find(Filters.regex("nombre", "^A"));

// $expr: Buscar usuarios cuya edad es mayor que 20 usando una expresión
coleccion.find(Filters.expr(Document.parse("{ $gt: [ '$edad', 20 ] }")));

// $text: Buscar usuarios que contengan la palabra 'programador' en los campos indexados para texto
coleccion.find(Filters.text("programador"));
```

### Selectores de arrays en MongoDB
| Selector      | Descripción                                         | Ejemplo de uso                                 |
|--------------|-----------------------------------------------------|------------------------------------------------|
| $all         | El array contiene todos los valores especificados    | Filters.all("tags", Arrays.asList("java", "mongodb")) |
| $elemMatch   | Al menos un elemento del array cumple la condición   | Filters.elemMatch("notas", Filters.gt("valor", 8))    |
| $size        | El array tiene el tamaño especificado                | Filters.size("notas", 3)                              |

#### Ejemplo de cada selector:
```java
// $all: Buscar usuarios cuyo array 'tags' contiene 'java' y 'mongodb'
coleccion.find(Filters.all("tags", Arrays.asList("java", "mongodb")));

// $elemMatch: Buscar usuarios con al menos una nota mayor que 8 en el array 'notas'
coleccion.find(Filters.elemMatch("notas", Filters.gt("valor", 8)));

// $size: Buscar usuarios cuyo array 'notas' tiene exactamente 3 elementos
coleccion.find(Filters.size("notas", 3));
``` 

## OPERACIONES DE AGREGACIÓN

MongoDB dispone de múltiples funciones de agregado. Se trata de funciones que permiten realizar operaciones de agregación, procesando documentos y obteniendo nuevos resultados, calculados o transformados.

Hay tres formas de realizar operaciones de agregación:

- **Agregación pipeline**
- **Agregación map-reduce**
- **Agregación de propósito único**

En este tema solo veremos la **agregación pipeline**. De todas las posibilidades, nos centraremos en las agrupaciones de documentos que comparten el valor de uno o varios atributos.

### Funciones de agrupamiento más comunes

| Función      | Descripción                                                        |
|-------------|---------------------------------------------------------------------|
| $sum        | Suma los valores de un campo para cada grupo                        |
| $avg        | Calcula la media de los valores de un campo para cada grupo         |
| $min        | Devuelve el valor mínimo de un campo para cada grupo                |
| $max        | Devuelve el valor máximo de un campo para cada grupo                |
| $push       | Crea un array con los valores de un campo para cada grupo           |
| $addToSet   | Crea un array de valores únicos de un campo para cada grupo         |
| $first      | Devuelve el primer valor de un campo en cada grupo (según orden)    |
| $last       | Devuelve el último valor de un campo en cada grupo (según orden)    |

Ejemplo básico de uso de pipeline para agrupar:
```js
db.usuarios.aggregate([
  {
    $group: {
      _id: "$edad",
      total: { $sum: 1 }
    }
  }
])
```
En este ejemplo, se agrupan los usuarios por edad y se cuenta cuántos hay de cada edad.


