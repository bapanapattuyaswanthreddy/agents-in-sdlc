---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información adicional, asegúrate de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que ofrecen directrices para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos específicos de rutas.
- generarás la primera parte de los datos de filtrado (una función auxiliar de acceso a datos para obtener editores) con las instrucciones actuales.
- añadirás un estándar global del repositorio a `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás en `main` la actualización de las instrucciones y la parte del filtrado para que Copilot use las directrices actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares indicados. Copilot es no determinista. El objetivo de este ejercicio es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado en cada carácter.

## Archivos de instrucciones

### Situación

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La parte superior de cada archivo debe incluir un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Permiten indicar convenciones de programación, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot para **cada** solicitud del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y otras directrices globales.
- `.github/instructions/*.instructions.md`, archivos que se pueden crear para tareas o tipos de archivos específicos. Por ejemplo, para orientar la programación en TypeScript o Astro, o para crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no explica en detalle cómo crear archivos de instrucciones, pero el proyecto de ejemplo presenta un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las indicaciones generales del proyecto, como una descripción de lo que se está creando, la estructura y los estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones sobre tipos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas específicas.
- Escribe las directrices en lenguaje natural y de forma clara. Incluye ejemplos de código correcto e incorrecto.

No hay una única forma correcta de crear archivos de instrucciones, del mismo modo que tampoco hay una sola forma de usar la IA. Con la experimentación descubrirás qué funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto quizá veas indicaciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando lo explicará cuando proceda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Lee los archivos incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y un conjunto de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas para flujos de trabajo de GitHub Actions. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, se aplica a los archivos de prueba TypeScript que coinciden con `**/*.test.ts`.
5. Observa las instrucciones para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes. Así puedes dividir directrices grandes en archivos pequeños y reutilizables, y proporcionar ejemplos a Copilot. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas sobre los comentarios TSDoc y los encabezados de comentarios.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, primero envía un mensaje con el archivo actual. Después lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas del editor de ejercicios anteriores para que Copilot solo use el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicarle a Copilot dónde debe estar la función auxiliar.
3. En la lista de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda cambiar archivos.

   ![Captura de la lista de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente al archivo de la función auxiliar y a sus pruebas.
6. Observa que la función auxiliar propuesta tiene tipos, recibe el cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario en el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, quizá añada comentarios de documentación sin que se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia al actualizar las instrucciones.

## Añade un nuevo estándar para todo el repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, que debería estar cerca de la línea 27. La sección documenta los estándares de programación, pero todavía no hay instrucciones sobre la documentación del código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en `.github/copilot-instructions.md` para ofrecer directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. Las normas globales, como la regla de comentarios que acabas de añadir, corresponden al archivo principal.

## Vuelve a enviar el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para dirigir la atención de Copilot al archivo correcto.
3. Envía exactamente el mismo mensaje de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que se usará en el siguiente ejercicio.

## Confirma los cambios de instrucciones y publica la rama

Los archivos de instrucciones se gestionan mediante control de código fuente, igual que los demás elementos del repositorio. Vamos a confirmar los cambios y publicar la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Ejecuta lo siguiente para confirmar que están presentes las instrucciones actualizadas y los cambios de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos modificados por Copilot para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones de archivos auxiliares (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de cambios

Una vez publicada la rama, crea una solicitud de cambios y relaciónala con la incidencia sobre los estándares de documentación de la lista de tareas. Copilot puede buscar la incidencia, crear una PR que la cierre y fusionarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub; no tienes que hacer nada más. La primera vez que Copilot use una, VS Code quizá te pida iniciar sesión. Sigue las indicaciones para autenticarte. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en Codespaces.
2. Presiona <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de las instrucciones y cree una PR desde la rama actual. Indica que debe describir tanto las instrucciones como la base de la función auxiliar para editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele que la fusione y que vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y fusionado una solicitud de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar los resultados con las instrucciones existentes.
- añadir un estándar global del repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar en `main` los cambios de instrucciones y la base de la función auxiliar para que el siguiente ejercicio pueda continuar.

A continuación, pondrás en práctica estas instrucciones en el [modo agente][next-lesson] para añadir una funcionalidad al sitio.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
