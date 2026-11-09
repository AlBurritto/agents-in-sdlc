---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos por añadir una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta y las próximas lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, por último, la combinarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo en la misma sesión. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- Iniciarás una sesión de agente nueva desde una incidencia de GitHub.
- Definirás los requisitos en modo **Plan**.
- Implementarás la funcionalidad con el modo **Autopilot**.
- Revisarás el código.
- Validarás manualmente la funcionalidad en un canvas de navegador.

Mientras sigues trabajando en la funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de escribir código. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos. De hecho, son aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia que describa el trabajo.
2. Elaborar un plan de lo que hay que crear.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad nueva.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y se complete correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido al anterior.

Seguir este enfoque permite comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina cuánta autonomía tiene el agente. Puedes establecerlo desde el menú desplegable situado debajo del cuadro de mensaje y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tu respuesta para continuar.
- **Plan**: el agente primero crea un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se haya combinado la PR de las valoraciones con estrellas y que la rama local `main` esté actualizada.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia esté adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, comprueba la configuración de PATH o del gestor de versiones de la aplicación y ciérrala y vuelve a abrirla por completo. Repite la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluya el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios al plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación cambia el modo. Confirma que el indicador muestre **Autopilot**.

Copilot empezará a implementar la funcionalidad. Verás cómo sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobarlo, la implementación puede empezar de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio. Examina el código y ejecuta el sitio para comprobar que todo funciona:

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Corrige los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenezca a este árbol de trabajo y no a una lección anterior.

## Explora la funcionalidad nueva

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un canvas de navegador.

1. Usa este mensaje para pedir a Copilot que inicie la aplicación y abra la página en el canvas:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coincidan. Quita el filtro y, después, selecciona una editorial y confirma que los resultados coincidan con ella.
4. Combina los filtros de categoría y editorial, y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo de la sesión y cierre el canvas:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las Lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código de filtrado, añadirás mejoras a la habilidad y al agente de QA y, después, crearás y combinarás la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva después de detener el servidor y volver a ella en la siguiente lección.

## Resumen y próximos pasos

Has usado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- Iniciaste una sesión de agente nueva desde una incidencia de GitHub.
- Definiste los requisitos en modo **Plan**.
- Implementaste la funcionalidad con el modo **Autopilot**.
- Revisaste el código.
- Validaste manualmente la funcionalidad en un canvas de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
