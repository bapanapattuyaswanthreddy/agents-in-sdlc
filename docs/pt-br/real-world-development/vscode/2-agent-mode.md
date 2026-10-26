---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo geralmente exigem alterações em vários arquivos e a execução de operações como testes. No fluxo de trabalho de uma pessoa desenvolvedora, é comum localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... E a lista continua.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar de forma mais autônoma no IDE. Ele trabalha de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar de terem nomes parecidos, o modo de agente e o agente na nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem atua como um par de programação, trabalhando de forma assíncrona como uma pessoa da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar novas funcionalidades na interface e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer merge no código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um helper de editoras; agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site Tailspin Toys para entender as funcionalidades existentes.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir os detalhes de cada um. O site é um único aplicativo Astro que renderiza suas páginas como HTML estático durante o build. As páginas consultam um banco de dados SQLite local diretamente no frontmatter, usando o Drizzle ORM — não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada por meio de um pequeno elemento `<script>` do Astro, com escopo limitado e usando as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Depois do exercício anterior, você deverá estar no branch `main`.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para um novo branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, será exibida uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar o link da URL localhost no terminal do Codespace redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando o servidor Web.

### Explore o site

Depois que o site estiver em execução, explore suas funcionalidades. Os principais recursos incluem:

- **Página inicial:** exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo:** ao selecionar um jogo, você acessa uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará as opções relevantes.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot para mostrar esses itens.

1. Selecione **Agent** no menu suspenso de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações autonomamente em vários arquivos, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em detrimento de outro. Ao criar o workshop, usamos Claude Sonnet 4.5 e, por isso, ele aparece nas instruções. A intenção é que as sugestões de código sejam relativamente consistentes para proporcionar uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot talvez use um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog de issues enviando o prompt a seguir:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com aqueles que considera mais úteis para os usuários.

## Revise os arquivos de instruções

Antes de iniciar o agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades. Elas incluem:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para arquivos de instruções específicos de diferentes tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos complexos em partes menores e mais gerenciáveis e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações distintas no aplicativo:

- Adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento de filtragem.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de você fazer merge das alterações no código. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de exemplo da seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade conforme os requisitos da issue correspondente no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade. Ele deverá encontrar tanto os helpers da camada de dados quanto a interface e os testes. Em seguida, começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Dependendo da complexidade da tarefa e do código, isso pode levar alguns minutos. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração. Não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes até falhem durante o processo. Não há problema. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão concluída do modo de agente do Copilot no Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer merge no código.

Agora, vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Com as habilidades aprendidas neste exercício, peça ao Copilot que atualize o site para implementar paginação. Considere estes pontos:

- siga as práticas recomendadas existentes, inclusive o uso dos arquivos de instruções atuais;
- decida como implementar a paginação: permitir que as pessoas selecionem o tamanho da página ou defini-lo de forma fixa;
- ao criar o prompt, forneça as orientações necessárias para que o Copilot implemente a solução desejada;
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
