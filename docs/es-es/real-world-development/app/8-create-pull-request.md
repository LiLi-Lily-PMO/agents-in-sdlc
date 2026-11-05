---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de aseguramiento de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- revisarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge únicamente después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control para autorizar la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la solicitud y resuelve los problemas que la bloquean: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y aplica un rebase a la rama cuando es necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar también requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Vamos a permitir que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge empezará. Realizará estas acciones:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, la incluirá en el cuerpo de la descripción.
- Aplicará un rebase a la rama o gestionará posibles conflictos de combinación con la rama de destino.
- Supervisará el proceso de CI para asegurarse de que se superen todas las comprobaciones.
- Supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot. Hará cambios para responder a esos comentarios.
- De forma opcional, combinará la PR automáticamente cuando todo se haya completado correctamente.

Vamos a permitir que Agent Merge también combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** esté marcada.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Combina la PR cuando se hayan superado todas las comprobaciones obligatorias y se hayan obtenido las aprobaciones. No eludas las protecciones del repositorio.
    ```

9. Sigue el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR sea **Merged**; que Agent Merge tenga permiso para combinar o siga trabajando no confirma que la combinación haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: la generación, las pruebas y la verificación del código, y ahora el flujo de trabajo de las solicitudes de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has revisado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás otro de triaje][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
