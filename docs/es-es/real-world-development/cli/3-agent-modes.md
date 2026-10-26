---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una pequeña funcionalidad al proyecto. Los cambios más complejos requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo de tu organización y ayudarte a crear lo que necesitas de la forma adecuada. En esta y otras lecciones seguirás un proceso de desarrollo típico con agentes: partirás de una incidencia, generarás una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, por último, la fusionarás en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma conversación y rama durante el flujo de la funcionalidad. Normalmente usarías distintas ramas o PR para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la nueva funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador abierto mediante el puerto reenviado.

Al avanzar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la aptitud `quality-checks`, añadirás validación con MCP, crearás un agente de control de calidad y abrirás la PR de la funcionalidad.

## Situación

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia pendiente describe la funcionalidad, pero antes de programarla hay que acordar algunos detalles, como combinar categorías. Usarás el modo Plan para resolver esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Partir de una incidencia que describa lo que hay que hacer.
2. Crear un plan de implementación.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Fusionar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son variantes del flujo anterior.

Este enfoque estándar permite comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que considera que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza con la incidencia

Antes de trabajar en los filtros, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de valoración con estrellas.
3. Si el terminal no está abierto, presiona <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Presiona <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Presiona <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Presiona <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o un número parecido). El carácter `#` permite incorporar al contexto una incidencia o PR de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir el enfoque de implementación antes de encargárselo a Copilot. Es recomendable dedicar tiempo a planificar cualquier tarea compleja. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este mensaje:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Es posible que Copilot te haga preguntas sobre el funcionamiento de los filtros. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté listo, presiona <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte del plan que no refleje tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, es hora de implementarlo. Deja que Copilot trabaje en ello con Autopilot.

Autopilot permite a Copilot iterar en el problema hasta que considera que ha terminado.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador situado debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar de inmediato; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y comprueba la implementación

Como ocurre con cualquier código, debes revisar el código generado antes de fusionarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona.

1. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, presiona <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en el navegador mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editor, además de las combinaciones acordadas en el plan.
4. Confirma que el comportamiento al restablecer los filtros y cuando no hay resultados coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una nueva conversación de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la nueva funcionalidad con Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador abierto mediante el puerto reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas al [usar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
