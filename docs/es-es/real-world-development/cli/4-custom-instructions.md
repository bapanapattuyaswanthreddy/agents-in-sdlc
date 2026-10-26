---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot tenga acceso a esas indicaciones. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado los filtros, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para garantizar que se respeten los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Situación

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones no evidentes, no repetir lo que ya hace el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Permiten indicar las convenciones de programación, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **cada** solicitud del repositorio. Debe contener información general relevante para la mayoría de las solicitudes enviadas a Copilot.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para lenguajes, tipos de archivos o tareas específicas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones de este proyecto

El proyecto de ejemplo ya incluye varios archivos de instrucciones. Antes de hacer cambios, explora su contenido para ver el efecto que tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y lee la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y archivos del proyecto. Así puedes dividir conjuntos de instrucciones grandes en otros más pequeños y reutilizables, y ofrecer a Copilot ejemplos que seguir.

## Actualiza las instrucciones para adaptarlas a las directrices del equipo

Aunque los archivos existentes son un buen punto de partida, aún queda algo por hacer. Modifica el archivo principal `copilot-instructions.md` para garantizar que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra y retoma la conversación de filtrado para que Copilot las cargue sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y lee los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI utiliza los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- has actualizado el archivo de instrucciones para garantizar que se respeten los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la aptitud reutilizable `quality-checks`][next-lesson] para garantizar que el lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
