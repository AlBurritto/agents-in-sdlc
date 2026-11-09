---
title: "Lección 8: Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Ya combinaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sesión y una rama, y todo el trabajo se incluirá en esta PR.

En esta lección:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys ahora quiere automatizar las tareas restantes de la PR y mantener el control del desarrollador sobre la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo activas, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a los comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después, combina la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado el código. Ahora permitirás que Agent Merge gestione el proceso de la PR:

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge se inicia y puede:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Incluir en el cuerpo de la descripción una referencia a la incidencia relacionada si iniciaste la sesión desde una incidencia.
- Volver a aplicar la base o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para asegurarse de que se superen todas las comprobaciones.
- Supervisar los comentarios de otros desarrolladores y de la revisión de código de Copilot, y realizar cambios para responder a ellos.
- Combinar automáticamente la PR cuando todo se haya completado correctamente, si autorizas esa acción.

Deja que Agent Merge combine la PR cuando se completen correctamente todas las comprobaciones:

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el proceso y resuelve cualquier bloqueo que se indique. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar, probar y validar código, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había combinado.

A continuación, [usarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
