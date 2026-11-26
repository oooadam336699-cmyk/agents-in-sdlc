---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores has asignado tres tareas distintas al agente en la nube de Copilot para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de agente con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y orientar una sesión que esté en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo controles de selección, parece lógico incluirlo en el mismo trabajo. Orientarás al agente para que incorpore el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube

Consulta el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en ejecución y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de PR, que aparece a la derecha y permite consultar la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Ya que estamos trabajando en un modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar la apariencia que prefieran.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la solicitud nueva al flujo cuando termine el paso que estaba realizando al recibir el mensaje.

El trabajo puede tardar varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has explorado estos conceptos:

- has explorado Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
