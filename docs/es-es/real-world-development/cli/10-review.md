---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI durante un flujo de trabajo completo con Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado.
- has añadido valoraciones con estrellas, revisado el resultado en un navegador abierto mediante el puerto reenviado y fusionado manualmente tu primera solicitud de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la aptitud existente `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar los filtros en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad para evaluar los requisitos, la cobertura, los resultados de la aptitud y las pruebas en el navegador.
- has revisado todos los cambios del filtrado y autorizado a Agent Merge a gestionar la PR.
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en una rama basada en la versión actualizada de `main`:

1. **Valoraciones con estrellas:** mostrar en las tarjetas de juegos el `starRating` existente y un estado explícito cuando no haya valoración.
2. **Filtrado y flujo de calidad:** implementar los filtros, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de control de calidad e incluir las pruebas correspondientes.

Desde la planificación de los filtros hasta la apertura de su PR, has trabajado en la misma conversación y rama. Para simplificar el taller, hemos reunido todo ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, examinándolo en el navegador y mediante la exploración del navegador con Copilot y MCP. La aptitud `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos y ha presentado los resultados en el nuevo formato. El agente de control de calidad ha reunido esos resultados con la revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas añadidas deben cubrir carencias reales; es correcto que una comprobación de calidad no necesite añadir pruebas. Después de realizar cambios, revisa el código y las pruebas, y vuelve a generar las pruebas necesarias.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una aptitud, creado un perfil de control de calidad y configurado un servidor MCP. Reutiliza esas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las aptitudes describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para participar en los cambios concretos y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para los cambios rutinarios y uno más potente para las tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más funciones para explorar

Has completado el flujo principal. También puedes explorar otras funciones de Copilot CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que hace años que quieres crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
