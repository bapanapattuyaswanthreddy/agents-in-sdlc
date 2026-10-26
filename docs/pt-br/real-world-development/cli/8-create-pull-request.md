---
title: "Lição 8 - Crie e faça merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes. Em seguida, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em um único branch. É hora de revisar tudo em conjunto e abrir um pull request. Você fez merge do pull request (PR) de classificação por estrelas por conta própria; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e em um branch, e incluirá todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionar todas as alterações do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request. Ao habilitá-lo, o Copilot resolve os impedimentos do PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub, quando permitido pelas configurações do repositório.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisá-lo em conjunto, criar o PR e permitir que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que o Copilot exibiu uma mensagem semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório ou permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente o merge revisado quando as configurações do repositório permitirem.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação do código e, agora, o fluxo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionou todas as alterações do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [explorará outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
