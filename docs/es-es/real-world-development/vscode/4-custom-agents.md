---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados que puedes adaptar a tareas o ámbitos concretos de tu flujo de desarrollo. Se definen mediante archivos Markdown en la carpeta `.github/agents` del repositorio. Puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para ayudar a Copilot a realizar determinados tipos de trabajo. Los equipos pueden convertir sus conocimientos en agentes reutilizables, como un agente de accesibilidad que aplique los requisitos de [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` o, globalmente, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con al menos `name` y `description`, seguido de un mensaje Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] tienen algunas similitudes: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La diferencia más sencilla es que el **agente personalizado** es el trabajador y las **aptitudes** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan aptitudes y otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Para ello, podría usar aptitudes como la de flujo de trabajo de solicitudes de cambios o la de ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera hasta encontrar lo que mejor funcione en tu entorno y tus situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys quiere que toda persona pueda usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han señalado que el tema oscuro actual les resulta difícil de leer porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha pedido implementar un modo de alto contraste que los usuarios puedan activar y desactivar.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad preparado. Vamos a revisar el contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace, abre un terminal y crea una rama nueva desde `main` para trabajar en accesibilidad, separada de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que se deben evitar.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados de `.github/agents` en la lista de agentes situada en la parte inferior de Copilot Chat. Puedes seleccionar uno para limitar la sesión a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

1. En la lista de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura de la lista de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista de agentes de la parte inferior de la vista Chat (muestra el agente activo, que de forma predeterminada es **default**).
3. Selecciona **Accessibility agent** en la lista.
4. Envía este mensaje al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará la base de código, propondrá cambios y los aplicará a los archivos. Cada edición aparecerá en el chat junto con la ruta del archivo y las diferencias para que puedas revisarlas.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> Es probable que el proceso tarde varios minutos. Copilot hará cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas.

En la lección siguiente revisarás y guiarás el trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados para tareas y ámbitos concretos. Te permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

Has explorado estos temas:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson]: revisarás los cambios y añadirás un interruptor del modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: prueba tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
