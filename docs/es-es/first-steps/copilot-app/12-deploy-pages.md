---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Ofrece al Space Quiz un hogar público con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte en la implementación, pero eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás la implementación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero la gente todavía necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible mediante una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare la implementación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este prompt. Si está seleccionada la opción **Approve all**, no esperes otro mensaje de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepara la implementación de GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo implemente en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Añade un README que describa la aplicación y cómo ejecutarla localmente si aún no existe. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso de Pages necesario. No combines la solicitud, no habilites la combinación automática, no cambies la configuración de Pages ni implementes el sitio. Detente y espera a que lo revise.
```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

Antes de autorizar la combinación, revisa la solicitud y el flujo de trabajo para confirmar que la implementación sigue el proceso previsto.

1. Confirma que el README solo se ha añadido si el repositorio no tenía ya uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación e implementación del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo de trabajo implementa desde la rama predeterminada y no desde una solicitud de incorporación de cambios que aún no se ha combinado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la combinación.

## Autoriza la combinación y la implementación

> [!WARNING]
> El siguiente prompt autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de revisar los cambios y confirmar el repositorio y la solicitud. La configuración de aprobaciones puede permitir estas acciones sin pedir otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt por separado:

```plaintext
He revisado la solicitud de incorporación de cambios <PR_NUMBER> en este repositorio y autorizo su combinación e implementación. Habilita GitHub Pages con GitHub Actions, combina únicamente esa solicitud cuando se hayan superado las comprobaciones obligatorias y supervisa la implementación hasta que termine correctamente. Indica la URL final del sitio web publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba la implementación

Sigue la ejecución de GitHub Actions y confirma que el sitio publicado está disponible en la URL indicada.

1. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la implementación se complete y el sitio publicado se cargue.

## Resumen y pasos siguientes

En esta lección:

- has pedido a Copilot que configure una implementación en GitHub Pages.
- has revisado el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarlos.
- has seguido la implementación y comprobado el sitio publicado.

Continúa con la [lección 13: Revisión y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
