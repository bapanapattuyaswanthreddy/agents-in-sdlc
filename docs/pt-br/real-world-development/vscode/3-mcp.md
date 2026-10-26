---
title: "Exercício 3 - Testar sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela realmente funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Em seguida, publique o branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador;
- publicar o branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot faça isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com MCP, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome usando ferramentas.

Essas ferramentas e recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server):** fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode executar ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp):** oferece automação de navegador usando Playwright. O agente de IA pode navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições para o ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem com o template do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta a ele por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto à entrada `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar depois que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparecerá acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha confiar nele para permitir a inicialização.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code o iniciará — e pedirá que você confie nele — na próxima etapa, quando o Copilot precisar das ferramentas.

## Teste a filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar que um recurso funciona como esperado sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Isso significa que o agente analisa elementos estruturados e identificados, como botões, links e itens de lista, da mesma forma que uma tecnologia assistiva. Assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, percorrerá cada etapa e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` durante o teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique o branch e crie um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para a equipe revisar. Primeiro, publique o branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar o branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com ou pela interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, vamos pedir que ele crie o PR. Ele pode encontrar a issue correspondente e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute as operações necessárias. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se. Siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar manualmente a filtragem no navegador antes de publicar o código;
- publicou o branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
