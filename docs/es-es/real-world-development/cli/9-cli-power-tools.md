---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas en la nube de forma opcional."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye varios comandos de barra. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos algunos más.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo con árboles de trabajo y delegar tareas al agente en la nube.

## Situación

Has terminado el flujo de trabajo principal de la CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar tareas en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, pero puedes examinar y compactar el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y el modo en que se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara los resultados. Si la conversación ya es pequeña, quizá no veas un gran cambio.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Si vas a cambiar a una tarea no relacionada y quieres iniciar una conversación nueva, usa `/clear` o `/new`.

## Elige un modelo

Cada modelo tiene distintos puntos fuertes y cada desarrollador tiene sus preferencias. Copilot CLI te permite ver los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o presiona <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir conocimientos y colaborar ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, como `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir mensajes, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no quiera compartir.

## Opcional: amplía o delega el trabajo

Has terminado el taller principal. Copilot CLI también incluye comandos para tareas más grandes:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva y bien definida, y revisa el resultado con el flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre entre bastidores. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido a exportar o compartir una sesión con `/share`.
- has explorado los comandos opcionales para trabajar en paralelo con árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI tiene muchos más comandos de barra. Para terminar, [repasemos lo aprendido][next-lesson] y veamos cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
