---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Como desarrollador, el flujo suele consistir en buscar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a actualizarlo... y así sucesivamente.

El modo agente de Copilot ayuda a gestionar este proceso.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo utilizar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, lo que permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados.
- cómo puede implementar nuevas funciones en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para obtener editores; ahora completarás con el modo agente de Copilot el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz de usuario en el cliente. Las funciones auxiliares reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia un único servidor de desarrollo de Astro. Puedes ejecutarlo en GitHub Codespaces así:

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro, `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL local en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar con los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La elección del modo y el modelo varía en cada ejercicio; los pasos correspondientes indicarán qué opciones elegir.

La primera versión del sitio web funciona, pero queremos añadir nuevas funciones. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en la lista de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y llama a herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo en concreto. Durante su desarrollo utilizamos Claude Sonnet 4.5 y por eso lo mencionamos en las instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias que recibas difieran de las que se describen en el taller.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar otro comando MCP y, aun así, completar la tarea.

3. Envía a Copilot esta solicitud para preguntarle por las incidencias del backlog:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según cuáles serán más útiles para los usuarios.
   ```

4. Selecciona **Continue** para permitir que ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio web.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Revisa las indicaciones generales para añadir funcionalidad, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar indicaciones, dividir instrucciones complejas en archivos más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar la función de filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- añadir o mejorar la lógica de filtrado de la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y asegurarse de que se superen antes de combinar los cambios en el código base. El modo agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura donde se muestra un ejemplo de selección del archivo de instrucciones de la interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** siga seleccionado en la lista de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que siga seleccionado **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia correspondiente del backlog:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editor y categoría según los requisitos de la incidencia correspondiente del backlog de GitHub. Ya existe una función auxiliar para obtener editores del ejercicio anterior; consérvala y mejórala si es necesario mientras completas el trabajo pendiente de la capa de datos, la interfaz y las pruebas. Asegúrate de que se superen todas las pruebas antes de terminar. El servidor ya está en ejecución, así que no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y busca los archivos asociados a la funcionalidad. Debería encontrar tanto las funciones auxiliares de la capa de datos y la interfaz como las pruebas. Después, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura donde se muestra Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot llevará a cabo varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar varios minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de mantener o deshacer todos los cambios, así que no hace falta que selecciones esos botones mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del panel Copilot Chat con una solicitud de confirmación para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido:

- cómo el modo agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- cómo puede explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de ampliación: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que actualice el sitio:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si este será fijo.
- al preparar la solicitud, proporciona a Copilot las indicaciones necesarias para crear la implementación que necesitas.
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. ¡Es normal trabajar así con Copilot!

## Recursos

- [Modos ask, edit y agent de Copilot: para qué sirven y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
