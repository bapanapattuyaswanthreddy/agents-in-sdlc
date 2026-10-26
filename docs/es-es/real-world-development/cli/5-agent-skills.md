---
title: "Lección 5 - Personaliza y usa una aptitud de comprobación de calidad"
description: "Explora la aptitud existente quality-checks, personaliza el formato del informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para garantizar que siga nuestros estándares. Pero ¿qué hay de las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **aptitudes de agente** son ideales para estas tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas correctamente.

En esta lección:

- explorarás la aptitud existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar y ejecutar la aptitud.

## Situación

Tailspin Toys usa la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, son carpetas que contienen un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción de la aptitud, una descripción general de su función e indicaciones sobre cuándo debe usarse. También puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una aptitud no necesita carpetas ni archivos adicionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las aptitudes pueden encontrarse en la carpeta `.github/skills` del proyecto, para que el equipo las comparta y reutilice, o en la carpeta personal de aptitudes `~/.copilot/skills`.

## Explora la aptitud

Vamos a explorar la aptitud que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos. Se llama `quality-checks`.

1. Vuelve a tu Codespace y, en el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Observa que la aptitud todavía no incluye una sección **Results output formatting**.

## Ejecuta la aptitud antes de hacer cambios

Puedes llamar a las aptitudes desde Copilot CLI o pedirlo con lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Vuelve a la conversación de filtrado en el modo Interactive.
2. Usa este mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final.

## Personaliza el informe

Mejoremos el informe para que indique qué se ha ejecutado, si se ha superado y qué resultados han devuelto las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la aptitud actualizada

Copilot CLI puede volver a cargar las aptitudes editadas sin reiniciar la conversación. Veamos cómo funciona.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo mensaje que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutar la aptitud.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
