---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. El flujo habitual de desarrollo consiste en localizar los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. El modo agente de Copilot ayuda a coordinar ese trabajo.

El modo agente trabaja de forma más autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. En este ejercicio lo utilizarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica archivos pertinentes y coordina cambios.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos con el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editor. En el ejercicio anterior ya añadiste un helper de editores; ahora completarás con el modo agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender las funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro y las API estándar del DOM.

### Inicia el sitio

Se ha proporcionado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL.
5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> Si se abre un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.
>
> En Codespaces, al seleccionar el enlace de localhost en el terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor web que se ejecuta en tu Codespace.

### Explora el sitio web

Cuando se esté ejecutando, podrás explorar las funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra información adicional, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

El sitio ya funciona, pero quieres añadir nuevas capacidades. Pide a GitHub Copilot que muestre las incidencias del backlog, que se creó automáticamente al generar el proyecto desde la plantilla.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo en particular. Se utilizó Claude Sonnet 4.5 al crear el taller para que las sugerencias fueran relativamente coherentes, pero, como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias varíen.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según la utilidad que considera que tienen para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, entre ellas:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes pequeñas y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado habrá que realizar al menos tres cambios en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas del comportamiento de filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.
4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editor y categoría según los requisitos de la incidencia relacionada de GitHub en el backlog. El helper de editores ya existe gracias al ejercicio anterior; consérvalo y mejóralo si hace falta mientras completas la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad, incluidos los helpers de datos, la interfaz y las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará, modificará archivos y ejecutará pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente sigue trabajando; al terminar podrás aceptar o deshacer todos los cambios.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
8. Es posible que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.
9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código que generan Copilot u otras herramientas de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has explorado cómo usar el modo agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. En concreto, has aprendido cómo:

- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- el modo agente explora el proyecto, identifica archivos pertinentes y coordina cambios.
- revisar los cambios y las pruebas generados antes de combinarlos con el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido y pide a Copilot que añada esta función. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo se implementará la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si este será fijo.
- Proporciona a Copilot la orientación necesaria para obtener la implementación que quieres.
- Es posible que tengas que iterar, pedir cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
