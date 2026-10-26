---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y proporcionar a Copilot más automatizaciones. Para eso sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y use otras herramientas y servicios.

En esta lección:

- aprenderás qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Situación

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz de usuario hay que interactuar con ella. Permitirás que Copilot use el sitio como lo haría una persona para automatizar aún más los cambios y comprobar que funcionan como esperas.

## ¿Qué es el Model Context Protocol (MCP)?

El [Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden consultar información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante servidores MCP, que actúan como puente entre el agente de IA y los servicios externos. Cada servidor MCP ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]**: permite acceder a las API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización de navegadores con Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir los servidores y colaborar con el ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y ten en cuenta sus implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP con este comando:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y presiona <kbd>Tab</kbd>.
4. Confirma el tipo **STDIO** presionando <kbd>Enter</kbd> y luego <kbd>Tab</kbd>.
5. Pega este comando en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Presiona <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Presiona <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente que la funcionalidad se comportaba como esperabas. Ahora usa el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa este mensaje para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No tienes que pedir expresamente a Copilot que use el servidor MCP; normalmente lo averigua por su cuenta. Sin embargo, indicarle qué herramienta debe usar puede producir resultados más coherentes y ahorrar tokens.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- has añadido el servidor Playwright MCP si aún no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combinará la aptitud y las herramientas del navegador.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
