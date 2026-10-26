---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: modo agente][previous-lesson] · [Siguiente lección: agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y permitirás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una solicitud de cambios para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede usar herramientas externas al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden consultar información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los servidores MCP proporcionan estas herramientas y recursos, y actúan como puente entre el agente de IA y los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos. Algunos ejemplos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización de navegadores, como abrir páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) que facilita descubrir servidores y colaborar con el ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Usa únicamente servidores de confianza y concede acceso a recursos sensibles con cautela.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye el servidor `github`, que permitió a Copilot leer la lista de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no tienes que instalar nada localmente. VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, requisito para Codespaces, donde no hay un escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP **cuando los necesita**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparece una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y acepta para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites estos pasos, VS Code lo iniciará (y te pedirá que confirmes que confías en él) cuando Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En lugar de probar los cambios manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y comunicarte el resultado.

El servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de listas), como lo hace la tecnología de asistencia. Una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador a través del servidor Playwright MCP, seguirá los pasos y te informará de lo que ha observado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona correctamente, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el mensaje. La primera vez que Copilot use Playwright MCP quizá tenga que descargar un navegador. Si informa de que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Ya has confirmado que la funcionalidad funciona. Ahora puedes abrir una solicitud de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea la solicitud de cambios

Hay varias formas de crear una PR, por ejemplo, desde github.com o con la interfaz de línea de comandos de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que cree la PR y asocie la incidencia correspondiente.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Asegúrate de que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte.
5. Observa cómo Copilot busca incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la solicitud de cambios, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador.
- has publicado la rama y abierto una solicitud de cambios.

Has usado un servidor MCP para probar una funcionalidad, pero MCP permite ampliar Copilot de muchas maneras. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
