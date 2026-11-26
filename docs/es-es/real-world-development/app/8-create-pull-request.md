---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas; después, crea una solicitud de incorporación de cambios (PR) y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Combinaste personalmente tu primera solicitud de incorporación de cambios (PR), la de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias solicitudes de incorporación de cambios (PR). Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una misma sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión lee la solicitud y resuelve los bloqueos: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar aún requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado el código. Permite que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí sola no lo inicia.

Agent Merge empezará a trabajar. Hará lo siguiente:

- Crear la solicitud con un título y una descripción.
- Si la sesión se inició desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción.
- Volver a aplicar la base o resolver posibles conflictos con la rama de destino.
- Supervisar la integración continua para confirmar que se superan todas las comprobaciones.
- Supervisar la PR por si recibe comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar cambios para responder a ellos.
- Combinar la PR automáticamente cuando todo se haya completado correctamente, si así se lo autorizas.

Pide a Agent Merge que combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la solicitud:

   ```plaintext
   Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
