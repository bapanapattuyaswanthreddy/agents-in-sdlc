---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho em um navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — alternâncias para os modos de alto contraste e claro — e orientou a sessão em andamento para ampliar o trabalho. Agora é hora de publicar esse trabalho local e revisá-lo da mesma forma que sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a adição de IA generativa. Você ainda precisa de um ciclo real de revisão de tudo o que o Copilot produz. Com isso em mente, vamos enviar as alterações de acessibilidade do codespace, abrir um pull request e examinar o diff antes de envolver o restante da equipe.

## Publicar os recursos de acessibilidade

As alternâncias de alto contraste e modo claro que você implementou com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no codespace como alterações registradas em commits. Vamos publicá-las em uma branch e abrir um pull request para que o restante da equipe possa revisar.

1. Volte ao codespace.
2. Abra a visualização **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade estão registradas em commits. Se houver alterações do Exercício 5 sem commit, prepare-as e faça o commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite o aviso ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro (por exemplo, `Add high-contrast and light-mode toggles`) e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois que o PR estiver aberto, selecione a guia **Files changed** para revisar o trabalho de ponta a ponta. Preste atenção especial a:
   - Os componentes de interface para alternar entre os modos.
   - O uso do armazenamento local para persistir as preferências do usuário.
   - As alterações de CSS ou estilo para os modos de alto contraste e claro.
   - Os atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - O código JavaScript/TypeScript que gerencia a alternância de modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo correr bem, eles deverão passar.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR com uma solicitação como "revise este PR para identificar outros problemas de WCAG" ou "sugira melhorias na navegação por teclado". O Copilot iniciará uma nova sessão para atender ao comentário.

## Exercício opcional — continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que só se desenvolve com prática. Algumas ideias para sessões de acompanhamento no VS Code:

- Adicionar um formulário de interesse para apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- Ampliar o escopo do agente de acessibilidade — por exemplo, auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso do VS Code! Neste laboratório, você:

- **Usou o Playwright MCP para testar manualmente a funcionalidade.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Conduziu o modo de agente em alterações coordenadas por toda a pilha.** Adicionou uma funcionalidade de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente voltado à acessibilidade no seletor de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas, aceitou ou rejeitou cada uma e ampliou a sessão com um pedido de modo claro.
- **Concluiu o ciclo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta como faria sua equipe.

## Revisão e próximos passos

Este é o fim do percurso obrigatório do VS Code. Você pode parar aqui com o workshop concluído.

Se quiser conhecer outros recursos de agentes do Copilot, os outros percursos apresentam cenários relacionados em diferentes ambientes:

- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills, um agente personalizado de QA, Playwright MCP, Agent Merge e transferências de contexto.
- ☁️ **[Percurso do agente na nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente na nuvem, monitorar sessões na página de agentes e iterar de forma assíncrona em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que podem ser adaptados aos seus projetos.

Para uma extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
