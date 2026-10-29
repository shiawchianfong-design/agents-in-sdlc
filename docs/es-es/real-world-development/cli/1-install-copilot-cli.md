---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Permite delegar tareas, solicitar cambios y mantener la concentración. Como cabe esperar, el primer paso es instalar la herramienta; por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalar GitHub Copilot CLI con npm;
- autenticarte con tu cuenta de GitHub;
- confiar en el repositorio del taller y probar una conversación rápida;
- localizar la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores ya trabajan habitualmente. En esta lección lo instalarás, te autenticarás y lo dejarás listo para usar durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet y Homebrew. GitHub Codespaces ya incluye Node.js, así que utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autenticarse con GitHub

La primera vez que lo inicies, Copilot CLI te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, quizá ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarizarte con la herramienta

Los comandos escritos en el prompt normal del shell se ejecutan directamente en tu Codespace. Después de iniciar Copilot CLI, los mensajes en lenguaje natural se envían al agente y los comandos de barra controlan la conversación.

1. Introduce `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Introduce `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz a Copilot una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Introduce `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que busque la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para buscar en este repositorio la incidencia titulada «Allow users to filter games by category and publisher». Dame la URL y un breve resumen. No hagas cambios.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Normalmente, Copilot CLI pregunta antes de usar herramientas fuera de los permisos establecidos. Para este taller, volverás a iniciarlo con `--yolo`, un atajo aprobado por ti que elimina esas solicitudes de aprobación en el Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado como un Codespace o una máquina virtual; nunca lo conviertas en un alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

Para este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que usarán las lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- instalaste GitHub Copilot CLI con npm;
- te autenticaste con tu cuenta de GitHub;
- confiaste en el repositorio del taller y probaste una conversación rápida;
- localizaste la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [empezarás tu primer cambio centrado][next-lesson] y usarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
