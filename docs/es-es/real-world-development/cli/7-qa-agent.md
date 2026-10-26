---
title: "Lección 7 - Crea y usa un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la aptitud quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la aptitud `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- crearás e inspeccionarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus resultados según la incidencia de filtrado.

## Situación

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de cambios (PR). Un agente personalizado puede coordinar el proceso y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. En este taller, definirás el rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen distintas funciones. Las instrucciones del repositorio describen los estándares del equipo. La aptitud `quality-checks` agrupa las comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo usar esas capacidades para comprobar los requisitos y comunicar sus resultados. No sustituye a las demás herramientas ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha capturado el flujo de control de calidad y sus límites. Así evitarás que un agente incompleto o demasiado permisivo cambie la funcionalidad cuando solo quieres comprobarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. El campo `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible al agente.
3. Lee las instrucciones del perfil y confirma que el proceso empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la aptitud `quality-checks` y usa Playwright MCP.
4. Confirma que el agente comunica los resultados con pruebas, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de cambios.
5. Si al perfil le falta alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Retoma la misma conversación de filtrado después de crear el perfil y, a continuación, selecciona el agente de control de calidad para que use la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este mensaje:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aunque no aparezca, el comando anterior lo activará.

2. Usa este mensaje para pedir al agente de control de calidad que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- has creado e inspeccionado un perfil reutilizable de control de calidad.
- has seleccionado el agente de control de calidad y revisado sus resultados según la incidencia de filtrado.

Ya tienes preparada la implementación, la aptitud actualizada, el perfil de control de calidad, las pruebas y el informe de verificación. A continuación, [reunirás estos elementos en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
