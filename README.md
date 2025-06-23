# ExamenIngenieria

Aquí tienes un resumen de los temas que mencionaste:

### ADR (Architecture Decision Records)

* **Qué son**: Son registros documentales que capturan decisiones importantes de arquitectura, su contexto, intención y consecuencias.
* **Estructura**: Incluye título, estado, contexto, decisión, consecuencias, gobernanza y notas. El "Estado" puede ser "Propuesto", "Solicitando comentarios", o "Aceptado". Se pueden crear nuevos ADRs si una decisión se vuelve obsoleta.
* **Beneficios**: Proveen transparencia, facilitan la comunicación entre equipos y mantienen un registro de la evolución de la arquitectura.

### Fitness Functions

* **Qué son**: Mecanismos que permiten mantener la salud de una arquitectura de software al medir sus características clave.
* **Tipos**:

  * **Atómica**: Verifica un solo aspecto de la arquitectura.
  * **Holística**: Evalúa varios aspectos a la vez.
  * **Automatizada**: Ejecución continua de pruebas en entornos de integración continua.
  * **Estática y dinámica**: Estáticas tienen resultados fijos, mientras que las dinámicas pueden variar.
* **Cuándo utilizarlas**: Desde la fase inicial de arquitectura hasta la evolución continua del sistema, ayudan a garantizar la integridad de los aspectos arquitectónicos.

### Elección de Lenguaje de Programación

* **Tipado estático vs. dinámico**:

  * **Estático**: Verificación de tipos en tiempo de compilación (ej. Java, C).
  * **Dinámico**: Tipos determinados en tiempo de ejecución (ej. Python, JavaScript).
* **Soporte para estructuras de datos y interfaces**: Los lenguajes con tipado estático (como Java y TypeScript) ofrecen mayor soporte para la creación de estructuras de datos definidas y la creación de interfaces que permiten patrones de diseño como la inyección de dependencias.

### Almacenamiento de Datos

* **CRUD**: Operaciones básicas de bases de datos (Crear, Leer, Actualizar, Eliminar).
* **CQRS**: Segrega las operaciones de lectura y escritura, permitiendo la optimización de ambos modelos.
* **Event Store**: Almacena eventos en lugar de estados, útil en sistemas donde los cambios deben ser registrados para facilitar la reconstrucción del estado de la aplicación.

### Latencia

* **Factores de latencia**: Cliente, red y servidor.
* **Reducción de latencia**:

  * **Cliente**: Minimizar código, usar marcos reactivos ligeros.
  * **Red**: Reducir número de solicitudes y servir recursos estáticos a través de CDN.
  * **Servidor**: Procesar trabajos asíncronos para no bloquear la respuesta inmediata.

### Logging

* **Estrategia de registro**: Es fundamental planificar cómo se manejarán los registros desde la arquitectura, centralizando los logs y manteniendo contexto útil.
* **Contexto en registros**: Se puede incluir información relevante en los registros mediante el uso de variables globales o locales por hilo, asegurando que toda la información esté disponible para la depuración.

### Pruebas de Software

* **Pruebas automatizadas**: La automatización es clave para manejar grandes sistemas y mantener la calidad en el ciclo de desarrollo continuo.
* **Tamaño y alcance de las pruebas**:

  * **Unitarias**: Validan funciones individuales de forma aislada.
  * **Integración**: Validan interacciones entre componentes.
  * **End-to-End**: Verifican todo el sistema.
* **Pruebas grandes**: Son necesarias para probar sistemas completos y sus interacciones, pero son más lentas y difíciles de manejar debido a la complejidad del entorno.

### Pruebas a Gran Escala

* **Desafíos**: A medida que el sistema escala, es necesario usar pruebas más grandes para verificar la interacción entre componentes, pero estas son lentas, frágiles y requieren recursos significativos.
* **Soluciones**: Dividir las pruebas grandes en partes más pequeñas y manejables, y utilizar herramientas como "record/replay proxies" para simular comportamientos en pruebas más pequeñas.

---

## Ejemplos

### 1. Ejemplo de **ADR** (Architecture Decision Record)

Un **ADR** es una forma estructurada de documentar una decisión arquitectónica que se ha tomado, su contexto, las razones detrás de esa decisión y sus consecuencias.

#### Ejemplo de ADR: Decisión sobre el tipo de base de datos

```markdown
# Título: Usar una base de datos NoSQL para almacenar productos

## Estado
Propuesto

## Contexto
El equipo está desarrollando una aplicación para un comercio electrónico, donde los productos tienen una estructura de datos que puede cambiar dinámicamente (por ejemplo, productos con diferentes atributos dependiendo de la categoría). Se necesita una solución de almacenamiento flexible que pueda soportar estos cambios sin necesidad de migraciones frecuentes.

## Decisión
Después de evaluar diferentes opciones, decidimos usar MongoDB, una base de datos NoSQL. Esto nos proporcionará la flexibilidad necesaria para almacenar productos con estructuras de datos variadas sin la rigidez de una base de datos relacional.

## Consecuencias
- **Positivas**: 
  - Mayor flexibilidad en la estructura de datos.
  - Facilidad para escalar horizontalmente a medida que aumentan los datos.
- **Negativas**: 
  - La consistencia eventual de MongoDB puede ser un desafío en situaciones donde la coherencia es crítica.
  - Necesitaremos implementar estrategias personalizadas para garantizar la integridad de los datos.

## Gobernanza
La implementación de esta decisión será monitoreada mediante revisiones periódicas del diseño de la base de datos y pruebas de rendimiento para asegurar que cumple con los requisitos.

## Notas
Fecha de aprobación: 2025-06-23
Aprobado por: Equipo de Arquitectura
```

### 2. Ejemplo de **Fitness Function**

Una **Fitness Function** evalúa una característica específica de la arquitectura para asegurarse de que el sistema se mantenga saludable según los criterios definidos (por ejemplo, escalabilidad, seguridad).

#### Ejemplo de Fitness Function para verificar la **escabilidad** del sistema

Para este ejemplo, usaremos un enfoque basado en pruebas automatizadas para evaluar la escalabilidad de un servicio. Aquí usamos un test de carga que simula múltiples usuarios.

##### Fitness Function en **Java** utilizando JUnit y un framework de simulación de carga como **Gatling**:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class ScalabilityFitnessFunction {

    @Test
    public void testServiceScalability() {
        // Simula un número alto de usuarios concurrentes
        int simulatedUsers = 1000;
        long responseTimeThreshold = 2000;  // 2 segundos

        long responseTime = simulateServiceLoad(simulatedUsers);

        // La función de fitness evalúa si la latencia está dentro del umbral aceptable
        assertTrue(responseTime < responseTimeThreshold, "La respuesta fue demasiado lenta");
    }

    private long simulateServiceLoad(int users) {
        long startTime = System.currentTimeMillis();

        // Simulación de una carga de trabajo en el servicio
        // Esto puede ser reemplazado por una herramienta como Gatling que hace las peticiones HTTP reales
        for (int i = 0; i < users; i++) {
            // Aquí simulamos una petición de servicio por cada usuario
            simulateRequest();
        }

        return System.currentTimeMillis() - startTime;
    }

    private void simulateRequest() {
        try {
            Thread.sleep(10);  // Simula un retraso de 10 ms por cada solicitud
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

#### Explicación:

* **Simulamos una carga**: La función `simulateServiceLoad()` simula un número dado de usuarios concurrentes que hacen solicitudes a un servicio.
* **Verificación de la respuesta**: Se asegura que el tiempo de respuesta para una carga de 1000 usuarios no exceda los 2 segundos.
* **Escalabilidad**: La Fitness Function evalúa si el sistema es capaz de manejar la carga sin que los tiempos de respuesta se disparen.

### 3. Ejemplo de **Fitness Function** para **Seguridad**

Si necesitas asegurar que los datos sensibles no se expongan (por ejemplo, en una base de datos), puedes definir una Fitness Function para verificar que los datos estén cifrados correctamente.

#### Ejemplo en **Java** con una función que valida si los datos sensibles están cifrados en la base de datos:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class SecurityFitnessFunction {

    @Test
    public void testSensitiveDataEncryption() {
        String data = getDataFromDatabase("userPassword");

        // Evaluamos que la contraseña esté cifrada
        assertTrue(isEncrypted(data), "Los datos sensibles no están cifrados correctamente");
    }

    private String getDataFromDatabase(String columnName) {
        // Aquí se simula una consulta a la base de datos
        // En un caso real, obtendrías datos de tu base de datos, como una contraseña de un usuario
        return "encrypted_password_12345";  // Simula datos cifrados
    }

    private boolean isEncrypted(String data) {
        // Función simple que verifica si los datos contienen un prefijo que indica que están cifrados
        return data.startsWith("encrypted_");
    }
}
```

#### Explicación:

* **Obtenemos datos sensibles** de la base de datos (simulados en este caso).
* **Verificamos** que los datos estén cifrados usando un prefijo (`"encrypted_"`) como un simple criterio de validación.

---

## Ejemplo (Pseucodocodigo)

### Historia de Usuario: Manejo de Información del Usuario

#### Como **usuario administrador** del sistema,

Quiero poder **actualizar el nombre de los usuarios** y **consultar los detalles de un usuario**,

Para poder **mantener actualizada la información personal de los usuarios** y **verificar sus datos cuando sea necesario**.

---

### **Criterios de Aceptación**:

1. **Actualizar el nombre de usuario**:

   * El sistema debe permitir que un administrador actualice el nombre de un usuario.
   * El nombre del usuario debe ser actualizado correctamente en la base de datos.
   * El sistema debe verificar que el usuario existe antes de intentar actualizar los datos.
   * Si el usuario no existe, el sistema debe manejar el error de forma adecuada.

2. **Obtener los detalles de un usuario**:

   * El sistema debe permitir que un administrador consulte los detalles de un usuario usando su `userId`.
   * El sistema debe devolver la información completa del usuario (por ejemplo, `userId`, `name`, etc.).
   * Si el usuario no existe, el sistema debe manejar el error de forma adecuada.

---

### **Historias de Usuario en el código**:

El **Command Handler** y el **Query Handler** que definimos están directamente relacionados con las funcionalidades de la historia de usuario. Aquí es como se alinea:

* **Comando de Actualización de Usuario** (`UpdateUserNameHandler`): Permite al administrador actualizar el nombre de un usuario. Este es el comportamiento esperado de la funcionalidad de "Actualizar el nombre de usuario".

* **Consulta de Usuario** (`GetUserDetailsHandler`): Permite al administrador consultar la información del usuario (como su nombre y `userId`). Esta funcionalidad corresponde a la parte de "Obtener los detalles de un usuario".

### **En términos de pruebas**:

* Las pruebas unitarias que escribimos validan que la **actualización de datos** y la **consulta de datos** funcionan correctamente. Estas pruebas aseguran que las funcionalidades del sistema están implementadas correctamente y que el sistema reacciona de manera adecuada ante situaciones como cambios de datos o consultas incorrectas.

---

### Ejemplo en pseudocódigo

#### 1. **Command Handler** (Manejo de Comandos)

El **Command Handler** maneja una acción que cambia el estado de la aplicación.

```plaintext
// Command: Actualizar nombre de usuario
class UpdateUserNameCommand {
    constructor(userId, newUserName) {
        this.userId = userId;
        this.newUserName = newUserName;
    }
}

// Command Handler
class UpdateUserNameHandler {
    constructor(userRepository) {
        this.userRepository = userRepository;  // Repositorio que accede a los datos
    }

    handle(command) {
        let user = this.userRepository.findUserById(command.userId);
        if (user) {
            user.name = command.newUserName;
            this.userRepository.save(user);  // Guardar cambios
        }
    }
}
```

#### 2. **Query Handler** (Manejo de Consultas)

El **Query Handler** maneja una consulta que solo lee datos sin cambiar el estado de la aplicación. Aquí crearemos uno para obtener los detalles de un usuario por su ID.

```plaintext
// Query: Obtener detalles de un usuario
class GetUserDetailsQuery {
    constructor(userId) {
        this.userId = userId;
    }
}

// Query Handler
class GetUserDetailsHandler {
    constructor(userRepository) {
        this.userRepository = userRepository;  // Repositorio que accede a los datos
    }

    handle(query) {
        return this.userRepository.findUserById(query.userId);
    }
}
```

### 3. **Pruebas Unitarias** (usando el patrón Arrange, Act, Assert)

#### Prueba unitaria para el **Command Handler** (`UpdateUserNameHandler`)

```plaintext
// Prueba para el Command Handler UpdateUserNameHandler
function testUpdateUserName() {
    // Arrange: Preparar los objetos necesarios
    let userRepository = new InMemoryUserRepository(); // Usamos un repositorio en memoria para pruebas
    let handler = new UpdateUserNameHandler(userRepository);
    let user = new User(1, "Juan");  // Crear un usuario de prueba
    userRepository.save(user);  // Guardar el usuario en el repositorio en memoria
    let command = new UpdateUserNameCommand(1, "Carlos");  // Crear el comando para actualizar el nombre

    // Act: Ejecutar la acción
    handler.handle(command);  // Llamar al handler para procesar el comando

    // Assert: Verificar que el estado ha cambiado correctamente
    let updatedUser = userRepository.findUserById(1);
    assert(updatedUser.name === "Carlos", "El nombre del usuario no se actualizó correctamente.");
}
```

#### Prueba unitaria para el **Query Handler** (`GetUserDetailsHandler`)

Aquí tenemos la prueba unitaria para asegurarnos de que el **GetUserDetailsHandler** retorna correctamente los detalles de un usuario.

```plaintext
// Prueba para el Query Handler GetUserDetailsHandler
function testGetUserDetails() {
    // Arrange: Preparar los objetos necesarios
    let userRepository = new InMemoryUserRepository(); // Usamos un repositorio en memoria para pruebas
    let handler = new GetUserDetailsHandler(userRepository);
    let user = new User(1, "Carlos");  // Crear un usuario de prueba
    userRepository.save(user);  // Guardar el usuario en el repositorio en memoria
    let query = new GetUserDetailsQuery(1);  // Crear la consulta para obtener detalles del usuario

    // Act: Ejecutar la consulta
    let result = handler.handle(query);  // Llamar al handler para obtener los detalles del usuario

    // Assert: Verificar que el resultado es correcto
    assert(result.name === "Carlos", "El nombre del usuario no es el esperado.");
    assert(result.id === 1, "El ID del usuario no es el esperado.");
}
```

### Explicación del **Patrón Arrange, Act, Assert**:

1. **Arrange (Preparar)**:

   * Aquí se preparan todas las dependencias necesarias y el entorno de prueba. En los ejemplos anteriores, creamos instancias de `userRepository`, `handler`, y los datos de prueba (como el `User` y el `UpdateUserNameCommand`).

2. **Act (Ejecutar)**:

   * En esta etapa, se ejecuta la acción o el comportamiento que estamos probando. Llamamos al `handler.handle(command)` o `handler.handle(query)` para realizar la operación de cambio de estado o consulta.

3. **Assert (Verificar)**:

   * Finalmente, se verifica que el resultado de la acción sea el esperado. En este caso, se asegura que el nombre del usuario haya sido actualizado correctamente y que la consulta retorne los detalles esperados.

### Fitness Function: Evaluación del Command Handler y Query Handler de Usuarios

#### **¿Qué?**

La eficiencia y efectividad del **Command Handler** y **Query Handler** para manejar las solicitudes de actualización de datos y consulta de usuarios en el sistema, midiendo:

* **Tiempo de ejecución del Command Handler** (latencia para actualizar un usuario).
* **Tiempo de ejecución del Query Handler** (latencia para consultar los detalles de un usuario).
* **Tasa de éxito de las operaciones** (porcentaje de actualizaciones y consultas exitosas).
* **Completitud y consistencia de los datos** (validar que el usuario haya sido actualizado correctamente o que los datos consultados sean correctos).
* **Escalabilidad** bajo carga de múltiples solicitudes (concurrencia de solicitudes de actualización o consulta).

#### **¿Cómo?**

Mediante pruebas de rendimiento y validación de lógica de negocio con herramientas automatizadas como **JUnit**, **Mockito**, o herramientas de prueba de rendimiento como **k6** y **Apache JMeter**, considerando lo siguiente:

##### **Métricas recolectadas**:

* **Tiempo de respuesta promedio** (`avg_response_time`) para manejar las solicitudes de actualización y consulta.
* **Tiempo máximo de respuesta** (`max_response_time`) para manejar las solicitudes de actualización y consulta.
* **Porcentaje de errores** (`error_rate`), que mide las respuestas no exitosas en el proceso de actualización de datos o consulta de detalles.
* **Completitud de la actualización**: Verificar que el nombre del usuario se haya actualizado correctamente.
* **Consistencia de los datos**: Validar que los detalles del usuario sean correctos en cada consulta.

##### **Escenarios de prueba**:

* **Comando de actualización de usuario**:

  * Actualización de un único usuario con datos válidos.
  * Prueba con un nombre de usuario inválido o no encontrado.
  * Prueba de latencia para actualizar un nombre bajo carga simultánea (e.g., 10, 100, 500 actualizaciones simultáneas).
* **Consulta de detalles del usuario**:

  * Consulta exitosa de los detalles de un usuario por ID.
  * Consulta de un usuario que no existe (debería devolver un error o un valor nulo).
  * Evaluación de la latencia para las consultas bajo carga de múltiples solicitudes simultáneas.
  * Verificar que la respuesta del `Query Handler` sea consistente y precisa, garantizando que los datos devueltos corresponden con los almacenados en el sistema.

#### **¿Cuándo?**

* Durante las **pruebas de rendimiento** de la aplicación, antes del despliegue en producción (pruebas prerelease).
* En cada **integración continua (CI/CD)** para detectar cualquier **regresión** en el rendimiento del **Command Handler** o **Query Handler**.
* Después de cada **refactorización** o cambio importante en la capa de persistencia de datos o lógica de negocio.
* Durante las **pruebas de estrés** para evaluar la capacidad del sistema bajo carga y garantizar que la aplicación pueda manejar múltiples solicitudes concurrentes de actualización o consulta sin degradación significativa del rendimiento.






