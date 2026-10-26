---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente do Copilot na nuvem que assumisse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas de agente com pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente na nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o trabalho já atualizava o estilo do site e adicionava a funcionalidade de alternância, pareceu lógico incluir esse recurso. Você vai orientar o trabalho do agente para que ele adicione tanto o modo claro quanto o modo de alto contraste.

## Revise as tarefas do agente do Copilot na nuvem

Vamos verificar o status das tarefas atribuídas ao agente do Copilot na nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - Atualizar a documentação do código.
    - Gerar APIs para modificar jogos.
    - Adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as etapas executadas pelo Copilot. Elas podem incluir:
    - Obter o código do repositório.
    - Criar o ambiente para o Copilot trabalhar.
    - Configurar servidores MCP.
    - Executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas da lista variam de acordo com o estado do trabalho do Copilot e a abordagem que ele adotou.

4. Observe também o painel de pull request (PR), que aparece à direita. Nele, você pode ver o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente na nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que inclua a alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à criação do modo de alto contraste. O título exato varia de acordo com o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente na nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa enquanto o Copilot trabalha em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa específica que estava executando quando você enviou a mensagem.

Isso levará alguns minutos, então aproveite para pausar e refletir sobre tudo o que aprendeu e explorou até agora.

## Resumo e próximas etapas

Este exercício apresentou a página de agentes do Copilot, o centro para monitorar e orientar tarefas do agente do GitHub Copilot na nuvem. Nesse painel central, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e até redirecionar uma tarefa para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usar o painel central e a página de agentes para monitorar tarefas do agente na nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
