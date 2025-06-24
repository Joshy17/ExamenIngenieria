# Practica

---

## Historia de Usuario: Gestión de Notificaciones

* **Como** administrador de un sistema de notificaciones,

*  **Quiero** poder crear, editar y eliminar notificaciones,

*  **Para** que los usuarios del sistema reciban alertas relevantes según las actualizaciones o eventos importantes.

### **Criterios de aceptación**:

#### **1. Creación de notificaciones:**

* **Como** administrador autorizado,
  **Puedo** crear una notificación con un título, contenido y lista de destinatarios,
  **Para** que la notificación sea enviada a los usuarios correspondientes.

  **Escenarios:**

    * **Escenario 1: Crear notificación exitosamente**
      Cuando se proporcionan todos los campos requeridos (título, contenido y destinatarios), la notificación debe ser creada y se debe devolver un identificador único de la notificación.

    * **Escenario 2: Entrada inválida en creación**
      Si el título o contenido está vacío, la notificación no debe ser creada, y el sistema debe devolver un mensaje de error detallado.

    * **Escenario 3: Usuario no autorizado para crear notificación**
      Si el usuario no tiene permisos de administrador, el sistema debe devolver un error de autorización.

#### **2. Edición de notificaciones:**

* **Como** administrador autorizado,
  **Puedo** editar una notificación existente,
  **Para** actualizar la información o cambiar su estado.

  **Escenarios:**

    * **Escenario 1: Editar notificación exitosamente**
      Cuando el administrador proporciona nuevos datos para una notificación existente, el sistema debe actualizar la notificación y devolver un mensaje de éxito con la nueva información.

    * **Escenario 2: Intentar editar notificación no existente**
      Si la notificación que se intenta editar no existe, el sistema debe devolver un error indicando que la notificación no fue encontrada.

#### **3. Eliminación de notificaciones:**

* **Como** administrador autorizado,
  **Puedo** eliminar una notificación,
  **Para** que las notificaciones obsoletas o no deseadas sean eliminadas del sistema.

  **Escenarios:**

    * **Escenario 1: Eliminar notificación exitosamente**
      Cuando el administrador elimina una notificación existente, el sistema debe eliminarla y devolver un mensaje de confirmación de la eliminación.

    * **Escenario 2: Intentar eliminar notificación no existente**
      Si la notificación a eliminar no existe, el sistema debe devolver un error indicando que la notificación no fue encontrada.

#### **4. Verificación de permisos:**

* **Como** administrador del sistema,
  **Puedo** garantizar que solo los usuarios con privilegios adecuados puedan realizar operaciones de creación, edición y eliminación,
  **Para** que el sistema sea seguro y evite acciones no autorizadas.

  **Escenarios:**

    * **Escenario 1: Verificación de permisos**
      Si un usuario intenta realizar una acción (crear, editar, eliminar) sin tener el rol adecuado, el sistema debe devolver un mensaje de error de autorización.

---
