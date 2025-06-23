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

### Conclusión

* **ADR**: Es una herramienta poderosa para mantener un registro de las decisiones arquitectónicas, documentando su contexto, las decisiones tomadas y las consecuencias. Te permite gestionar el cambio de forma estructurada.
* **Fitness Functions**: Son funciones automatizadas que aseguran que tu sistema cumpla con ciertos criterios de calidad, como rendimiento, seguridad, escalabilidad, etc. Estas funciones son esenciales para mantener la salud de la arquitectura en sistemas complejos y en evolución.

Si necesitas ejemplos más específicos o ajustes, no dudes en pedírmelo.


