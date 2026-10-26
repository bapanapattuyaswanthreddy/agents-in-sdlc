---
slug: es-es/real-world-development/cloud
title: "Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asigna trabajo desde GitHub y el agente en la nube lo realiza en segundo plano: explora el repositorio, hace cambios y abre una solicitud de cambios mientras tú sigues con otras tareas.

En estos ejercicios añadirás instrucciones personalizadas para el agente en la nube y, después, le asignarás una incidencia de GitHub para que implemente el trabajo. Revisarás y usarás agentes personalizados para orientar su enfoque, supervisarás y guiarás las sesiones desde el panel de agentes y, por último, revisarás sus solicitudes de cambios e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente en la nube de Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Gestión de agentes][ex4] | Supervisión | Supervisa y guía sesiones de agentes |
| [5. Iteración][ex5] | Revisión | Revisa las PR, itera sobre el trabajo de Copilot y decide los siguientes pasos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente en la nube de Copilot habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller, así que puedes completar todos los entornos con ese plan.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**, es decir, en todos excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este entorno no incluye MCP. Para explorar el uso de servidores MCP con Copilot, consulta el entorno de [CLI][cli] o el de [VS Code][vscode].

## Empieza

**[Empieza por el ejercicio 0: requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
