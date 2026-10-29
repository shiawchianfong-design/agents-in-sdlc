---
title: "Ejercicio 2: Añadir funcionalidad con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso la actualización más sencilla de una aplicación suele requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a actualizarlo. Y así sucesivamente.

Para eso sirve el modo Agente de Copilot.

El modo Agente de Copilot está diseñado para trabajar con mayor autonomía desde el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usar el modo Agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo Agente y el agente en la nube ofrecen experiencias distintas. El modo Agente trabaja en el IDE para que puedas iterar rápidamente e interactuar con él. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo Agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina cambios entre ellos;
- cómo puede implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos;
- cómo revisar antes de combinar los cambios y las pruebas generados por el modo Agente de Copilot.

## Escenario

A medida que crece la lista de juegos, queremos permitir que se filtren por categoría y editor. En el ejercicio anterior ya añadiste un helper de editores; ahora completarás con el modo Agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un marco de interfaz de usuario del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito específico que usa las API DOM estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en tu Codespace de GitHub:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo dentro del Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo de Astro, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Mientras el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funciones. Empezaremos explorando el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que la muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Al preparar el taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente uniformes y la experiencia sea más predecible. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que recibas sugerencias distintas. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog enviándole este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según lo útiles que considera que serán para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura;
   - principios para diseñar componentes, probarlos y garantizar su accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar y dividir conjuntos complejos en partes más manejables, además de proporcionar a Copilot ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado hay que hacer al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar y superar las pruebas antes de combinar los cambios. El modo Agente de Copilot puede realizar estas tareas. Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog con este mensaje:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los helpers de la capa de datos como la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, verás esos controles para todos los cambios; no tienes que seleccionarlos mientras Copilot siga trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot Chat en modo Agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo Agente de GitHub Copilot para añadir funcionalidades al sitio web Tailspin Toys. Hemos visto cómo:

- el modo Agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- el modo Agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo Agente antes de combinarlos en el código.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio para pedir a Copilot que actualice el sitio. Al diseñar la implementación:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación y si la persona usuaria podrá seleccionar el tamaño de página o será fijo;
- proporciona a Copilot las instrucciones necesarias para generar la implementación deseada;
- itera con GitHub Copilot y pide cambios o proporciona contexto cuando sea necesario. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
