### 📋 IMPORTANTE: Patrón de Gestión de Conexiones (Metodología de Clase)

**La metodología establecida en los ejercicios del Tema 2**. 
Se requiere:

#### 1. **Clase Resultado<T>** (Generic Ok/Fail Pattern)

Crea una clase genérica para envolver resultados y evitar propagación de excepciones:

```java
public class Resultado<T> {
    private boolean ok;
    private T data;
    private String message;
    
    private Resultado(boolean ok, T data, String message) {
        this.ok = ok;
        this.data = data;
        this.message = message;
    }
    
    public static <T> Resultado<T> ok(T data) {
        return new Resultado<>(true, data, null);
    }
    
    public static <T> Resultado<T> fail(String message) {
        return new Resultado<>(false, null, message);
    }
    
    public boolean isOk() { return ok; }
    public T getData() { return data; }
    public String getMessage() { return message; }
}
```

#### 2. **Clases DAO (Data Access Object)**

**Patrón requerido:**
- ✅ Constructor recibe `Connection` (inyección de dependencia)
- ✅ **NO lanzan SQLException** (captura interna con Logger)
- ✅ Retornan valores seguros: listas vacías, `Optional.empty()`, `false`, `-1`
- ✅ NO abren ni cierran conexiones

**Ejemplo: Sistema de Biblioteca**

```java
public class LibroDAO {
    private final Connection conn;
    private static final Logger LOG = Logger.getLogger(LibroDAO.class.getName());
    
    // ✅ CORRECTO - Constructor recibe Connection
    public LibroDAO(Connection conn) {
        this.conn = conn;
    }
    
    // ✅ CORRECTO - NO lanza SQLException, retorna lista vacía si hay error
    public List<Libro> listarDisponibles() {
        String sql = "SELECT * FROM libros WHERE disponible = true";
        
        try (PreparedStatement pstmt = conn.prepareStatement(sql);
             ResultSet rs = pstmt.executeQuery()) {
            
            List<Libro> libros = new ArrayList<>();
            while (rs.next()) {
                Libro libro = new Libro();
                libro.setIdLibro(rs.getInt("id_libro"));
                libro.setTitulo(rs.getString("titulo"));
                libro.setAutor(rs.getString("autor"));
                libro.setIsbn(rs.getString("isbn"));
                libro.setDisponible(rs.getBoolean("disponible"));
                libros.add(libro);
            }
            return libros;
            
        } catch (SQLException e) {
            LOG.log(Level.SEVERE, "Error en LibroDAO.listarDisponibles", e);
            return Collections.emptyList(); // Valor seguro
        }
    }
    
    // ✅ CORRECTO - Retorna Optional.empty() si no existe o hay error
    public Optional<Libro> buscarPorId(int id) {
        String sql = "SELECT * FROM libros WHERE id_libro = ?";
        
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            try (ResultSet rs = pstmt.executeQuery()) {
                if (rs.next()) {
                    Libro libro = new Libro();
                    libro.setIdLibro(rs.getInt("id_libro"));
                    libro.setTitulo(rs.getString("titulo"));
                    libro.setAutor(rs.getString("autor"));
                    return Optional.of(libro);
                }
            }
        } catch (SQLException e) {
            LOG.log(Level.SEVERE, "Error en LibroDAO.buscarPorId", e);
        }
        return Optional.empty();
    }
    
    // ✅ CORRECTO - Retorna boolean, false si hay error
    public boolean actualizarDisponibilidad(int idLibro, boolean disponible) {
        String sql = "UPDATE libros SET disponible = ? WHERE id_libro = ?";
        
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setBoolean(1, disponible);
            pstmt.setInt(2, idLibro);
            return pstmt.executeUpdate() > 0;
        } catch (SQLException e) {
            LOG.log(Level.SEVERE, "Error en LibroDAO.actualizarDisponibilidad", e);
            return false;
        }
    }
}

// ❌ INCORRECTO - DAO NO debe abrir conexiones
public List<Libro> listarDisponibles() {
    try (Connection conn = ConexionBD.obtenerConexion(); ...) {
        // NO hacer esto
    }
}

// ❌ INCORRECTO - DAO NO debe lanzar SQLException
public List<Libro> listarDisponibles() throws SQLException {
    // NO hacer esto
}
```

#### 3. **Capa de Servicio**

**Patrón requerido:**
- ✅ Obtiene `Connection` con `ConexionBD.obtenerConexion()`
- ✅ Usa **try-with-resources** para cierre automático
- ✅ Crea DAOs inyectando Connection en constructor
- ✅ **Retorna `Resultado<T>`** (nunca lanza SQLException)
- ✅ Gestiona transacciones: `setAutoCommit(false)`, `commit()`

**Ejemplo: Sistema de Biblioteca**

```java
public class PrestamoServicio {
    private static final Logger LOG = Logger.getLogger(PrestamoServicio.class.getName());
    
    // ✅ CORRECTO - Servicio gestiona conexión y retorna Resultado<T>
    public Resultado<Integer> crearPrestamo(int idUsuario, int idLibro) {
        Connection conn = ConexionBD.obtenerConexion();
        
        if (conn == null) {
            return Resultado.fail("No se pudo obtener conexión");
        }
        
        try (Connection c = conn) {
            c.setAutoCommit(false);
            
            // Crear DAOs con constructor injection
            PrestamoDAO prestamoDAO = new PrestamoDAO(c);
            LibroDAO libroDAO = new LibroDAO(c);
            
            // Verificar disponibilidad
            Optional<Libro> libroOpt = libroDAO.buscarPorId(idLibro);
            if (libroOpt.isEmpty()) {
                c.rollback();
                return Resultado.fail("Libro no encontrado");
            }
            
            if (!libroOpt.get().isDisponible()) {
                c.rollback();
                return Resultado.fail("Libro no disponible");
            }
            
            // Crear préstamo
            Prestamo prestamo = new Prestamo();
            prestamo.setIdUsuario(idUsuario);
            prestamo.setIdLibro(idLibro);
            prestamo.setFechaPrestamo(LocalDate.now());
            
            int id = prestamoDAO.insertar(prestamo);
            if (id == -1) {
                c.rollback();
                return Resultado.fail("Error al crear préstamo");
            }
            
            // Marcar libro como no disponible
            boolean actualizado = libroDAO.actualizarDisponibilidad(idLibro, false);
            if (!actualizado) {
                c.rollback();
                return Resultado.fail("Error al actualizar disponibilidad");
            }
            
            c.commit();
            return Resultado.ok(id);
            
        } catch (SQLException e) {
            LOG.log(Level.SEVERE, "Error en crearPrestamo", e);
            return Resultado.fail("Error: " + e.getMessage());
        }
    }
    
    // ✅ CORRECTO - Devolución con transacción
    public Resultado<Boolean> devolverLibro(int idPrestamo) {
        Connection conn = ConexionBD.obtenerConexion();
        
        if (conn == null) {
            return Resultado.fail("No se pudo obtener conexión");
        }
        
        try (Connection c = conn) {
            c.setAutoCommit(false);
            
            PrestamoDAO prestamoDAO = new PrestamoDAO(c);
            LibroDAO libroDAO = new LibroDAO(c);
            
            // Obtener préstamo
            Optional<Prestamo> prestamoOpt = prestamoDAO.buscarPorId(idPrestamo);
            if (prestamoOpt.isEmpty()) {
                c.rollback();
                return Resultado.fail("Préstamo no encontrado");
            }
            
            Prestamo prestamo = prestamoOpt.get();
            
            // Marcar préstamo como devuelto
            prestamo.setFechaDevolucion(LocalDate.now());
            boolean actualizado = prestamoDAO.actualizarDevolucion(idPrestamo);
            if (!actualizado) {
                c.rollback();
                return Resultado.fail("Error al actualizar préstamo");
            }
            
            // Marcar libro como disponible
            actualizado = libroDAO.actualizarDisponibilidad(prestamo.getIdLibro(), true);
            if (!actualizado) {
                c.rollback();
                return Resultado.fail("Error al liberar libro");
            }
            
            c.commit();
            return Resultado.ok(true);
            
        } catch (SQLException e) {
            LOG.log(Level.SEVERE, "Error en devolverLibro", e);
            return Resultado.fail("Error: " + e.getMessage());
        }
    }
}

// ❌ INCORRECTO - Servicio NO debe lanzar SQLException
public int crearPrestamo(...) throws SQLException {
    // NO hacer esto
}

// ❌ INCORRECTO - Servicio NO debe retornar primitivos/null
public int crearPrestamo(...) {
    return -1; // NO hacer esto, usar Resultado<Integer>
}
```

#### 4. **Controlador**

**Patrón requerido:**
- ✅ Para operaciones simples: obtiene Connection, crea DAO con constructor
- ✅ Para operaciones complejas: llama a Servicio y trabaja con `Resultado<T>`
- ✅ Verifica `resultado.isOk()` antes de obtener datos

**Ejemplo: Sistema de Biblioteca**

```java
public class Controlador {
    private Vista vista;
    private PrestamoServicio prestamoServicio;
    
    // ✅ CORRECTO - Operación simple: obtiene conexión, crea DAO
    public void listarLibrosDisponibles() {
        Connection conn = ConexionBD.obtenerConexion();
        
        if (conn == null) {
            vista.mostrarError("No hay conexión");
            return;
        }
        
        try (Connection c = conn) {
            LibroDAO libroDAO = new LibroDAO(c);
            List<Libro> libros = libroDAO.listarDisponibles();
            vista.mostrarLibros(libros);
        } catch (SQLException e) {
            vista.mostrarError("Error al cerrar conexión");
        }
    }
    
    // ✅ CORRECTO - Operación compleja: delega en Servicio, trabaja con Resultado<T>
    public void realizarPrestamo() {
        int idUsuario = vista.solicitarIdUsuario();
        int idLibro = vista.solicitarIdLibro();
        
        Resultado<Integer> resultado = prestamoServicio.crearPrestamo(idUsuario, idLibro);
        
        if (resultado.isOk()) {
            int id = resultado.getData();
            vista.mostrarExito("Préstamo creado con ID: " + id);
        } else {
            vista.mostrarError(resultado.getMessage());
        }
    }
    
    // ✅ CORRECTO - Devolución de libro
    public void devolverLibro() {
        int idPrestamo = vista.solicitarIdPrestamo();
        
        Resultado<Boolean> resultado = prestamoServicio.devolverLibro(idPrestamo);
        
        if (resultado.isOk()) {
            vista.mostrarExito("Libro devuelto correctamente");
        } else {
            vista.mostrarError(resultado.getMessage());
        }
    }
}

// ❌ INCORRECTO - Controlador NO debe mantener instancias de DAO
private LibroDAO libroDAO = new LibroDAO(); // NO hacer esto
```

#### **Resumen del flujo:**

```
┌─────────────┐
│ Controlador │ obtiene Connection → crea DAO(conn) → llama métodos
└─────────────┘         ↓
                try-with-resources cierra automáticamente

┌──────────┐
│ Servicio │ obtiene Connection → crea DAOs(conn) → retorna Resultado<T>
└──────────┘         ↓
         try-with-resources + transacciones

┌─────┐
│ DAO │ recibe Connection en constructor → captura SQLException → retorna valores seguros
└─────┘    NO abre/cierra conexiones
```

#### **Ventajas de este patrón:**
- ✅ Sin propagación de SQLException
- ✅ Clean error handling con Resultado<T>
- ✅ Constructor injection (testeable)
- ✅ Cierre automático con try-with-resources
- ✅ Rollback automático si no hay commit
- ✅ Logging centralizado
- ✅ Single Responsibility por capa

---