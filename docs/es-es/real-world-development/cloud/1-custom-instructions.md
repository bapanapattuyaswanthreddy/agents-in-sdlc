---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: agente en la nube de Copilot →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] permiten proporcionar indicaciones para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás un mensaje «antes» y «después». El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, así que resulta más difícil mostrar sus efectos en tiempo real. Más adelante, al revisar las solicitudes de cambios creadas por el agente, verás cómo influye el archivo de instrucciones.

## Archivos de instrucciones

### Situación

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La parte superior de cada archivo debe incluir un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Permiten indicar las convenciones de programación, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot para **cada** solicitud del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Puedes usarlos, por ejemplo, para dar instrucciones sobre lenguajes como TypeScript o Astro, o sobre tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no trata en detalle cómo crear archivos de instrucciones, pero el proyecto de ejemplo ilustra un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las directrices generales del proyecto, como su descripción, su estructura y las normas de programación globales.
- Usa archivos `*.instructions.md` para dar instrucciones sobre tipos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas específicas.
- Escribe las indicaciones en lenguaje natural, de forma clara, e incluye ejemplos de código correcto e incorrecto.

No hay una única forma correcta de crear archivos de instrucciones, del mismo modo que tampoco hay una sola forma de usar la IA. Con la experimentación descubrirás qué funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá veas que incluyen indicaciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando lo explicará cuando proceda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

Explorarás estos temas:

- cómo se definen los archivos de instrucciones personalizadas.
- cómo añadir instrucciones al repositorio.

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y un conjunto de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas directrices se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un patrón glob (relativo a la raíz del repositorio) para seleccionar los archivos a los que se aplican las instrucciones. En este caso, se aplican a todos los archivos de prueba TypeScript, por ejemplo, los que coinciden con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir. Las rutas de esos enlaces son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas sobre los comentarios TSDoc y los encabezados de comentarios en los archivos.

## Añade un estándar nuevo para el repositorio

El siguiente paso es la única modificación que harás en este ejercicio: añadir una regla global que exija comentarios TSDoc y un encabezado de comentario en cada archivo. El agente en la nube aplicará la regla al trabajar en las incidencias que le asignes en los siguientes ejercicios.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios de forma ordenada:

1. Desde el terminal de Codespaces, crea y selecciona una rama nueva:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya se indicó, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación del proyecto, pero aún no hay reglas para documentar el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden encontrarse en la raíz del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas específicos. El archivo principal es el lugar adecuado para estándares que abarcan todo el proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, publica y fusiona las instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia a Copilot en el ejercicio siguiente, Copilot creará una rama a partir de `main`. Por tanto, los cambios en las instrucciones deben llegar a `main` para que el agente los use.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` hacia `main` y fusiónala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios en `main`. La rama permite seguir un proceso más parecido al de un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Durante el resto de este entorno, comprobarás cómo se aplica este estándar en las solicitudes de cambios que crea el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
