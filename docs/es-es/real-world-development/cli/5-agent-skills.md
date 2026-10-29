---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que limitarse a escribirlo. Hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son perfectas para este tipo de tareas. Ayudan a Copilot a entender cómo debe ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que un agente carga cuando los necesita. En esencia, una habilidad es una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye frontmatter con un nombre y una descripción, una explicación de lo que hace y orientaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad utiliza.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad que el equipo de Tailspin Toys creó para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo debe usar la habilidad.
3. Lee las instrucciones y fíjate en cómo guían a Copilot para ejecutar las pruebas y el lint.
4. Observa que todavía no hay una sección **Results output formatting** en la habilidad.

## Ejecuta la habilidad antes de cambiarla

Puedes llamar directamente a las habilidades desde Copilot CLI o pedir que se ejecuten con lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación del filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos mejorar el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados ha proporcionado la herramienta. Vamos a actualizar la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica cada comando que se haya ejecutado y si se ha superado, ha fallado o se ha bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo cuando la herramienta los comunique. Indica el siguiente paso para cada error o bloqueo y nunca presentes una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Una vez realizados los cambios, vamos a probarlos. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutar la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, dejarás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
