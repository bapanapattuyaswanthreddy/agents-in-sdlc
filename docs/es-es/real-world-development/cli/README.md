---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crea, verifica y entrega dos cambios de Tailspin Toys mientras aprendes sobre los modos de Copilot CLI, las personalizaciones, las herramientas MCP y la automatización de solicitudes de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot al terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas desde la línea de comandos, para que puedas seguir trabajando sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo con Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con el entorno.
2. Añade una valoración con estrellas, revisa el resultado en el navegador y fusiona manualmente tu primera solicitud de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en el modo Plan, crea la funcionalidad en Autopilot y revísala en Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la aptitud existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar los filtros en un navegador.
7. Crea un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa todos los cambios del filtrado y usa Agent Merge para la PR de la funcionalidad.
9. Explora los comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la aptitud, el perfil de control de calidad y las pruebas. El filtrado y el flujo de calidad comparten una conversación y una rama para que puedas aprovechar el trabajo previo mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instala Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI, y familiarízate con el entorno |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado sin valorar; después, fusiona tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de la incidencia, créala con Autopilot y revísala en Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa una aptitud de comprobación de calidad][ex5] | Comprobaciones repetibles | Explora la aptitud, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Exploración en navegador | Configura MCP en la CLI y comprueba el comportamiento del filtrado |
| [7. Crea y usa un agente de control de calidad][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crea y fusiona la PR de la funcionalidad][ex8] | Revisión y fusión | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, implementa un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos de operaciones de terminal o línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Empieza

**[Empieza por los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
