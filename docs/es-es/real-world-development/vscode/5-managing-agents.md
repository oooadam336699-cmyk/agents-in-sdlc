---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición. Puedes revisar cada diff, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás al agente durante la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide añadir también un control para el modo claro, destinado a quienes consideran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura de archivo, edición y comando de terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá un diff:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y dejar que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Autoriza solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías recibir un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Una vez implementado el modo de alto contraste, amplía la misma conversación con un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

   ```plaintext
   Buen trabajo. Ahora añade un control para el modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón, persistir mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar de forma independiente el alto contraste y el modo claro.
   ```

2. El agente retomará el trabajo, explorará los cambios existentes y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición nueva como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te gusta—, detenlo con el botón **Stop** junto al cuadro de chat y envía un mensaje de aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes, corregir el rumbo o ampliar el alcance; por ejemplo, simplificar el CSS con variables en vez de selectores duplicados, integrar el modo claro en el panel existente o añadir atajos de teclado.

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa sus diffs. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones a medida que aparecían, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has orientado una sesión en curso con prompts de seguimiento.

En el [siguiente ejercicio][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
