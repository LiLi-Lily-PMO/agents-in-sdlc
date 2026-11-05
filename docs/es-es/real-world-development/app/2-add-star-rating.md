---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de juegos y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has recorrido el espacio de trabajo y utilizado un chat rápido. Ahora iniciarás una **sesión de agente** y realizarás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración por estrellas en los datos, pero aún no se muestra en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una sesión de agente y conocerás su estructura.
- pedirás al agente que realice un cambio pequeño y concreto en el proyecto.
- revisarás el cambio en la vista de diferencias del espacio de trabajo.
- ejecutarás la aplicación en local para comprobar el cambio en el navegador.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño e independiente, perfecto para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión dispone de un **worktree y una rama de Git propios**, lo que permite ejecutar varias sesiones a la vez (por ejemplo, una para añadir una función y otra para corregir un error) sin que los cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio; selecciona una para cambiar a ella.

Dentro de una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** del agente mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Inicia una sesión y solicita el cambio

Iniciemos una sesión nueva para explorar el proyecto e implementar la funcionalidad. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Crearemos una sesión nueva para ese repositorio y solicitaremos el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona el repositorio `tailspin-toys`.
4. Elige un **new working tree** y el modo **Interactive** debajo del cuadro de solicitud. Envía esta solicitud:

    ```plaintext
    Muestra la propiedad starRating de cada juego sobre 5 en las tarjetas de la página de lista. Si la valoración es null, muestra "No rating yet". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar la solicitud a Copilot.

La aplicación Copilot empieza por crear un worktree nuevo, es decir, una copia aislada del proyecto. Después, explora el proyecto, localiza los archivos que debe actualizar para añadir la función y crea el código necesario. Ya has añadido una función con la aplicación Copilot.

## Revisa las diferencias

Hay que revisar todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Examinemos los cambios directamente en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal utilizado para mostrar los detalles de los juegos. Debería parecerse al siguiente bloque: muestra la valoración cuando existe y utiliza `No rating yet` cuando `starRating` es `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como Copilot, al igual que otras herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. No obstante, debería ser relativamente parecido al ejemplo.

## Comprueba los cambios

Revisa los resultados de las comprobaciones automatizadas del agente antes de abrir un navegador. Confirma que las pruebas cubren una valoración `starRating` numérica y la alternativa para `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por sentado que funciona. Pidamos a Copilot que abra el sitio web para poder examinar la interfaz de usuario actualizada. Para ello, le pediremos que inicie el sitio y lo abra en un lienzo del navegador.

> [!TIP]
> Un lienzo es un widget interactivo disponible dentro de la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero por ahora utilizaremos el lienzo de navegador integrado.

1. Envía esta solicitud a Copilot para que inicie la aplicación y abra la página en el lienzo del navegador:

    ```plaintext
    Inicia la aplicación y abre la página en el lienzo del navegador.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de los juegos con valoración muestren su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Detén el servidor de desarrollo y cierra el lienzo del navegador.
    ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora abrirás una solicitud de incorporación de cambios (PR) para integrar el nuevo código en el código base existente.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot indica que la política del repositorio exige vincular una incidencia antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR** situada justo encima del chat para abrir la solicitud en el panel de revisión. Puedes revisar la PR desde ahí.
6. Cuando estés conforme, selecciona **Ready to merge**.
7. En el cuadro de diálogo nuevo, selecciona **Merge pull request** para combinar la solicitud.

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- has iniciado una sesión de agente y conocido su estructura.
- has pedido al agente que realice un cambio pequeño y concreto en las tarjetas de juegos.
- has revisado el cambio en la vista de diferencias del espacio de trabajo.
- has ejecutado la aplicación en local para comprobar la valoración por estrellas en el navegador.
- has abierto la PR 1, revisado sus comprobaciones y autorizado explícitamente la combinación.

A continuación, [empezarás por la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para implementar una función mayor.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instala-y-configura-la-aplicacion-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
