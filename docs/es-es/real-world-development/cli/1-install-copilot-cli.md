---
title: "Lección 1 - Instala GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu codespace, familiarízate con el entorno y busca la incidencia de filtrado precargada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación breve.
- encontrarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI lleva esa capacidad a la terminal, donde ya trabajan habitualmente muchos desarrolladores. En esta lección lo instalarás, te autenticarás y lo dejarás listo para utilizarlo durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js preinstalado, utilizarás npm.

1. Vuelve a tu codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y que cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba que se haya instalado:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autentícate con GitHub

La primera vez que se inicia, Copilot CLI te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con código de dispositivo para iniciar sesión y autorizar Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponda a tu repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions** para indicar que confías en los archivos.

> [!NOTE]
> Es posible que ya estés autenticado mediante tu sesión de GitHub en el codespace. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarízate con el entorno

Los comandos que escribes en el indicador habitual del shell se ejecutan directamente en el codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y, a continuación, vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que todo funcione:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub esté disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Utiliza GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Indícame su URL y un breve resumen. No cambies nada.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Utiliza el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas que no estén dentro de los permisos establecidos. Para este taller, lo volverás a iniciar con `--yolo`, un acceso directo aprobado por el usuario que elimina esas solicitudes de aprobación en el codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` activa todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Utilízalo solo en un entorno aislado, como un codespace o una máquina virtual; nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que se utilizarán en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El codespace limita el acceso a tu equipo local, pero los recursos de GitHub a los que te autentiques siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta sencilla sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm.
- te has autenticado con tu cuenta de GitHub.
- has indicado que confías en el repositorio del taller y has probado una conversación breve.
- has encontrado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás tu primer cambio concreto][next-lesson] y utilizarás Copilot CLI para mostrar la valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
