---
title: "Ejercicio 2 - Añade funcionalidades con el modo agente"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones sencillas de una aplicación suelen afectar a varios archivos y requieren ejecutar operaciones como las pruebas. El flujo habitual de un desarrollador consiste en buscar los archivos pertinentes, hacer los cambios, ejecutar pruebas, depurar, descubrir qué archivo se ha pasado por alto, volver a modificarlo y repetir el proceso.

El modo agente de Copilot puede ayudarte con este trabajo.

El modo agente de Copilot funciona de forma autónoma en el IDE. Se parece al trabajo de un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige los errores que encuentra. Veamos cómo usarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos rápidos de comentarios y cambios. El agente en la nube funciona de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina los cambios.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y en la capa de datos.
- revisar los cambios y las pruebas que genera el modo agente antes de fusionarlos en la base de código.

## Situación

A medida que crece la lista de juegos, quieres permitir a los usuarios filtrar por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para obtener editores. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar una lista de juegos y ver sus detalles. Es una aplicación Astro que genera HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones auxiliares de acceso a datos reutilizables están en `src/lib/`; la interactividad se añade con scripts Astro `<script>` pequeños y acotados que usan las API DOM estándar.

### Inicia el sitio

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en GitHub Codespaces así:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá un mensaje con una URL parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Mantén presionada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace `localhost` desde el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al servidor web de tu Codespace.

### Explora el sitio

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

La primera versión del sitio funciona, pero queremos añadir funciones. Empecemos por revisar la lista de tareas. Al crear el proyecto a partir de la plantilla, se generó una lista de incidencias. Pide a GitHub Copilot que te la muestre.

1. En la lista de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de la lista de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan ningún modelo en particular. Usamos Claude Sonnet 4.5 al preparar el taller para que las sugerencias fueran relativamente coherentes, pero los modelos de lenguaje son probabilísticos y los resultados pueden variar. Es normal.

> [!NOTE]
> Debido al comportamiento probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP diferente y completar igualmente la tarea.

3. Envía este mensaje a Copilot:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que lista las incidencias.
5. Revisa la lista generada.

Observa que Copilot incluso ha ordenado las tareas según cuáles considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, facilidad de prueba y accesibilidad.
   - enlaces a instrucciones para distintos tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en archivos pequeños y proporcionar ejemplos y plantillas.

## Implementa los filtros

Para completar los filtros habrá que hacer al menos tres cambios distintos:

- añadir o mejorar la lógica de filtrado de la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de los filtros.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Antes de fusionar los cambios, también hay que ejecutar y superar las pruebas. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de la interfaz](../../../_images/ex3-select-instructions-file.png)

3. En la lista de agentes de la vista Chat, confirma que sigue seleccionado **Agent**.

   ![Captura de la lista de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y la base de código. Mientras trabaja, quizá veas los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer todos los cambios; no es necesario seleccionar esos botones mientras trabaja.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que el proceso se pause y que algunas pruebas fallen. No pasa nada: Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot en el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has añadido funcionalidades al sitio de Tailspin Toys con el modo agente de GitHub Copilot. Has aprendido cómo:

- el modo agente implementa funciones en la interfaz y en la capa de datos.
- el modo agente explora el proyecto, identifica archivos y coordina cambios.
- revisar los cambios y las pruebas generados antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crece la lista de juegos, será necesario añadir paginación. Usa lo que has aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes y usa los archivos de instrucciones.
- decide cómo implementar la paginación, por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo.
- incluye en el mensaje las indicaciones necesarias para obtener el resultado que quieres.
- quizá tengas que iterar con Copilot y pedir cambios, aportando contexto. Este es el flujo habitual de trabajo.

## Recursos

- [Modos ask, edit y agente de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
