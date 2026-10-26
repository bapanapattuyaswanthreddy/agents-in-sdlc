---
title: "Ejercicio 4 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de agentes con solicitudes de cambios abiertas, explorar las operaciones realizadas y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Situación

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo funciones para cambiar de modo, parece lógico incluir también esta funcionalidad. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de las tareas que has asignado al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel lateral. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos de la lista varían según el estado del trabajo de Copilot y el enfoque que haya elegido.

4. Observa también el panel de solicitudes de cambios (PR) que aparece a la derecha. Te permite consultar la PR y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto varía según el nombre que haya elegido Copilot y el estado actual de la tarea.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Presiona <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

Como antes, esto tardará varios minutos. Aprovecha para hacer una pausa y reflexionar sobre lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Desde este centro puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para cambiar su alcance o proporcionar más indicaciones.

Has explorado estos temas:

- has explorado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
