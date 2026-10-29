---
title: "Ejercicio 4: Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de Copilot en la nube que se encargara de tres tareas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes ver todas las tareas de los agentes con solicitudes de incorporación de cambios abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación del modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de alternancia, parecía lógico incluir esta funcionalidad. Orientarás al agente para que añada el modo claro además del alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Vamos a consultar el estado de todas las tareas asignadas al agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones que ha realizado Copilot, entre ellas:
   - obtener el código del repositorio;
   - crear el entorno de trabajo;
   - configurar servidores MCP;
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos exactos de la lista dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir supervisando el trabajo.

## Orientar al agente en la nube

Ya has visto las tareas activas. Ahora pediremos al agente que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el diálogo de orientación de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot comenzará a trabajar en la tarea actualizada. Incorporará la nueva solicitud después de completar el paso en el que estaba trabajando cuando enviaste el mensaje.

El trabajo puede llevar varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes realizar el seguimiento de tareas activas y completadas, revisar el trabajo y redirigir una tarea en curso para ajustar el alcance o dar indicaciones adicionales.

Has explorado estos conceptos:

- supervisar las tareas del agente en la nube desde mission control y la página de agentes;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
