---
title: "Exercício 2 - Agente do GitHub Copilot na nuvem"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem problemas de dívida técnica. Pode ser uma vulnerabilidade de segurança não resolvida, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo. O agente do GitHub Copilot na nuvem foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando conclui o trabalho, ele cria um PR de rascunho para uma pessoa desenvolvedora revisar. Assim, tarefas repetitivas podem ser delegadas, o desenvolvimento pode avançar mais rapidamente e as pessoas desenvolvedoras podem se concentrar em questões mais amplas.

Com o agente do Copilot na nuvem, você vai explorar:

- como personalizar o ambiente para gerar código.
- como garantir que as operações sejam executadas com segurança.
- a importância de delimitar as issues com clareza.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe gostaria de adicionar comentários de documentação TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Ainda não é necessária uma implementação refinada; basta algo que a equipe possa usar para fazer testes de aceitação da experiência do usuário. A tarefa está bloqueada no momento, mas há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem acabar sendo adiadas e que são ótimas candidatas para atribuir ao agente do Copilot na nuvem. Ele pode trabalhar nelas de forma assíncrona enquanto a pessoa desenvolvedora se concentra em outras tarefas e, depois, revisar o trabalho para confirmar que tudo está como esperado.

## Conheça o agente do GitHub Copilot na nuvem

O [agente do GitHub Copilot na nuvem](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como ao trabalhar com uma pessoa da equipe, há várias maneiras de atribuir uma tarefa, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode abrir a sessão do Copilot enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request!

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot — você ainda precisa pensar. Até mesmo operações aparentemente simples se tornam complexas quando examinadas em detalhes; por isso, [delimite com cuidado as tarefas para o agente do Copilot na nuvem][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte a abordagem ao longo do caminho. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente do Copilot na nuvem, leia os arquivos de instruções já incluídos no repositório, sem fazer alterações, para observar depois como eles influenciam o trabalho.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe no pull request resultante os comentários de documentação TSDoc, as convenções de TypeScript e os cabeçalhos de comentários. Essas orientações vêm desses arquivos de instruções. Vamos voltar a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente do Copilot na nuvem

Criar código, independentemente de quem está executando a tarefa, geralmente requer um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em ordem. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes do início do trabalho. Isso permite disponibilizar as ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado para facilitar o laboratório e permitir essa experiência de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para as etapas. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e popule o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se o workflow é executado corretamente sem precisar esperar que o Copilot o inicie.

## Adicione documentação

Embora todos reconheçam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica muitas vezes não é resolvido, reduzindo a produtividade e dificultando a manutenção do código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue ideal para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) criado, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se o novo PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente, executando o workflow mencionado, analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que é possível acompanhar a sessão ao vivo e ver como o Copilot resolve o problema: ele explora o código e entende o estado atual, pausa para pensar e escolher uma abordagem e, em seguida, cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente do Copilot na nuvem é executar tarefas de forma assíncrona para que possamos nos concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente na nuvem e, em seguida, voltando nossa atenção para a criação de funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente do GitHub Copilot na nuvem é poder dividir o trabalho: você se concentra em um conjunto de tarefas enquanto o agente trabalha em outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas esse tempo poderia ser usado para outras tarefas. Vamos atribuir essa tarefa ao agente do Copilot na nuvem!

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. O agente do Copilot na nuvem funciona de forma semelhante a uma pessoa engenheira de software: você não precisa monitorar o trabalho continuamente, basta revisá-lo quando for concluído. Agora vamos nos concentrar em criar e usar agentes personalizados.

## Resumo e próximas etapas

Este exercício apresentou o [agente do GitHub Copilot na nuvem][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. O Copilot pode resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam executadas com segurança.
- delimitar issues com clareza.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos nos concentrar em criar e usar agentes personalizados. [O agente do Copilot na nuvem também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente do Copilot na nuvem][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente do Copilot na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
