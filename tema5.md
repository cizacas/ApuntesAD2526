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

:computer: [Instalar MongoDB Máquina Virtual](/ApuntesAD2526/instalacion_MongoDB.md)

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
:computer: Ejemplo 

## 3. Colecciones y documentos

  - **Colección:** Es equivalente a una tabla en bases de datos relacionales, pero sin esquema fijo.
  - **Documento:** Es una estructura de datos en **formato BSON** (similar a JSON), que representa una fila o registro.
  > **⚠️ Nota:**
  > Cuando tratamos documentos `Document` en java tenemos que importar `import org.bson.Document`

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
  - **Desde la shell de MongoDB:**
    ```js
    db.createCollection("usuarios");
    ```
  - **Desde Java:**
      ```java
      database.createCollection("usuarios");
      ```
- **Eliminar colección:**
  - **Desde la shell de MongoDB:**
    ```js
     db.usuarios.drop()
    ```
  - **Desde Java:**
      ```java
      database.getCollection("usuarios").drop();
      ```

## 5. Añadir, modificar y eliminar documentos

### Añadir documento

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.insertOne({ nombre: "Ana", edad: 25 })
  ```

**Desde Java:**
  ```java
  MongoCollection<Document> coleccion = database.getCollection("usuarios");
  Document doc = new Document("nombre", "Ana").append("edad", 25);
  coleccion.insertOne(doc);
  ```

### Insertar varios documentos

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.insertMany([
    { _id: "usuario01", nombre: "Ana", edad: 25 },
    { _id: "usuario02", nombre: "Luis", edad: 30 },
    { _id: "usuario03", nombre: "Marta", edad: 28 }
  ])
  ```

**Desde Java:**
  ```java
  coleccion.insertMany(Arrays.asList(
    new Document("_id", "usuario01").append("nombre", "Ana").append("edad", 25),
    new Document("_id", "usuario02").append("nombre", "Luis").append("edad", 30),
    new Document("_id", "usuario03").append("nombre", "Marta").append("edad", 28)
  ));
  ```

### Modificar documento

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.updateOne(
    { nombre: "Ana" },
    { $set: { edad: 26 } }
  )
  ```

**Desde Java:**
  ```java
  coleccion.updateOne(
    Filters.eq("nombre", "Ana"),
    Updates.set("edad", 26) //modifica la primera colección que cumple la condición
  );
  ```

### Modificar varios documentos

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.updateMany(
    { edad: { $lt: 30 } },
    { $set: { edad: 35 } }
  )
  ```

**Desde Java:**
  ```java
  coleccion.updateMany(
    Filters.lt("edad", 30),
    Updates.set("edad", 35)
  );
  ```

### Eliminar documento

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.deleteOne({ nombre: "Ana" }) //Elimina la primera colección que cumple la condición.
  ```

**Desde Java:**
  ```java
  coleccion.deleteOne(Filters.eq("nombre", "Ana"));
  ```

### Eliminar varios documentos

**Desde la shell de MongoDB:**
  ```js
  db.usuarios.deleteMany({ edad: 35 })
  ```

**Desde Java:**
  ```java
  coleccion.deleteMany(Filters.eq("edad", 35));
  ```

## 6. Lenguajes de consulta y realización de consultas

MongoDB utiliza su propio lenguaje de consultas basado en JSON.

### Consultar documentos

El método `find` de MongoDB puede usarse con tres sintaxis principales:

| Sintaxis                                 | Explicación                                                      |
|------------------------------------------|------------------------------------------------------------------|
| find()                                   | Devuelve todos los documentos de la colección                    |
| find(filtro)                             | Devuelve los documentos que cumplen el filtro especificado       |
| find(filtro, proyección)                 | Devuelve los documentos que cumplen el filtro, mostrando solo los campos indicados en la proyección |

**Consultar documentos:**

**Desde la shell de MongoDB:**
```js
db.usuarios.find() // Todos los documentos
db.usuarios.find({edad: 25}) // Documentos con edad 25
db.usuarios.find({edad: 25}, {nombre: 1, edad: 1, _id: 0}) // Solo nombre y edad, sin _id
```

**Desde Java:**
```java
//todos los documentos
FindIterable<Document> docs = coleccion.find(); 
for (Document d : docs) {
    System.out.println(d.toJson());
}
//busca y muestra todos los documentos de la colección usuarios donde la edad es 25.
FindIterable<Document> docs = coleccion.find(Filters.eq("edad", 25));
for (Document d : docs) {
    System.out.println(d.toJson());
}
//muestra solo los campos nombre y edad, excluyendo el campo _id, para los documentos donde edad es 25.
FindIterable<Document> docs = coleccion.find(Filters.eq("edad", 25))
    .projection(Projections.fields(
        Projections.include("nombre", "edad"),
        Projections.exclude("_id")
    ));
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

**Ejemplo de consulta avanzada:**

**Desde la shell de MongoDB:**
```js
db.usuarios.find({edad: {$gt: 20}}).sort({nombre: 1}).limit(5).pretty()
```

**Desde Java:**
```java
FindIterable<Document> docs = coleccion.find(Filters.gt("edad", 20))
  .sort(Sorts.ascending("nombre"))
  .limit(5);
for (Document d : docs) {
  System.out.println(d.toJson());
}
```

**Explicación paso a paso:**
- `find({edad: {$gt: 20}})`: Busca todos los documentos en la colección usuarios donde el campo edad es mayor que 20.
- `sort({nombre: 1})`: Ordena los resultados por el campo nombre en orden ascendente (de la A a la Z).
- `limit(5)`: Limita el resultado a los primeros 5 documentos que cumplen la consulta.
- `pretty()`: Muestra los documentos en un formato más legible en el shell de MongoDB.

**Resumen:**
El comando busca los usuarios con edad mayor a 20, los ordena alfabéticamente por nombre, muestra solo los primeros 5 y los presenta de forma legible.

### Operadores lógicos 
| Operador | Descripción                  | Ejemplo de uso                      |
|----------|------------------------------|-------------------------------------|
| $and     | Y lógico                     | Filters.and(f1, f2)                 |
| $or      | O lógico                     | Filters.or(f1, f2)                  |
| $not     | Negación                     | Filters.not(Filters.eq("edad", 20))|
| $nor     | Ni uno ni otro               | Filters.nor(f1, f2)                 |

## Ejemplos de operadores lógicos

**Desde la shell de MongoDB:**
```js
//busca en la colección usuarios todos los documentos que cumplen ambas condiciones:
//El campo edad es mayor que 20.
//El campo nombre es igual a "Ana".
db.usuarios.find({ $and: [ { edad: { $gt: 20 } }, { nombre: "Ana" } ] })
//busca en la colección usuarios todos los documentos que cumplen al menos una de estas dos condiciones:
//El campo edad es menor que 20.
//El campo nombre es igual a "Luis".
db.usuarios.find({ $or: [ { edad: { $lt: 20 } }, { nombre: "Luis" } ] })
//busca en la colección usuarios todos los documentos donde el campo edad no es mayor que 30.
//Es decir, selecciona los documentos cuyo valor de edad es 30 o menos.
db.usuarios.find({ edad: { $not: { $gt: 30 } } })
//busca en la colección usuarios todos los documentos que no cumplen ninguna de estas dos condiciones:
//El campo edad es igual a 25.
//El campo nombre es igual a "Ana".
//Es decir, selecciona los documentos donde la edad no es 25 y el nombre no es "Ana".
db.usuarios.find({ $nor: [ { edad: 25 }, { nombre: "Ana" } ] })
```
**Desde Java:**
```java
coleccion.find(Filters.and(
  Filters.gt("edad", 20),
  Filters.eq("nombre", "Ana")
));

coleccion.find(Filters.or(
  Filters.lt("edad", 20),
  Filters.eq("nombre", "Luis")
));

coleccion.find(Filters.not(Filters.gt("edad", 30)));
coleccion.find(Filters.nor(
  Filters.eq("edad", 25),
  Filters.eq("nombre", "Ana")
));

```

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
**Desde la Shell de MongoDB:**
```js
// Buscar usuarios con edad mayor de 25 Y nombre igual a "Ana"
db.usuarios.find({ $and: [ { edad: { $gt: 25 } }, { nombre: "Ana" } ] })

// Buscar usuarios con edad menor de 20 O nombre igual a "Luis"
db.usuarios.find({ $or: [ { edad: { $lt: 20 } }, { nombre: "Luis" } ] })
```

**Desde Java:**
```java
coleccion.find(Filters.and(
  Filters.gt("edad", 25),
  Filters.eq("nombre", "Ana")
));

coleccion.find(Filters.or(
  Filters.lt("edad", 20),
  Filters.eq("nombre", "Luis")
));
```

### Selectores de elemento
| Selector      | Descripción                                         | Ejemplo de uso                                 |
|--------------|-----------------------------------------------------|------------------------------------------------|
| $exists      | Comprueba si un campo existe en el documento        | Filters.exists("email")                       |
| $type        | Comprueba el tipo de dato de un campo               | Filters.type("edad", "int")                  |
| $regex       | Busca coincidencias con una expresión regular       | Filters.regex("nombre", "^A")                |
| $expr        | Permite usar expresiones de agregación en consultas | Filters.expr(Document.parse("{ $gt: [ '$edad', 20 ] }")) |
| $text       | Realiza búsquedas de texto completo                  | Filters.text("palabra clave")                   |

#### Ejemplo de cada selector:

**Desde la Shell de MongoDB:**
```js
// $exists: Buscar usuarios que tengan el campo 'email'
db.usuarios.find({ email: { $exists: true } })

// $type: Buscar usuarios donde 'edad' es de tipo entero
db.usuarios.find({ edad: { $type: "int" } })

// $regex: Buscar usuarios cuyo nombre empieza por 'A'
db.usuarios.find({ nombre: { $regex: "^A" } })

// $expr: Buscar usuarios cuya edad es mayor que 20 usando una expresión
db.usuarios.find({ $expr: { $gt: [ "$edad", 20 ] } })

// $text: Buscar usuarios que contengan la palabra 'programador' en los campos indexados para texto
db.usuarios.find({ $text: { $search: "programador" } })
```
**Desde Java:**

```java
coleccion.find(Filters.exists("email"));

coleccion.find(Filters.type("edad", "int"));

coleccion.find(Filters.regex("nombre", "^A"));

coleccion.find(Filters.expr(Document.parse("{ $gt: [ '$edad', 20 ] }")));

coleccion.find(Filters.text("programador"));
```

### Operadores de actualización
| Operador | Descripción                                               | Ejemplo de uso                                      |
|----------|-----------------------------------------------------------|-----------------------------------------------------|
| $set     | Asigna un valor a un campo                                | Updates.set("edad", 30)                             |
| $unset   | Elimina un campo del documento                            | Updates.unset("email")                              |
| $inc     | Incrementa o decrementa el valor de un campo numérico     | Updates.inc("edad", 1)                              |
| $mul     | Multiplica el valor de un campo numérico                  | Updates.mul("salario", 1.1)                         |
| $rename  | Cambia el nombre de un campo                              | Updates.rename("nombre", "nombreCompleto")          |
| $push    | Añade un elemento a un array                              | Updates.push("notas", 9)                            |
| $pull    | Elimina elementos de un array que cumplan una condición   | Updates.pull("notas", 5)                            |
| $addToSet| Añade un elemento a un array solo si no existe            | Updates.addToSet("tags", "nuevo")                  |

#### Ejemplo de uso de operadores de actualización:
**Desde la Shell de MongoDB:**
```js
// $set: Asignar la edad a 30 para el usuario con nombre "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $set: { edad: 30 } }
)

// $unset: Eliminar el campo 'email' del usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $unset: { email: "" } }
)

// $inc: Incrementar la edad en 1 para el usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $inc: { edad: 1 } }
)

// $mul: Multiplicar el salario por 1.1 para el usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $mul: { salario: 1.1 } }
)

// $rename: Cambiar el campo 'nombre' a 'nombreCompleto' para el usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $rename: { nombre: "nombreCompleto" } }
)

// $push: Añadir la nota 9 al array 'notas' del usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $push: { notas: 9 } }
)

// $pull: Eliminar la nota 5 del array 'notas' del usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $pull: { notas: 5 } }
)

// $addToSet: Añadir el tag 'nuevo' al array 'tags' solo si no existe, para el usuario "Ana"
db.usuarios.updateOne(
  { nombre: "Ana" },
  { $addToSet: { tags: "nuevo" } }
)

```
**Desde Java:**
```java

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.set("edad", 30)
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.unset("email")
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.inc("edad", 1)
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.mul("salario", 1.1)
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.rename("nombre", "nombreCompleto")
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.push("notas", 9)
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.pull("notas", 5)
);

coleccion.updateOne(
  Filters.eq("nombre", "Ana"),
  Updates.addToSet("tags", "nuevo")
);
```

### Selectores de arrays
| Selector      | Descripción                                         | Ejemplo de uso                                 |
|--------------|-----------------------------------------------------|------------------------------------------------|
| $all         | El array contiene todos los valores especificados    | Filters.all("tags", Arrays.asList("java", "mongodb")) |
| $elemMatch   | Al menos un elemento del array cumple la condición   | Filters.elemMatch("notas", Filters.gt("valor", 8))    |
| $size        | El array tiene el tamaño especificado                | Filters.size("notas", 3)                              |

#### Ejemplo de cada selector:
**Desde la Shell de MongoDB:**
```js
// $all: Buscar usuarios cuyo array 'tags' contiene 'java' y 'mongodb'
db.usuarios.find({ tags: { $all: ["java", "mongodb"] } })
// $elemMatch: Buscar usuarios con al menos una nota mayor que 8 en el array 'notas'
db.usuarios.find({ notas: { $elemMatch: { valor: { $gt: 8 } } } })
// $size: Buscar usuarios cuyo array 'notas' tiene exactamente 3 elementos
db.usuarios.find({ notas: { $size: 3 } })
```

**Desde Java:**
```java

coleccion.find(Filters.all("tags", Arrays.asList("java", "mongodb")));

coleccion.find(Filters.elemMatch("notas", Filters.gt("valor", 8)));

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
**Desde la Shell de MongoDB:**
```js
//En este ejemplo, se agrupan los usuarios por edad y se cuenta cuántos hay de cada edad.
db.usuarios.aggregate([
  {
    $group: {
      _id: "$edad",
      total: { $sum: 1 }
    }
  }
])
```
**Desde Java:**
```java

// ...
import com.mongodb.client.AggregateIterable;
import org.bson.Document;
import java.util.Arrays;

// Agrupar usuarios por edad y contar cuántos hay de cada edad
AggregateIterable<Document> resultado = coleccion.aggregate(Arrays.asList(
    new Document("$group", new Document("_id", "$edad")
        .append("total", new Document("$sum", 1)))
));

for (Document doc : resultado) {
    System.out.println("Edad: " + doc.get("_id") + ", Total: " + doc.get("total"));
}
// ...
```
:computer: Hoja01 y Hoja02