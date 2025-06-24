## Solo vamos a hacer el codigo de nuevo pero en **Java**

### Estructura de Carpetas Sugerida:

```
/src
 ├── /main
 │    ├── /java
 │    │    └── /com
 │    │         └── /examen
 │    │             ├── /api
 │    │             │    ├── /commands
 │    │             │    │    ├── RedactarMensajeCommand.java
 │    │             │    │    └── AprobarMensajeCommand.java
 │    │             │    ├── /queries
 │    │             │    │    ├── ObtenerMensajesQuery.java
 │    │             │    │    └── ObtenerMensajesPendientesQuery.java
 │    │             │    ├── /handlers
 │    │             │    │    ├── RedactarMensajeCommandHandler.java
 │    │             │    │    ├── AprobarMensajeCommandHandler.java
 │    │             │    │    ├── ObtenerMensajesQueryHandler.java
 │    │             │    │    └── ObtenerMensajesPendientesQueryHandler.java
 │    │             │    └── /models
 │    │             │         ├── MensajeDTO.java
 │    │             │         ├── MensajePendienteDTO.java
 │    │             │         └── Result.java
 │    │             ├── /config
 │    │             │    └── AppConfig.java
 │    │             ├── /services
 │    │             │    └── MensajeService.java
 │    │             └── /utils
 │    │                  └── AuthorizationUtils.java
 │    ├── /resources
 │    │    └── application.properties
 │    └── /test
 │         ├── /java
 │         │    └── /com
 │         │         └── /examen
 │         │             └── /api
 │         │                 ├── /commands
 │         │                 │    ├── RedactarMensajeCommandHandlerTest.java
 │         │                 │    └── AprobarMensajeCommandHandlerTest.java
 │         │                 ├── /queries
 │         │                 │    ├── ObtenerMensajesQueryHandlerTest.java
 │         │                 │    └── ObtenerMensajesPendientesQueryHandlerTest.java
 │         │                 └── /services
 │         │                      └── MensajeServiceTest.java
 │         └── /resources
 └── /target
      └── (archivos generados por la compilación)
```
---
 Historia de Usuario: Redactar Mensajes
---
```java


// 1. Command Handler: RedactarMensajeCommandHand

public interface RedactarMensajeCommand {
    class Command {
        String titulo;
        String contenido;
        List<String> destinatarios;

        public Command(String titulo, String contenido, List<String> destinatarios) {
            this.titulo = titulo;
            this.contenido = contenido;
            this.destinatarios = destinatarios;
        }
    }

    interface Result {}

    class Success implements Result {
        String mensajeId;

        public Success(String mensajeId) {
            this.mensajeId = mensajeId;
        }
    }

    class InvalidInput implements Result {
        String errorMessage;

        public InvalidInput(String errorMessage) {
            this.errorMessage = errorMessage;
        }
    }

    class Unauthorized implements Result {
        String userId;

        public Unauthorized(String userId) {
            this.userId = userId;
        }
    }

    Result handle(Command command);
}
```
---
```java
// 2. Query Handler: ObtenerMensajesQueryHandler

public interface ObtenerMensajesQuery {
    class Query {
        String estado;

        public Query(String estado) {
            this.estado = estado;
        }
    }

    class MensajeDTO {
        String mensajeId;
        String titulo;
        String contenido;
        String estado;

        public MensajeDTO(String mensajeId, String titulo, String contenido, String estado) {
            this.mensajeId = mensajeId;
            this.titulo = titulo;
            this.contenido = contenido;
            this.estado = estado;
        }
    }

    class Result {
        List<MensajeDTO> mensajes;

        public Result(List<MensajeDTO> mensajes) {
            this.mensajes = mensajes;
        }
    }

    Result handle(Query query);
}
```
---
 Historia de Usuario: Aprobar Mensajes
---
```java

// 1. Command Handler: AprobarMensajeCommandHandler

public interface AprobarMensajeCommand {
    class Command {
        String mensajeId;
        String aprobadorId;
        String comentario;

        public Command(String mensajeId, String aprobadorId, String comentario) {
            this.mensajeId = mensajeId;
            this.aprobadorId = aprobadorId;
            this.comentario = comentario;
        }
    }

    interface Result {}

    class Success implements Result {
        String mensajeId;

        public Success(String mensajeId) {
            this.mensajeId = mensajeId;
        }
    }

    class InvalidState implements Result {
        String errorMessage;

        public InvalidState(String errorMessage) {
            this.errorMessage = errorMessage;
        }
    }

    class Unauthorized implements Result {
        String userId;

        public Unauthorized(String userId) {
            this.userId = userId;
        }
    }

    Result handle(Command command);
}
```
---
```java
// 2. Query Handler: ObtenerMensajesPendientesQueryHandler

public interface ObtenerMensajesPendientesQuery {
    class Query {
        String aprobadorId;

        public Query(String aprobadorId) {
            this.aprobadorId = aprobadorId;
        }
    }

    class MensajePendienteDTO {
        String mensajeId;
        String titulo;
        String remitente;
        Date fechaEnvio;

        public MensajePendienteDTO(String mensajeId, String titulo, String remitente, Date fechaEnvio) {
            this.mensajeId = mensajeId;
            this.titulo = titulo;
            this.remitente = remitente;
            this.fechaEnvio = fechaEnvio;
        }
    }

    class Result {
        List<MensajePendienteDTO> mensajesPendientes;

        public Result(List<MensajePendienteDTO> mensajesPendientes) {
            this.mensajesPendientes = mensajesPendientes;
        }
    }

    Result handle(Query query);
}
```
---
Pruebas unitarias para RedactarMensajeCommandHandler
---
```java
// Escenario 1: Redactar mensaje exitosamente

public void testRedactarMensajeExitosamente() {
    // Arrange
    RedactarMensajeCommand.Command command = new RedactarMensajeCommand.Command(
        "Reporte de Incidente", 
        "Descripción detallada del incidente", 
        Arrays.asList("policia1@cocos.gov", "policia2@cocos.gov")
    );
    RedactarMensajeCommandHandler handler = new RedactarMensajeCommandHandler();

    // Act
    RedactarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof RedactarMensajeCommand.Success;
    assert ((RedactarMensajeCommand.Success) result).mensajeId != null;
}
```
---
```java
// Escenario 2: Redactar mensaje con entrada inválida

public void testRedactarMensajeEntradaInvalida() {
    // Arrange
    RedactarMensajeCommand.Command command = new RedactarMensajeCommand.Command(
        "",  // Título vacío, entrada inválida
        "Descripción detallada del incidente",
        Arrays.asList("policia1@cocos.gov", "policia2@cocos.gov")
    );
    RedactarMensajeCommandHandler handler = new RedactarMensajeCommandHandler();

    // Act
    RedactarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof RedactarMensajeCommand.InvalidInput;
    assert ((RedactarMensajeCommand.InvalidInput) result).errorMessage.equals("El título no puede estar vacío");
}
```
---
```java
// Escenario 3: Redactar mensaje por usuario no autorizado

public void testRedactarMensajeUsuarioNoAutorizado() {
    // Arrange
    RedactarMensajeCommand.Command command = new RedactarMensajeCommand.Command(
        "Reporte de Incidente", 
        "Descripción detallada del incidente", 
        Arrays.asList("policia1@cocos.gov", "policia2@cocos.gov")
    );
    RedactarMensajeCommandHandler handler = new RedactarMensajeCommandHandler();

    // Simulación de autorización fallida
    handler.setUserAuthorizationStatus("usuario_no_autorizado", false);

    // Act
    RedactarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof RedactarMensajeCommand.Unauthorized;
    assert ((RedactarMensajeCommand.Unauthorized) result).userId.equals("usuario_no_autorizado");
}
```
---
 Pruebas unitarias para AprobarMensajeCommandHandler
---
```java
// Escenario 1: Aprobar mensaje exitosamente

public void testAprobarMensajeExitosamente() {
    // Arrange
    AprobarMensajeCommand.Command command = new AprobarMensajeCommand.Command(
        "12345", 
        "jefe_estacion_1", 
        "Aprobado sin comentarios"
    );
    AprobarMensajeCommandHandler handler = new AprobarMensajeCommandHandler();

    // Act
    AprobarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof AprobarMensajeCommand.Success;
    assert ((AprobarMensajeCommand.Success) result).mensajeId.equals("12345");
}
```
---
```java
// Escenario 2: Aprobar mensaje en estado inválido

public void testAprobarMensajeEstadoInvalido() {
    // Arrange
    AprobarMensajeCommand.Command command = new AprobarMensajeCommand.Command(
        "12345", 
        "jefe_estacion_1", 
        "Aprobado sin comentarios"
    );
    AprobarMensajeCommandHandler handler = new AprobarMensajeCommandHandler();

    // Simulación de mensaje en estado no aprobable
    handler.setMensajeEstado("12345", "Enviado");

    // Act
    AprobarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof AprobarMensajeCommand.InvalidState;
    assert ((AprobarMensajeCommand.InvalidState) result).errorMessage.equals("El mensaje ya ha sido enviado y no puede ser aprobado");
}
```
---
```java
// Escenario 3: Aprobar mensaje por usuario no autorizado

public void testAprobarMensajeUsuarioNoAutorizado() {
    // Arrange
    AprobarMensajeCommand.Command command = new AprobarMensajeCommand.Command(
        "12345", 
        "policia1", 
        "Aprobado sin comentarios"
    );
    AprobarMensajeCommandHandler handler = new AprobarMensajeCommandHandler();

    // Simulación de autorización fallida
    handler.setUserAuthorizationStatus("policia1", false);

    // Act
    AprobarMensajeCommand.Result result = handler.handle(command);

    // Assert
    assert result instanceof AprobarMensajeCommand.Unauthorized;
    assert ((AprobarMensajeCommand.Unauthorized) result).userId.equals("policia1");
}
```
