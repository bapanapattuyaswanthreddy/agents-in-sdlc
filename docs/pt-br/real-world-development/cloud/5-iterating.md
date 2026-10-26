---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Vamos explorar as alterações de código e, se necessário, fornecer feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já foi destacado várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo habitual de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e o agente do GitHub Copilot na nuvem

Como o agente do Copilot na nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter tudo seguro. Entre elas:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **apenas** ao branch que usará para o código.
- O agente na nuvem é executado no GitHub Actions, onde cria um ambiente separado e temporário para trabalhar.
- Um humano precisa aprovar qualquer workflow do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revise a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente do GitHub Copilot na nuvem: o que adiciona documentação ao código. Você fará isso usando a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao explorar o PR, talvez você veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, inclusive a chamadas para servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente do Copilot na nuvem][agent-firewall].

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request intitulado algo como **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários de documentação TSDoc e outros tipos de documentação. As alterações exatas variam.

    Ao revisar as alterações, procure comentários de documentação TSDoc, convenções de TypeScript e cabeçalhos de comentários. Esses padrões vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Deve haver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Os workflows devem entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários — como faria com outras pessoas da equipe — no pull request ou em comentários associados a linhas do código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos indicar um texto exato para pedir alterações. Algumas ideias:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código com uma breve descrição da sua função.
- Adicionar comentários de documentação TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver no código.
2. Selecione **View Session** para observar o Copilot trabalhar. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando terminar, você poderá marcar o PR de rascunho como pronto e fazer merge dele no branch principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR gerado pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes de um jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Deve haver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. Os workflows devem entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode mudar para esse branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome do branch criado pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente do Copilot na nuvem para pedir atualizações. Por exemplo, talvez queira alterar a quantidade de jogos relacionados ou pedir cabeçalhos de comentários e comentários de documentação TSDoc. Lembre-se de que essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas. Como antes, basta adicionar um comentário na guia **Conversation**; o Copilot verá o comentário e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste, atribuído no Exercício 3, quanto o modo claro, solicitado no painel de agentes no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - Os componentes de interface de alternância entre modos.
   - O uso do armazenamento local para persistir as preferências do usuário.
   - As alterações de CSS ou estilo para os modos de alto contraste e claro.
   - Os atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - O código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Deve haver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Os workflows devem entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para esse branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome do branch criado pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as alternâncias dos modos de alto contraste e claro no navegador para confirmar que funcionam como esperado e que as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se você encontrar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário para pedir atualizações, como fez com os PRs anteriores.

## Exercício opcional — continue delegando

O agente na nuvem funciona melhor quando você pode atribuir a ele itens reais do backlog e continuar com outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente de trabalho com o agente na nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas fornecidos pelo repositório** para observar mais tarde como influenciam o resultado do agente na nuvem.
- **Atribuiu issues ao agente do GitHub Copilot na nuvem** e observou como ele configura o ambiente, planeja e executa o trabalho de forma assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando alternâncias para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade durante a execução.
- **Revisou e iterou sobre os pull requests do agente na nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o ambiente de trabalho com o agente na nuvem. Se quiser continuar explorando, os outros ambientes complementam o que você praticou:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Ambiente do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma integração opcional de `/delegate` com o agente na nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente na nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente do Copilot na nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente do Copilot na nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
