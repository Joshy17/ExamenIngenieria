## Historia de Usuario: Gestión de Mensajes

 * **Como** usuario del sistema de mensajería,

 * **Quiero** poder redactar y aprobar mensajes,

 * **Para** que los mensajes sean enviados correctamente a los destinatarios y aprobados por los usuarios correspondientes.

### **Criterios de aceptación**:

#### **1. Redacción de mensajes:**

* **Como** usuario autorizado,
  **Puedo** redactar un mensaje con un título, contenido y destinatarios,
  **Para** que el mensaje sea enviado y guardado en el sistema.

  **Escenarios:**

    * **Escenario 1: Redactar mensaje exitosamente**
      Cuando el título, contenido y destinatarios son proporcionados correctamente, el sistema debe crear un mensaje y devolver un identificador de mensaje.

    * **Escenario 2: Entrada inválida en redacción**
      Cuando el título del mensaje esté vacío, el sistema debe devolver un mensaje de error que indique que el título no puede estar vacío.

    * **Escenario 3: Usuario no autorizado para redactar**
      Si el usuario no está autorizado, el sistema debe devolver un error de autorización indicando el ID del usuario no autorizado.

#### **2. Aprobación de mensajes:**

* **Como** usuario con rol de aprobador,
  **Puedo** aprobar mensajes pendientes,
  **Para** que los mensajes sean finalmente enviados a los destinatarios.

  **Escenarios:**

    * **Escenario 1: Aprobar mensaje exitosamente**
      Cuando un mensaje es aprobado correctamente, el sistema debe devolver un mensaje de éxito con el ID del mensaje aprobado.

    * **Escenario 2: Aprobar mensaje en estado inválido**
      Si el mensaje ya ha sido enviado o no está en un estado aprobable, el sistema debe devolver un error indicando que no se puede aprobar el mensaje.

    * **Escenario 3: Usuario no autorizado para aprobar**
      Si un usuario no tiene el permiso adecuado para aprobar el mensaje, el sistema debe devolver un error de autorización con el ID del usuario no autorizado.

---

## El codigo fue hecho en Kotlin

### Pregunta #2: Programe el componente requerido para cada user history

#### Historia de Usuario: Redactar Mensajes

##### 1. Command Handler: RedactarMensajeCommandHandler

```kotlin
import java.util.*

interface RedactarMensajeCommand {
    data class Command(
        val titulo: String,
        val contenido: String,
        val destinatarios: List<String>
    )

    sealed interface Result {
        data class Success(val mensajeId: String) : Result
        data class InvalidInput(val errorMessage: String) : Result
        data class Unauthorized(val userId: String) : Result
    }

    fun handle(command: Command): Result
}
````

##### 2. Query Handler: ObtenerMensajesQueryHandler

```kotlin
interface ObtenerMensajesQuery {
    data class Query(
        val estado: String
    )

    data class MensajeDTO(
        val mensajeId: String,
        val titulo: String,
        val contenido: String,
        val estado: String
    )

    data class Result(
        val mensajes: List<MensajeDTO>
    )

    fun handle(query: Query): Result
}
```

---

### Historia de Usuario: Aprobar Mensajes

##### 1. Command Handler: AprobarMensajeCommandHandler

```kotlin
interface AprobarMensajeCommand {
    data class Command(
        val mensajeId: String,
        val aprobadorId: String,
        val comentario: String
    )

    sealed interface Result {
        data class Success(val mensajeId: String) : Result
        data class InvalidState(val errorMessage: String) : Result
        data class Unauthorized(val userId: String) : Result
    }

    fun handle(command: Command): Result
}
```

##### 2. Query Handler: ObtenerMensajesPendientesQueryHandler

```kotlin
interface ObtenerMensajesPendientesQuery {
    data class Query(
        val aprobadorId: String
    )

    data class MensajePendienteDTO(
        val mensajeId: String,
        val titulo: String,
        val remitente: String,
        val fechaEnvio: Date
    )

    data class Result(
        val mensajesPendientes: List<MensajePendienteDTO>
    )

    fun handle(query: Query): Result
}
```

---

### Pruebas Unitarias
Programe pruebas unitarias para cada componente bajo el formato visto en clase.

### RedactarMensajeCommandHandler

##### Escenario 1: Redactar mensaje exitosamente

```kotlin
fun testRedactarMensajeExitosamente() {
    // Arrange
    val command = RedactarMensajeCommand.Command(
        titulo = "Reporte de Incidente",
        contenido = "Descripción detallada del incidente",
        destinatarios = ["policia1@cocos.gov", "policia2@cocos.gov"]
    )
    val handler = RedactarMensajeCommandHandler()

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is RedactarMensajeCommand.Result.Success
    assert result.mensajeId != null
}
```

##### Escenario 2: Redactar mensaje con entrada inválida

```kotlin
fun testRedactarMensajeEntradaInvalida() {
    // Arrange
    val command = RedactarMensajeCommand.Command(
        titulo = "", // Título vacío, entrada inválida
        contenido = "Descripción detallada del incidente",
        destinatarios = ["policia1@cocos.gov", "policia2@cocos.gov"]
    )
    val handler = RedactarMensajeCommandHandler()

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is RedactarMensajeCommand.Result.InvalidInput
    assert result.errorMessage == "El título no puede estar vacío"
}
```

##### Escenario 3: Redactar mensaje por usuario no autorizado

```kotlin
fun testRedactarMensajeUsuarioNoAutorizado() {
    // Arrange
    val command = RedactarMensajeCommand.Command(
        titulo = "Reporte de Incidente",
        contenido = "Descripción detallada del incidente",
        destinatarios = ["policia1@cocos.gov", "policia2@cocos.gov"]
    )
    val handler = RedactarMensajeCommandHandler()

    // Simulación de autorización fallida
    handler.setUserAuthorizationStatus("usuario_no_autorizado", false)

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is RedactarMensajeCommand.Result.Unauthorized
    assert result.userId == "usuario_no_autorizado"
}
```

---

### AprobarMensajeCommandHandler

##### Escenario 1: Aprobar mensaje exitosamente

```kotlin
fun testAprobarMensajeExitosamente() {
    // Arrange
    val command = AprobarMensajeCommand.Command(
        mensajeId = "12345",
        aprobadorId = "jefe_estacion_1",
        comentario = "Aprobado sin comentarios"
    )
    val handler = AprobarMensajeCommandHandler()

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is AprobarMensajeCommand.Result.Success
    assert result.mensajeId == "12345"
}
```

##### Escenario 2: Aprobar mensaje en estado inválido

```kotlin
fun testAprobarMensajeEstadoInvalido() {
    // Arrange
    val command = AprobarMensajeCommand.Command(
        mensajeId = "12345",
        aprobadorId = "jefe_estacion_1",
        comentario = "Aprobado sin comentarios"
    )
    val handler = AprobarMensajeCommandHandler()

    // Simulación de mensaje en estado no aprobable
    handler.setMensajeEstado("12345", "Enviado")

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is AprobarMensajeCommand.Result.InvalidState
    assert result.errorMessage == "El mensaje ya ha sido enviado y no puede ser aprobado"
}
```

##### Escenario 3: Aprobar mensaje por usuario no autorizado

```kotlin
fun testAprobarMensajeUsuarioNoAutorizado() {
    // Arrange
    val command = AprobarMensajeCommand.Command(
        mensajeId = "12345",
        aprobadorId = "policia1",
        comentario = "Aprobado sin comentarios"
    )
    val handler = AprobarMensajeCommandHandler()

    // Simulación de autorización fallida
    handler.setUserAuthorizationStatus("policia1", false)

    // Act
    val result = handler.handle(command)

    // Assert
    assert result is AprobarMensajeCommand.Result.Unauthorized
    assert result.userId == "policia1"
}
```
---
### Fitness Function: Evaluación de Rendimiento del API Rest para Guardado Automático
#### ¿Qué?
La eficiencia del API REST al recuperar mensajes, midiendo:
* Tiempo de respuesta (latencia)
* Tasa de éxito (status 200 OK)
* Capacidad de respuesta bajo carga simultánea (concurrencia)
* Consistencia del contenido recuperado

#### ¿Cómo?
Mediante pruebas de rendimiento automatizadas con herramientas como Apache JMeter, k6
o Locust, considerando:

- Carga simulada: 10, 100, 500, 1000 solicitudes concurrentes.
#### Métricas recolectadas:
* Tiempo medio de respuesta (avg_response_time)
* Tiempo máximo de respuesta (max_response_time)
* Porcentaje de errores (error_rate)
* Porcentaje de respuestas bajo el umbral de 500 ms (p95 < 500ms)

### Escenarios de prueba:
* Recuperación de un único mensaje por ID
* Recuperación paginada (e.g., últimos 20 mensajes)
* Recuperación filtrada (e.g., por usuario, fecha)

### ¿Cuándo?
* Durante la etapa de pruebas del sistema, antes del paso a producción (pruebas prerelease).
* En cada integración continua (CI/CD) para detectar regresiones en el rendimiento.
* Después de cada refactor o cambio en la capa de recuperación de datos.

