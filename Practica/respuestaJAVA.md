# Respuesta Practica

### 1. **Modelos (DTOs)**

```java
// Notificación DTO
public class NotificacionDTO {
    private String id;
    private String titulo;
    private String contenido;
    private List<String> destinatarios;
    private String estado; // Estado de la notificación (Ej: "Enviada", "Pendiente")

    // Getters y Setters
    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getTitulo() {
        return titulo;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }

    public String getContenido() {
        return contenido;
    }

    public void setContenido(String contenido) {
        this.contenido = contenido;
    }

    public List<String> getDestinatarios() {
        return destinatarios;
    }

    public void setDestinatarios(List<String> destinatarios) {
        this.destinatarios = destinatarios;
    }

    public String getEstado() {
        return estado;
    }

    public void setEstado(String estado) {
        this.estado = estado;
    }
}
```

---

### 2. **Comandos (Commands)**

```java
// Comando para crear una notificación
public class CrearNotificacionCommand {
    public static class Command {
        private String titulo;
        private String contenido;
        private List<String> destinatarios;

        public Command(String titulo, String contenido, List<String> destinatarios) {
            this.titulo = titulo;
            this.contenido = contenido;
            this.destinatarios = destinatarios;
        }

        public String getTitulo() {
            return titulo;
        }

        public String getContenido() {
            return contenido;
        }

        public List<String> getDestinatarios() {
            return destinatarios;
        }
    }

    public interface Result {}

    public static class Success implements Result {
        private String id;

        public Success(String id) {
            this.id = id;
        }

        public String getId() {
            return id;
        }
    }

    public static class InvalidInput implements Result {
        private String errorMessage;

        public InvalidInput(String errorMessage) {
            this.errorMessage = errorMessage;
        }

        public String getErrorMessage() {
            return errorMessage;
        }
    }

    public static class Unauthorized implements Result {
        private String userId;

        public Unauthorized(String userId) {
            this.userId = userId;
        }

        public String getUserId() {
            return userId;
        }
    }

    public Result handle(Command command) {
        // Lógica para crear notificación
        // Verificar datos de entrada y permisos de usuario
        if (command.getTitulo().isEmpty() || command.getContenido().isEmpty()) {
            return new InvalidInput("El título o contenido no puede estar vacío");
        }

        // Crear notificación y devolver el ID
        String notificacionId = "12345"; // Simulación de ID generado
        return new Success(notificacionId);
    }
}
```

---

### 3. **Handlers**

```java
// Handler para la creación de notificación
public class CrearNotificacionCommandHandler {
    private CrearNotificacionCommand command;

    public CrearNotificacionCommandHandler(CrearNotificacionCommand command) {
        this.command = command;
    }

    public CrearNotificacionCommand.Result handle(CrearNotificacionCommand.Command cmd) {
        // Llamar al comando de creación de notificación
        return command.handle(cmd);
    }
}
```

---

### 4. **Consultas (Queries)**

```java
// Consulta para obtener notificaciones
public class ObtenerNotificacionesQuery {
    public static class Query {
        private String estado;

        public Query(String estado) {
            this.estado = estado;
        }

        public String getEstado() {
            return estado;
        }
    }

    public static class Result {
        private List<NotificacionDTO> notificaciones;

        public Result(List<NotificacionDTO> notificaciones) {
            this.notificaciones = notificaciones;
        }

        public List<NotificacionDTO> getNotificaciones() {
            return notificaciones;
        }
    }

    public Result handle(Query query) {
        // Lógica para obtener las notificaciones basadas en el estado
        List<NotificacionDTO> notificaciones = new ArrayList<>();
        // Agregar algunas notificaciones ficticias a la lista
        notificaciones.add(new NotificacionDTO());
        return new Result(notificaciones);
    }
}
```

---

### 5. **Pruebas Unitarias**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

// Prueba para la creación de una notificación
public class CrearNotificacionCommandHandlerTest {

    @Test
    public void testCrearNotificacionExitosamente() {
        // Arrange
        CrearNotificacionCommand command = new CrearNotificacionCommand();
        CrearNotificacionCommandHandler handler = new CrearNotificacionCommandHandler(command);

        CrearNotificacionCommand.Command cmd = new CrearNotificacionCommand.Command(
            "Notificación Importante", 
            "Este es un mensaje importante.", 
            Arrays.asList("user1@dominio.com", "user2@dominio.com")
        );

        // Act
        CrearNotificacionCommand.Result result = handler.handle(cmd);

        // Assert
        assertTrue(result instanceof CrearNotificacionCommand.Success);
        assertNotNull(((CrearNotificacionCommand.Success) result).getId());
    }

    @Test
    public void testCrearNotificacionConEntradaInvalida() {
        // Arrange
        CrearNotificacionCommand command = new CrearNotificacionCommand();
        CrearNotificacionCommandHandler handler = new CrearNotificacionCommandHandler(command);

        CrearNotificacionCommand.Command cmd = new CrearNotificacionCommand.Command(
            "",  // Título vacío, entrada inválida
            "Este es un mensaje importante.", 
            Arrays.asList("user1@dominio.com")
        );

        // Act
        CrearNotificacionCommand.Result result = handler.handle(cmd);

        // Assert
        assertTrue(result instanceof CrearNotificacionCommand.InvalidInput);
        assertEquals("El título o contenido no puede estar vacío", ((CrearNotificacionCommand.InvalidInput) result).getErrorMessage());
    }
}
```

---

### Explicación de la poronga:

1. **Modelos (DTOs):** La clase `NotificacionDTO` representa la estructura de los datos de la notificación.
2. **Comandos (Commands):** La clase `CrearNotificacionCommand` maneja la creación de nuevas notificaciones. Contiene una clase interna `Command` para pasar los datos de entrada y las clases de resultado `Success`, `InvalidInput`, y `Unauthorized` para manejar los diferentes resultados.
3. **Handlers:** El `CrearNotificacionCommandHandler` es el encargado de recibir el comando y manejar la lógica de negocio asociada.
4. **Consultas (Queries):** `ObtenerNotificacionesQuery` permite obtener las notificaciones en función de su estado.
5. **Pruebas Unitarias:** Las pruebas usan **JUnit** para verificar el comportamiento del sistema, como la creación de notificaciones y el manejo de entradas inválidas.

---
