---
title: "Lección 8 - Crea y fusiona la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la aptitud, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la aptitud modificada, el perfil de control de calidad y las pruebas están en una sola rama. Es hora de revisarlo todo y abrir una solicitud de cambios. Tú mismo fusionaste la solicitud de cambios (PR) de valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la aptitud y el agente de control de calidad en varias PR. Para simplificar el taller, hemos mantenido todo el trabajo del flujo de filtrado y calidad en una conversación y una rama; todos los cambios se incluirán en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Situación

Durante el flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, sin que el desarrollador pierda el control de la autorización de fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR, por ejemplo, corrige las comprobaciones de integración continua (CI) fallidas, responde a los comentarios de revisión y actualiza la rama base cuando es necesario. A continuación, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora, tú mismo has seleccionado **Merge pull request**. Agent Merge puede encargarse de ello. Revisa el trabajo y confirma que está listo antes de activar Agent Merge.

## Usa Agent Merge para gestionar la PR

Ya está todo el código. Revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y presiona <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, la fusionará.
7. Confirma que recibes un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones requeridas, la protección de ramas, las colas de fusión, la configuración del repositorio ni la falta de permisos. Si no puede continuar, lee el motivo y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación y, ahora, el proceso de la solicitud de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge únicamente después de revisar los cambios y has confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y las sesiones, y para delegar tareas en la nube de forma opcional.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
