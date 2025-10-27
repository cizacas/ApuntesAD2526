Proyecto de ejemplo: Hibernate + H2 (in-memory)

Este proyecto (carpeta raíz del repositorio) contiene un ejemplo mínimo para demostrar instalación, configuración y uso de Hibernate en Java.

Cómo compilar y ejecutar (PowerShell):

```powershell
mvn -DskipTests=false clean package
mvn exec:java -Dexec.mainClass="com.example.Main"
```

Descripción rápida:
- `pom.xml`: dependencias (Hibernate, H2, SLF4J)
- `src/main/resources/hibernate.cfg.xml`: configuración de Hibernate usando H2 en memoria
- `src/main/java/com/example/model`: entidades `Persona`, `Direccion`, `Telefono`
- `src/main/java/com/example/service`: `PersonaService` con operaciones CRUD
- `src/main/java/com/example/Main.java`: demo que persiste, consulta y borra entidades

Notas:
- La configuración usa `hbm2ddl.auto=create-drop` para crear el esquema al iniciar y eliminarlo al cerrar; solo para pruebas.
- Para usar MySQL u otra BD, cambia `hibernate.cfg.xml` y añade el driver en `pom.xml`.
