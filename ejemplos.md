## OBTENER METADATOS DE LA BASE DE DATOS
Mediante la clase **DatabaseMetaData** podemos obtener metadatos de la base de datos. Los metadatos son datos sobre la conexión establecida con la bases de datos, sobre la estructura de la base de datos (tablas que tiene) y sobre cada tabla de la base de datos (nombres de columnas, tipos)
* Para obtener un objeto de la clase DatabaseMetaData se utiliza el método `getMetaData()` del objeto Connection.

### Ejemplo: Recoger información sobre la conexión

```java
import java.sql.*;

public class EjemploMetadatos {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:ejemplo.db";
        
        try (Connection conexion = DriverManager.getConnection(url)) {
            // Obtener el objeto DatabaseMetaData
            DatabaseMetaData metadatos = conexion.getMetaData();
            
            System.out.println("=== INFORMACIÓN DE LA CONEXIÓN ===");
            
            // Información básica de la base de datos
            System.out.println("Nombre del producto de BD: " + metadatos.getDatabaseProductName());
            System.out.println("Versión del producto: " + metadatos.getDatabaseProductVersion());
            System.out.println("Versión mayor: " + metadatos.getDatabaseMajorVersion());
            System.out.println("Versión menor: " + metadatos.getDatabaseMinorVersion());
            
            // Información del driver
            System.out.println("\n=== INFORMACIÓN DEL DRIVER ===");
            System.out.println("Nombre del driver: " + metadatos.getDriverName());
            System.out.println("Versión del driver: " + metadatos.getDriverVersion());
            System.out.println("Versión mayor del driver: " + metadatos.getDriverMajorVersion());
            System.out.println("Versión menor del driver: " + metadatos.getDriverMinorVersion());
            
            // Información de la URL y usuario
            System.out.println("\n=== INFORMACIÓN DE CONEXIÓN ===");
            System.out.println("URL de conexión: " + metadatos.getURL());
            System.out.println("Nombre de usuario: " + metadatos.getUserName());
            
            // Capacidades de la base de datos
            System.out.println("\n=== CAPACIDADES DE LA BASE DE DATOS ===");
            System.out.println("Soporta transacciones: " + metadatos.supportsTransactions());
            System.out.println("Soporta procedimientos almacenados: " + metadatos.supportsStoredProcedures());
            System.out.println("Soporta subconsultas: " + metadatos.supportsSubqueriesInExists());
            System.out.println("Es de solo lectura: " + metadatos.isReadOnly());
            
            // Información sobre límites
            System.out.println("\n=== LÍMITES DE LA BASE DE DATOS ===");
            System.out.println("Máximo de conexiones: " + metadatos.getMaxConnections());
            System.out.println("Máximo de columnas en SELECT: " + metadatos.getMaxColumnsInSelect());
            System.out.println("Máximo de tablas en SELECT: " + metadatos.getMaxTablesInSelect());
            System.out.println("Longitud máxima de nombre de tabla: " + metadatos.getMaxTableNameLength());
            
            // Separadores y caracteres especiales
            System.out.println("\n=== SEPARADORES Y CARACTERES ===");
            System.out.println("Separador de catálogo: '" + metadatos.getCatalogSeparator() + "'");
            System.out.println("Término de catálogo: " + metadatos.getCatalogTerm());
            System.out.println("Término de esquema: " + metadatos.getSchemaTerm());
            System.out.println("Término de procedimiento: " + metadatos.getProcedureTerm());
            System.out.println("Caracteres extra para nombres: " + metadatos.getExtraNameCharacters());
            
        } catch (SQLException e) {
            System.err.println("Error al obtener metadatos: " + e.getMessage());
        }
    }
}
```

### Métodos más útiles de DatabaseMetaData para información de conexión:

| Método | Descripción |
|--------|-------------|
| `getDatabaseProductName()` | Nombre del producto de base de datos |
| `getDatabaseProductVersion()` | Versión del producto de base de datos |
| `getDriverName()` | Nombre del driver JDBC |
| `getDriverVersion()` | Versión del driver JDBC |
| `getURL()` | URL de conexión utilizada |
| `getUserName()` | Nombre del usuario conectado |
| `supportsTransactions()` | Si soporta transacciones |
| `isReadOnly()` | Si la base de datos es de solo lectura |
| `getMaxConnections()` | Número máximo de conexiones simultáneas |

### Obtener información sobre tablas y vistas

Suponiendo que tenemos un objeto `dbmd` de la clase `DatabaseMetaData`, podemos obtener información sobre las tablas y vistas de la base de datos:

```java
import java.sql.*;

public class EjemploMetadatosTablas {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:ejemplo.db";
        
        try (Connection conexion = DriverManager.getConnection(url)) {
            DatabaseMetaData dbmd = conexion.getMetaData();
            
            // 1. OBTENER TODAS LAS TABLAS Y VISTAS
            System.out.println("=== TABLAS Y VISTAS EN LA BASE DE DATOS ===");
            String[] tipos = {"TABLE", "VIEW"};
            try (ResultSet tablas = dbmd.getTables(null, null, null, tipos)) {
                while (tablas.next()) {
                    String nombreTabla = tablas.getString("TABLE_NAME");
                    String tipoTabla = tablas.getString("TABLE_TYPE");
                    String esquema = tablas.getString("TABLE_SCHEM");
                    String comentarios = tablas.getString("REMARKS");
                    
                    System.out.println("Tabla: " + nombreTabla + 
                                     " | Tipo: " + tipoTabla + 
                                     " | Esquema: " + esquema + 
                                     " | Comentarios: " + comentarios);
                }
            }
            
            // 2. INFORMACIÓN ESPECÍFICA DE LA TABLA "productos"
            System.out.println("\n=== INFORMACIÓN DE LA TABLA 'productos' ===");
            
            // Obtener columnas de la tabla productos
            System.out.println("\nColumnas de la tabla 'productos':");
            try (ResultSet columnas = dbmd.getColumns(null, null, "productos", null)) {
                while (columnas.next()) {
                    String nombreColumna = columnas.getString("COLUMN_NAME");
                    String tipoColumna = columnas.getString("TYPE_NAME");
                    int tamanoColumna = columnas.getInt("COLUMN_SIZE");
                    String nullable = columnas.getString("IS_NULLABLE");
                    String valorDefecto = columnas.getString("COLUMN_DEF");
                    int posicion = columnas.getInt("ORDINAL_POSITION");
                    
                    System.out.println("  " + posicion + ". " + nombreColumna + 
                                     " (" + tipoColumna + 
                                     (tamanoColumna > 0 ? "(" + tamanoColumna + ")" : "") + ")" +
                                     " - Nullable: " + nullable +
                                     (valorDefecto != null ? " - Default: " + valorDefecto : ""));
                }
            }
            
            // 3. OBTENER CLAVES PRIMARIAS
            System.out.println("\nClaves primarias de 'productos':");
            try (ResultSet clavesPrimarias = dbmd.getPrimaryKeys(null, null, "productos")) {
                while (clavesPrimarias.next()) {
                    String nombreColumna = clavesPrimarias.getString("COLUMN_NAME");
                    String nombreClave = clavesPrimarias.getString("PK_NAME");
                    short secuencia = clavesPrimarias.getShort("KEY_SEQ");
                    
                    System.out.println("  - Columna: " + nombreColumna + 
                                     " | Nombre PK: " + nombreClave + 
                                     " | Secuencia: " + secuencia);
                }
            }
            
            // 4. OBTENER CLAVES FORÁNEAS (IMPORTADAS)
            System.out.println("\nClaves foráneas importadas por 'productos':");
            try (ResultSet clavesForaneas = dbmd.getImportedKeys(null, null, "productos")) {
                while (clavesForaneas.next()) {
                    String tablaFK = clavesForaneas.getString("FKTABLE_NAME");
                    String columnaFK = clavesForaneas.getString("FKCOLUMN_NAME");
                    String tablaPK = clavesForaneas.getString("PKTABLE_NAME");
                    String columnaPK = clavesForaneas.getString("PKCOLUMN_NAME");
                    String nombreFK = clavesForaneas.getString("FK_NAME");
                    
                    System.out.println("  - " + tablaFK + "." + columnaFK + 
                                     " -> " + tablaPK + "." + columnaPK + 
                                     " (FK: " + nombreFK + ")");
                }
            }
            
            // 5. OBTENER ÍNDICES
            System.out.println("\nÍndices de la tabla 'productos':");
            try (ResultSet indices = dbmd.getIndexInfo(null, null, "productos", false, false)) {
                while (indices.next()) {
                    String nombreIndice = indices.getString("INDEX_NAME");
                    String nombreColumna = indices.getString("COLUMN_NAME");
                    boolean unico = !indices.getBoolean("NON_UNIQUE");
                    short tipo = indices.getShort("TYPE");
                    short posicion = indices.getShort("ORDINAL_POSITION");
                    
                    if (nombreIndice != null) {
                        System.out.println("  - Índice: " + nombreIndice + 
                                         " | Columna: " + nombreColumna + 
                                         " | Único: " + unico + 
                                         " | Posición: " + posicion);
                    }
                }
            }
            
        } catch (SQLException e) {
            System.err.println("Error al obtener metadatos de tablas: " + e.getMessage());
        }
    }
}
```

### Métodos principales de DatabaseMetaData para tablas:

| Método | Descripción |
|--------|-------------|
| `getTables(catalog, schema, tableNamePattern, types)` | Obtiene lista de tablas/vistas |
| `getColumns(catalog, schema, table, columnNamePattern)` | Obtiene columnas de una tabla |
| `getPrimaryKeys(catalog, schema, table)` | Obtiene claves primarias |
| `getImportedKeys(catalog, schema, table)` | Obtiene claves foráneas importadas |
| `getExportedKeys(catalog, schema, table)` | Obtiene claves foráneas exportadas |
| `getIndexInfo(catalog, schema, table, unique, approximate)` | Obtiene información de índices |

**Nota**: En SQLite, los parámetros `catalog` y `schema` suelen ser `null`.

### Obtener información de los resultados de una consulta
Cuando tenemos un objeto `ResultSet` con los resultados de una consulta, podemos obtener metadatos sobre esos resultados mediante el objeto `ResultSetMetaData`.
* Para obtener un objeto de la clase `ResultSetMetaData` se utiliza el método `getMetaData()` del objeto `ResultSet`.

```java
import java.sql.*;
public class EjemploResultSetMetaData {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:ejemplo.db";
        
        try (Connection conexion = DriverManager.getConnection(url);
             Statement stmt = conexion.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM productos")) {
             
            // Obtener el objeto ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            
            // Número de columnas en el ResultSet
            int numeroColumnas = rsmd.getColumnCount();
            System.out.println("Número de columnas: " + numeroColumnas);
            
            // Información de cada columna
            for (int i = 1; i <= numeroColumnas; i++) {
                String nombreColumna = rsmd.getColumnName(i);
                String tipoColumna = rsmd.getColumnTypeName(i);
                int tamanoColumna = rsmd.getColumnDisplaySize(i);
                boolean esNullable = rsmd.isNullable(i) == ResultSetMetaData.columnNullable;
                
                System.out.println("Columna " + i + ": " + nombreColumna + 
                                 " | Tipo: " + tipoColumna + 
                                 " | Tamaño: " + tamanoColumna + 
                                 " | Nullable: " + esNullable);
            }
            
        } catch (SQLException e) {
            System.err.println("Error al obtener metadatos del ResultSet: " + e.getMessage());
        }
    }
}
```
### Métodos principales de ResultSetMetaData:
| Método | Descripción |
|--------|-------------|
| `getColumnCount()` | Devuelve el número de columnas en el ResultSet |
| `getColumnName(int column)` | Devuelve el nombre de la columna especificada (1-based) |
| `getColumnTypeName(int column)` | Devuelve el tipo SQL de la columna especificada |
| `getColumnDisplaySize(int column)` | Devuelve el tamaño máximo de la columna especificada |



#### 3. Procedimiento más complejo: Insertar producto con validaciones

```sql
-- Procedimiento para insertar un nuevo producto con validaciones
DELIMITER $$
CREATE PROCEDURE InsertarProducto(
    IN p_nombre VARCHAR(40),
    IN p_cantidad INT,
    OUT p_id_generado INT,
    OUT p_resultado VARCHAR(100)
)
BEGIN
    DECLARE v_existe INT DEFAULT 0;
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SET p_resultado = 'Error al insertar producto';
        SET p_id_generado = -1;
    END;
    
    START TRANSACTION;
    
    -- Verificar si el producto ya existe
    SELECT COUNT(*) INTO v_existe 
    FROM productos 
    WHERE nombre = p_nombre;
    
    IF v_existe > 0 THEN
        SET p_resultado = 'El producto ya existe';
        SET p_id_generado = -1;
    ELSEIF p_cantidad < 0 THEN
        SET p_resultado = 'La cantidad no puede ser negativa';
        SET p_id_generado = -1;
    ELSE
        -- Insertar el producto
        INSERT INTO productos (nombre, cantidad) 
        VALUES (p_nombre, p_cantidad);
        
        -- Obtener el ID generado
        SET p_id_generado = LAST_INSERT_ID();
        SET p_resultado = 'Producto insertado correctamente';
        
        COMMIT;
    END IF;
END$$
DELIMITER ;
```

#### 4. Llamada al procedimiento de inserción desde Java

```java
public static void insertarProducto(String nombre, int cantidad) {
    String url = "jdbc:mysql://localhost:3306/empresa";
    String usuario = "root";
    String contraseña = "password";
    
    String sql = "{CALL InsertarProducto(?, ?, ?, ?)}";
    
    try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
         CallableStatement cstmt = conexion.prepareCall(sql)) {
         
        // Parámetros IN
        cstmt.setString(1, nombre);
        cstmt.setInt(2, cantidad);
        
        // Parámetros OUT
        cstmt.registerOutParameter(3, Types.INTEGER); // p_id_generado
        cstmt.registerOutParameter(4, Types.VARCHAR); // p_resultado
        
        // Ejecutar
        cstmt.execute();
        
        // Obtener resultados
        int idGenerado = cstmt.getInt(3);
        String resultado = cstmt.getString(4);
        
        System.out.println("Resultado: " + resultado);
        if (idGenerado > 0) {
            System.out.println("ID del producto creado: " + idGenerado);
        }
        
    } catch (SQLException e) {
        System.err.println("Error: " + e.getMessage());
    }
}
```
