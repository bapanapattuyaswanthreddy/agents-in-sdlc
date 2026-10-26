---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de tareas por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para actualizar código y añadir funciones de forma autónoma. Cuando termina, crea una solicitud de cambios preliminar para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones más importantes.

En este ejercicio explorarás cómo usar el agente en la nube de Copilot para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys quiere abordar parte de su deuda técnica. La primera versión del sitio, creada por contratistas, apenas tiene documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere que cada página de detalles de un juego muestre otros títulos de la misma categoría para que los usuarios puedan seguir explorando. No necesita una implementación definitiva todavía; quiere probar la experiencia de usuario y hacer pruebas de aceptación. Aunque es un bloqueo actual, hay otras incidencias más prioritarias.

Ambas tareas pueden quedar relegadas y son buenas candidatas para asignarlas al agente en la nube de Copilot. Mientras trabaja de forma asíncrona, puedes ocuparte de otras tareas y, después, revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al recibir una tarea, Copilot prepara un entorno, crea una solicitud de cambios preliminar y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando termina. Cuando la solución esté lista para revisar, Copilot te mencionará en la PR.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia ni elimina la necesidad de pensar. Incluso las operaciones aparentemente sencillas pueden ser complejas al analizarlas en detalle. Ten en cuenta las [prácticas recomendadas para definir tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador en pareja: divide el trabajo en fases, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian con la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has explorado cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio práctico de instrucciones personalizadas, has visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente en la nube, lee los archivos de instrucciones de este repositorio sin modificarlos para que después puedas reconocer su efecto.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en particular las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la sección siguiente, comprueba si la solicitud de cambios contiene comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentarios. Las instrucciones personalizadas influyen en el resultado. Volveremos a hablar de ellas al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y scripts de configuración. Lo mismo ocurre cuando asignas trabajo a Copilot, que realiza las tareas de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así tendrá acceso a las herramientas y dependencias necesarias. Este laboratorio ya incluye la configuración para que Copilot disponga de Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y cargada con datos de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a los pasos solo los permisos mínimos necesarios. Copilot recibirá un token propio para sus operaciones.
    permissions:
      # Para clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base de datos SQLite local para que haya datos durante la compilación y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene varios aspectos importantes:

- Incluye un solo trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona en lugar de esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos reconocemos la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica reduce la productividad y dificulta mantener la base de código o incorporar desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena tarea para asignarle. Trabajará en segundo plano y volverás más adelante para revisar los resultados.

1. En una pestaña nueva, ve a tu repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de cambios, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana.
12. En unos minutos debería aparecer una lista de tareas creada por Copilot.

> [!NOTE]
> Es posible que la lista de tareas tarde varios minutos en aparecer en la PR. Copilot está creando el entorno, ejecutando el flujo de trabajo descrito antes, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología de la PR y busca la actualización que indica que Copilot ha empezado a trabajar.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot explora el código, analiza el estado, decide un plan y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que trabaja de forma asíncrona y te permite centrarte en otras tareas. Aprovecharemos esa función asignando otra tarea al agente y, después, escribiremos código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

El agente en la nube te permite dividir el trabajo y ocuparte de una tarea mientras él se encarga de otra. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero podrías dedicarlo a otras tareas. Vamos a asignársela al agente.

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerán varios emojis 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente en la nube trabaja de forma parecida a un ingeniero de software, no tienes que supervisarlo continuamente: puedes revisar el resultado cuando termine. Ahora centraremos la atención en crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y se ocupe de deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

Has explorado estos temas:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y usar agentes personalizados. El agente en la nube también admite [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
