---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais sólido. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a criar os recursos certos da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo comum de desenvolvimento orientado por agentes: começará com uma issue para criar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e o mesmo branch durante todo o fluxo de trabalho desse recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivo alterados. Neste workshop, vamos simplificar o processo para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa no Copilot a partir de uma issue do GitHub;
- definir os requisitos no modo Plan;
- implementar o novo recurso com o modo Autopilot;
- revisar o código;
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e quem visita o site precisa filtrar os jogos por categoria e editora. A issue no backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser acordados antes da codificação. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação de escopo definido com o modo Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda seus fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreve o trabalho necessário.
2. Planejar o que precisa ser criado.
3. Implementar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, fazer merge do código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue alguma variação do fluxo acima.

Seguir essa abordagem padrão garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o grau de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive:** você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de prosseguir.
- **Plan:** o agente cria primeiro um plano e não pode editar os arquivos do projeto.
- **Autopilot:** o agente trabalha autonomamente — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de classificação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie um branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para realçar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` ou um número semelhante. O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem de implementação de um recurso ou de execução de uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é sempre uma boa ideia reservar algum tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador de modo abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano. Primeiro, explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot talvez faça perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver concluído, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a página para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise as partes que não correspondem às suas decisões.

## Autorize o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção de nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano definido, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois de gerar o código, revise-o antes do merge, como faria com qualquer outro código. Vamos examinar o código e executar o site para confirmar que tudo funciona como esperado.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

O código parece bom, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e as combinações definidas no plano.
4. Confirme que o comportamento de limpar os filtros e os resultados vazios correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa no Copilot a partir de uma issue do GitHub;
- definiu os requisitos no modo Plan;
- implementou o novo recurso com o modo Autopilot;
- revisou o código;
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos entender melhor como o código é gerado e garantir que siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
