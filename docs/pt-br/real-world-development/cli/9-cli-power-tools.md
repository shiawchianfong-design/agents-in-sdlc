---
title: "Lição 9 - Comandos slash do GitHub Copilot CLI"
description: "Explore comandos slash para gerenciar o contexto, escolher modelos, compartilhar sessões e, opcionalmente, delegar tarefas à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos slash para interagir com ele. Esses comandos expõem recursos avançados, informações de bastidores e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para conhecer os modelos disponíveis;
- aprender como `/share` pode exportar ou compartilhar uma sessão;
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente na nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora vamos conhecer alguns recursos adicionais: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e, opcionalmente, delegação ao [agente na nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você poderá atingir o tamanho máximo da janela de contexto do modelo. Quando necessário, o Copilot CLI compacta a conversa automaticamente. Você também pode inspecionar ou compactar o contexto manualmente com comandos slash.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se enche. Use `/compact` para escolher o momento. Use `/clear` ou `/new` quando mudar para uma tarefa não relacionada e quiser começar uma conversa nova.

## Escolha um modelo

Os modelos têm pontos fortes diferentes, e cada pessoa tem suas próprias preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

O trabalho em equipe e o compartilhamento do que aprendemos ajudam todas as pessoas a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretende compartilhar.

## Opcional: amplie o trabalho ou delegue

O workshop principal está concluído. O Copilot CLI também inclui comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` pode criar uma worktree do Git isolada para outra tarefa;
- `/delegate` pode enviar uma tarefa ao agente na nuvem do Copilot, que trabalha de forma assíncrona e talvez abra um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, comece uma tarefa nova e com escopo bem definido e revise o resultado pelo fluxo normal. Se quiser aprofundar seu conhecimento sobre o trabalho assíncrono dos agentes, continue com o [workshop do agente na nuvem][cloud-workshop].

## Resumo e próximas etapas

Os comandos slash do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e entender o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para conhecer os modelos disponíveis;
- aprendeu como `/share` pode exportar ou compartilhar uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente na nuvem.

Há muitos outros comandos slash e recursos para explorar no Copilot CLI. Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e conhecendo as próximas etapas.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente na nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
