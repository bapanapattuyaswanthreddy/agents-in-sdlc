---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla y añada funciones de accesibilidad, como los modos de alto contraste y claro. Vamos a revisar los cambios y, si es necesario, pedir a Copilot que mejore su trabajo.

### Situación

Los fundamentos del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre debemos revisar el código generado y seguir el proceso normal de DevOps. Revisaremos las propuestas de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se aplican restricciones para mantener la seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para sus cambios.
- El agente en la nube se ejecuta en GitHub Actions, en un entorno independiente y temporal.
- Una persona debe aprobar la ejecución de los flujos de trabajo de GitHub Actions.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de cambios (PR) que creó el agente en la nube de GitHub Copilot: añadir documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, quizá veas una advertencia de que el cortafuegos bloquea a GitHub Copilot. Es lo esperado: Copilot tiene un acceso limitado a recursos externos, incluidas las llamadas a servidores MCP. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios TSDoc y otra documentación. Los cambios exactos variarán.

    Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando termines de revisar los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios de línea del código, como harías con otros miembros del equipo. Copilot leerá los comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto exacto. Por ejemplo, puedes pedir a Copilot que:

- añada encabezados de comentarios breves a la parte superior de cada archivo de código.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que solicites un cambio a la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra sugerencia relacionada con la documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una nueva sesión para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te convenza. Cuando esté listo, marca la PR como lista para revisión y fusiónala en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR que creó Copilot para resolver la incidencia de mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada algo parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando termines de revisar los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador que muestra si hay flujos pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la función de juegos relacionados. Abre el terminal del Codespace y ejecuta los siguientes comandos; sustituye `<branch-name>` por el nombre de la rama de Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando y pedirle que ajuste, por ejemplo, el número de juegos relacionados o que añada encabezados y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Añade un comentario nuevo en la pestaña **Conversation** para solicitar estos cambios y Copilot iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. La PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada algo parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre los modos.
   - el almacenamiento local de las preferencias del usuario.
   - los cambios de CSS o de estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando termines de revisar los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador que muestra si hay flujos pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente las funciones de accesibilidad. Abre el terminal del Codespace y ejecuta los siguientes comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los conmutadores de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones según las prácticas recomendadas de accesibilidad. Si detectas algún problema o alguna mejora posible, puedes mencionar a **@copilot** en un comentario para pedir que los corrija, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para trabajar en incidencias reales y poder ocuparte de otras tareas mientras trabaja. Para acostumbrarte, crea otras incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para que los usuarios indiquen su interés en un juego, en la página de detalles.
- implementar paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **has inspeccionado los archivos de instrucciones personalizados del repositorio** y has observado cómo influyen en los resultados del agente.
- **has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **has creado y usado un agente personalizado** para implementar los modos de alto contraste y claro.
- **has usado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **has revisado e iterado sobre las solicitudes de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los demás entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: publica dos cambios revisados desde el terminal usando los modos Plan y Autopilot, aptitudes de agente, un agente de control de calidad personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
