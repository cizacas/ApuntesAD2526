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
  
	**Cumple con los siguientes estándares:**
	- **ODMG (Object Data Management Group)**: Proporciona un modelo estándar para bases de datos orientadas a objetos.
	- **JPA (Java Persistence API)**: Permite la persistencia de objetos Java de forma estándar y portable.
	- **JDO (Java Data Object)**: Define una interfaz estándar para la persistencia y recuperación de objetos Java.

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

