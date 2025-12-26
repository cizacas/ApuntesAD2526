# UNIDAD 4. Acceso a datos en bases de datos orientadas a objetos y objeto-relacionales

## 1. Gestores de bases de datos objeto-relacionales (BDOR)

### Características
- Combinan características de bases de datos relacionales y orientadas a objetos.
- Permiten definir tipos de datos complejos (objetos, arrays, colecciones) en las tablas.
- Soportan herencia, encapsulamiento y polimorfismo en los datos.
- Extienden SQL (ANSI SQL) para gestionar objetos y sus relaciones.
- Permiten almacenar métodos junto con los datos (funciones asociadas a tipos).

### Ventajas
- Mayor flexibilidad para modelar datos complejos.
- Integración sencilla con aplicaciones orientadas a objetos.
- Reutilización de código y estructuras de datos.
- Mejor rendimiento en consultas sobre datos complejos.

### Gestión de objetos con SQL; ANSI SQL
- SQL se amplía con tipos de datos definidos por el usuario (UDT).
- Se pueden crear, consultar y modificar objetos directamente con sentencias SQL estándar y extendidas.

### Ejemplo de gestores de bases de datos objeto-relacionales

Algunos ejemplos populares de gestores de bases de datos objeto-relacionales son:

- **PostgreSQL**: Permite definir tipos de datos complejos, herencia de tablas y funciones asociadas a tipos. Es uno de los sistemas más utilizados y con mayor soporte para características objeto-relacionales.
- **Oracle Database**: Incluye extensiones objeto-relacionales, permitiendo la creación de tipos de objetos, colecciones y métodos asociados.
- **IBM Db2**: Ofrece soporte para tipos de datos definidos por el usuario y objetos, integrando características relacionales y orientadas a objetos.

Estos gestores permiten trabajar tanto con datos relacionales tradicionales como con estructuras de datos complejas propias de la orientación a objetos.

### Acceso a funciones del gestor desde el lenguaje de programación
- APIs específicas (JDBC, ODBC, etc.) permiten manipular objetos y ejecutar funciones del gestor desde lenguajes como Java, C#, Python, etc.
- Soporte para procedimientos almacenados y funciones definidas por el usuario.

## 2. Gestores de bases de datos orientadas a objetos (BDOO)

### Características
- Almacenan datos como objetos, igual que en la programación orientada a objetos.
- Soportan herencia, encapsulamiento y polimorfismo.
- Los objetos pueden contener otros objetos (composición).
- No requieren conversión entre objetos y tablas (no hay "impedancia relacional").

### Estándares en bases de datos orientadas a objetos

Existen varios estándares que definen cómo debe ser la persistencia y gestión de objetos en bases de datos orientadas a objetos:

- **ODMG (Object Data Management Group):** Especifica un modelo estándar para bases de datos orientadas a objetos, incluyendo un lenguaje de definición de objetos, un lenguaje de consulta (OQL) y una API para distintos lenguajes de programación.
- **JPA (Java Persistence API):** Es un estándar de Java para la persistencia, gestión y consulta de datos entre objetos Java y bases de datos relacionales u orientadas a objetos.
- **JDO (Java Data Objects):** Especifica una interfaz estándar para la persistencia y recuperación de objetos Java, independiente del tipo de base de datos subyacente.

**Cumplimiento de estándares por gestores:**
- **ObjectDB:** Cumple con los estándares ODMG, JPA y JDO, lo que le permite integrarse fácilmente en aplicaciones Java y garantizar portabilidad y compatibilidad con frameworks y herramientas estándar.
- Otros gestores como db4o, Versant o GemStone/S pueden tener compatibilidad parcial o APIs propias, pero no cumplen todos estos estándares de forma completa.

### Ejemplo de gestores de bases de datos orientadas a objetos

Algunos ejemplos de gestores de bases de datos orientadas a objetos son:

- **ObjectDB**: Un sistema de base de datos orientado a objetos para Java, ampliamente utilizado en aplicaciones Java y JPA.
- **db4o**: Base de datos orientada a objetos para Java y .NET, fácil de integrar en aplicaciones OO.
- **Versant Object Database**: Utilizada en aplicaciones empresariales que requieren alto rendimiento y modelado complejo de objetos.
- **GemStone/S**: Base de datos orientada a objetos para Smalltalk y Java, con soporte avanzado para transacciones y persistencia de objetos.

Estos gestores permiten almacenar y gestionar objetos directamente, manteniendo sus relaciones, herencia y métodos asociados.

**¿Cuáles de estos gestores son embebidos?**

De los ejemplos mencionados, los siguientes gestores pueden funcionar como bases de datos embebidas (integradas en la propia aplicación, sin necesidad de un servidor independiente):

- **db4o**: Diseñada específicamente para ser embebida en aplicaciones Java y .NET.
- **ObjectDB**: Puede funcionar tanto en modo servidor como embebido en aplicaciones Java.

Por el contrario, Versant Object Database y GemStone/S suelen funcionar como servidores independientes y no están pensados principalmente para uso embebido.

### Ventajas
- Persistencia transparente de objetos complejos.
- Mayor naturalidad en el modelado de datos para aplicaciones OO.
- Reducción de código de mapeo entre objetos y base de datos.
- Mejor rendimiento en operaciones sobre estructuras de datos complejas.

### Gestión de la persistencia de objetos
- Los objetos se almacenan y recuperan directamente desde la base de datos.
- Se mantiene la identidad y las relaciones entre objetos.

### Interfaz de programación de aplicaciones (API) y OQL
- APIs específicas permiten manipular objetos en la base de datos desde el lenguaje de programación.
- OQL (Object Query Language) es un lenguaje de consultas similar a SQL pero orientado a objetos.

### Gestión de transacciones
- Soporte para transacciones ACID (Atomicidad, Consistencia, Aislamiento, Durabilidad).
- Permite operaciones seguras y coherentes sobre objetos persistentes.

### Desarrollo de programas que gestionan objetos en bases de datos
- Los desarrolladores pueden crear, modificar y consultar objetos directamente.
- Se facilita la integración entre la lógica de negocio y la persistencia de datos.

### **ObjectDB**: Uso práctico y características

computer: Ejemplo

**Introducción a ObjectDB**
- ObjectDB es una base de datos orientada a objetos para Java, muy utilizada por su alto rendimiento y compatibilidad con los estándares JPA y JDO.

#### Características de ObjectDB

- Permite almacenar y gestionar objetos Java de forma nativa, sin necesidad de conversión a tablas relacionales.
- Cumple con los estándares JPA y JDO, facilitando la integración con frameworks y herramientas Java.
- Ofrece alto rendimiento en operaciones de persistencia y consulta.
- Soporta transacciones ACID, consultas JPQL y gestión eficiente de grandes volúmenes de datos.
- Puede funcionar tanto en modo embebido como en modo servidor.

**Instalación y configuración en un proyecto Maven**

1. **Añadir la dependencia de ObjectDB en el archivo `pom.xml`:**

En la página de [ObjectDB](https://www.objectdb.com/download)

```xml

For using the new jakarta.persistence packages:

<repositories>
			...
	<repository>
		<id>objectdb</id>
		<name>ObjectDB Repository</name>
		<url>https://m2.objectdb.com</url>
	</repository>
			...
</repositories>
			...
<dependencies>
			...
	<dependency>
		<groupId>com.objectdb</groupId>
		<artifactId>objectdb-jk</artifactId>
		<version>2.9.4</version><!-- Usa la última versión disponible -->
	</dependency>
			...
</dependencies>

```

2. **Actualizar el proyecto Maven** para descargar la dependencia.

3. **Configurar el archivo `persistence.xml`** en la carpeta `src/main/resources/META-INF`:

Crea el archivo `persistence.xml` con la información que figura a continuación:
* Versión 2.1 de persistencia
* Ahora el proveedor del servicio de persistencia es **objectdb** `provider` a
`<provider>com.objectdb.jpa.Provider</provider>`  
* La base de datos automáticamente al conectarte desde la aplicación java, la crea si no existe.
`<property name="jakarta.persistence.jdbc.url"`  el valor cambia
  * Si utilizas ObjectDB con servidor. Por ejemplo, la URL: `value="objectdb://localhost:6136/miBD.odb;create=true"`
  * Si utilizas ObjectDB embebido. Por ejemplo , la URL:  `value="$objectdb/db/miBD.odb"`

Finalmente el archivo queda utilizando **ObjectDB embebido**:

```xml
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence           http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd">
  <persistence-unit name="miUnidadPersistencia" transaction-type="RESOURCE_LOCAL">
    <provider>com.objectdb.jpa.Provider</provider>
    <properties>
      <property name="jakarta.persistence.jdbc.url" value="$objectdb/db/miBD.odb"/>
    </properties>
  </persistence-unit>
</persistence>
```

**Modelo de persistencia**
- Definición de entidades persistentes en Java usando anotaciones JPA.
- Ejemplo de clase Java anotada como entidad.

Ejemplo de clase Entity:

```java
// Nota: Recuerda añadir esta clase al archivo persistence.xml dentro de la etiqueta <class>.
import jakarta.persistence.*;

@Entity
public class Empleado {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	private String nombre;
	private int edad;
	// Getters y setters
}
```


Ejemplo de clase Embeddable:

```java
@Embeddable
public class Direccion {
	private String calle;
	private String ciudad;
}
```

**Operaciones básicas**
En una aplicación que va a usar la **API JPA** se va a hacer uso de los interfaces:

[conexionesJPA](img/conexionesJPA.jpg)
* EntityManagerFactory: Es una interface que, entre otras cosas, permite crear instancias de objetos `EntityManager`.
* EntityManager: Permite crear un contexto de persistencia, una conexión a una base de datos en la que se pueden persistir los datos.
* EntityTRansaction: Permite controlar transacciones dentro de un contexto de persistencia.
* Query: Permite realizar y controlar consultas sobre clases persistentes.

- Ejemplo de apertura y cierre de conexión (`EntityManager`).
- Ejemplo de inserción, consulta, actualización y borrado de objetos.
- Uso de transacciones.

Ejemplo de uso de EntityManager y transacciones:

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("miUnidadPersistencia");
try (emf) {
	EntityManager em = emf.createEntityManager();
	try {
		em.getTransaction().begin();
		Empleado emp = new Empleado();
		emp.setNombre("Ana");
		emp.setEdad(30);
		em.persist(emp); // Aunque este objeto, realmente quedará guardado cuando se confirme la transacción de la que forme parte esta operación. Mientras tanto, el objeto estará en un estado managed
		// Nota: El método persist() marca el objeto para ser guardado de forma persistente en la base de datos al hacer commit de la transacción.
		// Puede lanzar errores como:
		// - EntityExistsException: si el objeto ya existe en la base de datos.
		// - IllegalArgumentException: si el objeto no es una entidad o es nulo.
		// - TransactionRequiredException: si no hay una transacción activa.
		em.getTransaction().commit();
	} catch (Exception ex) {
		if (em.getTransaction().isActive()) {
			em.getTransaction().rollback();
		}
		ex.printStackTrace();
	} finally {
		em.close();
	}
}
```

Consulta de objetos:
* Para obtener los objetos de una Entity Class almacenados en la base de datos, podemos usar Query con lenguaje **JPQL** y ejecutar la query
* La sintaxis JPQL se basa en el lenguaje SQL.
* En las consultas, en los nombres de las clases y de los atributos de las clases se diferencian mayúsculas y minúsculas.
* La API JPA proporciona varias formas de obtener objetos de la BD.

```java
List<Empleado> empleados = em.createQuery("SELECT e FROM Empleado e", Empleado.class).getResultList();
```

Actualización de objetos:

```java
Empleado emp=em.find(Empleado.class,1);
em.getTransaction().begin();
emp.setEdad(31);
em.merge(emp);
em.getTransaction().commit();
```

Borrado de objetos:

```java
Empleado emp=em.find(Empleado.class,1);
em.getTransaction().begin();
em.remove(emp);
em.getTransaction().commit();
```


**Consultas**
- Ejemplo de consultas JPQL (Java Persistence Query Language) sobre entidades almacenadas en ObjectDB.

Ejemplo de consulta JPQL:

```java
TypedQuery<Empleado> consulta = em.createQuery("SELECT e FROM Empleado e WHERE e.edad > :edad", Empleado.class);
consulta.setParameter("edad", 25);
List<Empleado> resultado = consulta.getResultList();
```

Ejemplo de NamedQuery:

```java
@NamedQuery(name = "Empleado.findByDepartamento", query = "SELECT e FROM Empleado e WHERE e.departamento.id = :idDept")
```

Ejecución:

```java
TypedQuery<Empleado> consulta = em.createNamedQuery("Empleado.findByDepartamento", Empleado.class);
consulta.setParameter("idDept", 7);
List<Empleado> resultado = consulta.getResultList();
```

Ejemplo de consulta con JOIN:

```java
List<Object[]> datos = em.createQuery("SELECT e.nombre, d.nombre FROM Empleado e JOIN e.departamento d").getResultList();
```

Ejemplo de consulta de actualización:

```java
em.getTransaction().begin();
Query q = em.createQuery("UPDATE Empleado e SET e.edad = e.edad + 1 WHERE e.id > 7");
q.executeUpdate();
em.getTransaction().commit();
```

**Consultas con Criteria API**

* La API Criteria permite construir consultas de forma dinámica en tiempo de ejecución mediante el uso de métodos para filtrar, ordenar, etc...
* No requiere escribir y usar una consulta JPQL.
* Se parte de una selección de todas las instancias de una entidad para después ir filtrando mediante métodos esas instancias.
Para realizar con Criteria una consulta de todos los empleados, resultaría complejo ya que tendríamos que hacer:

```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Empleado> cq = cb.createQuery(Empleado.class);
Root<Empleado> root = cq.from(Empleado.class);
cq.select(root)
List<Empleado> resultado = em.createQuery(cq).getResultList();
```
* *CriteriaBuilder*: Es una clase Factory de CriteriaQuery. Se puede usar el mismo CriteriaBuilder para instanciar varios CriteriaQuery.
* *CriteriaQuery*: Permite representar el tipo de resultado de la consulta que se va a crear y usar. En el caso anterior una consulta para devolver objetos Empleado. A un CriteriaQuery se le podrán aplicar después varios modificadores. 
* *CriteriaQuery.from*: Aquí indicamos que entidad estamos consultando. También podríamos usar relaciones JOIN y subconsultas.
* *CriteriaQuery.select*: Para especificar los objetos o campos de objeto que queremos devolver.
* *CriteriaQuery.where*: Para establecer las condiciones de filtrado de la consulta.

Ejemplo de obtención de datos de empleados de más de 25 años

```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Empleado> cq = cb.createQuery(Empleado.class);
Root<Empleado> root = cq.from(Empleado.class);
cq.select(root).where(cb.gt(root.get("edad"), 25));
List<Empleado> resultado = em.createQuery(cq).getResultList();
```

**Ventajas de ObjectDB**
- Alto rendimiento
- Sencillez de uso.
- Integración nativa con Java y frameworks estándar.

**Conclusión**
- ObjectDB es una opción recomendada para la persistencia de objetos en aplicaciones Java, gracias a su compatibilidad con los principales estándares y su facilidad de integración.

### Herramientas para trabajar con ObjectDB: ObjectDB Explorer

**ObjectDB Explorer** es una herramienta gráfica que permite visualizar, consultar y modificar bases de datos ObjectDB (.odb) de forma sencilla y visual.

**Características principales:**
- Permite explorar las entidades y sus relaciones.
- Ejecutar consultas JPQL de manera interactiva.
- Editar, añadir o eliminar datos directamente desde la interfaz.
- Visualizar la estructura de la base de datos y los objetos almacenados.

**¿Cómo obtener y usar ObjectDB Explorer?**
1. Descarga ObjectDB desde la web oficial: https://www.objectdb.com/download
2. Descomprime el archivo descargado.
3. Dentro de la carpeta encontrarás el archivo `explorer.exe` (Windows) o `explorer.sh` (Linux/Mac).
4. Ejecuta el explorador y abre tu archivo de base de datos `.odb` para comenzar a explorar y modificar los datos.
5. Manual de referencia https://www.objectdb.com/java/jpa/tool/explorer

**Nota:** ObjectDB Explorer no se instala automáticamente con las dependencias de Maven; es una herramienta independiente que debe descargarse aparte.

Esta utilidad es especialmente útil para depuración, pruebas y aprendizaje, ya que permite ver el contenido real de la base de datos y realizar operaciones sin necesidad de programar.

