---
title: "Ejercicio 5 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramientas y edición de archivos mientras se producen. Puedes revisar las diferencias, aceptar o rechazar cambios individuales y guiar la conversación con mensajes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- guiarás al agente durante la sesión para que añada un interruptor del modo claro además del modo de alto contraste.

## Situación

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas, lectura de archivos, edición y comando de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para consultar todos los archivos modificados en esta sesión.

> [!NOTE]
> Si una llamada a herramientas requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería aparecer un mensaje final que resuma los cambios.

## Guía al agente para que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un interruptor del modo claro. Como continúas en la misma sesión de chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, examinará los cambios existentes y propondrá las modificaciones para el modo claro.
3. Revisa cada nueva edición a medida que aparece y usa **Keep** o **Undo** como antes.
4. Si el agente toma un rumbo que no te convence (por ejemplo, intenta refactorizar la interfaz de los interruptores), detenlo con el botón **Stop** junto al cuadro de mensaje y envía una indicación para aclarar lo que necesitas.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer interruptor; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos interruptores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos de los archivos y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando hayas terminado la revisión, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones individuales.
- has guiado una sesión en curso con mensajes de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás los cambios que ha generado el agente personalizado local durante este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
