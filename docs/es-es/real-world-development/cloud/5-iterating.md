---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades: añadir documentación al código, crear una sección de juegos relacionados y añadir funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios de código y, si hace falta, proporciona comentarios a Copilot para que mejore el resultado.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, revisa las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

El agente en la nube realiza tareas de forma asíncrona y sin supervisión, por lo que se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para su código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el acceso a recursos externos está limitado, incluidos los servidores MCP. Consulta cómo [configurar el firewall del agente en la nube](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que ha creado el agente en la nube de GitHub Copilot: la que añade documentación al código. Utiliza la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre una advertencia que indique que el firewall ha bloqueado a GitHub Copilot. Es lo esperado: por defecto, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [configurar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o más detallado.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios de documentación TSDoc nuevos. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario que indican los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, igual que harías con otra persona del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un prompt único que debas utilizar. Puedes pedirle, por ejemplo:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios de documentación TSDoc a archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier otra persona. Utiliza una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Si quieres volver a la solicitud, selecciona **Back to pull request**.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista y combinarla con la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o más detallado.
4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un indicador que señale que algunos flujos de trabajo están pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Las comprobaciones del proyecto deberían superarse para la aplicación Astro; pueden tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando y pedirle que cambie, por ejemplo, el número de juegos que muestra o que añada encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o más detallado.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los controles de la interfaz para cambiar de modo.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS o estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript o TypeScript que controla el cambio de modo.
6. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que algunos flujos de trabajo están pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Las comprobaciones del proyecto deberían superarse para la aplicación Astro; pueden tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta los comandos siguientes; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan como esperas y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para que implementara estas funciones según las prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando recibe incidencias reales del backlog y puedes ocuparte de otra cosa mientras trabaja. Para practicar, registra otras incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para indicar interés en un juego desde su página de detalles.
- Implementar paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estos ejercicios:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar más adelante su efecto en el resultado del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como Mission Control** para supervisar y orientar durante la ejecución la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y pasos siguientes

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asignar al agente una incidencia de refactorización o cobertura de pruebas.
- Crear un agente personalizado para otro ámbito.
- Configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
