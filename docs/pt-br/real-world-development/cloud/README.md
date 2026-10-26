---
slug: pt-br/real-world-development/cloud
title: "Agente do GitHub Copilot na nuvem"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente do GitHub Copilot na nuvem](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui o trabalho no GitHub, e o agente na nuvem começa a executá-lo em segundo plano — explorando o repositório, fazendo alterações e abrindo um pull request — enquanto você pode cuidar de outras tarefas.

Ao longo destes exercícios, você adicionará instruções personalizadas que o agente na nuvem seguirá e, em seguida, atribuirá uma issue do GitHub para ele implementar. Você revisará o trabalho, usará agentes personalizados para orientar a abordagem, acompanhará e direcionará sessões no painel de agentes e, por fim, revisará os pull requests e iterará sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas que o agente na nuvem seguirá |
| [2. Agente na nuvem][ex2] | Agente assíncrono | Atribua issues ao agente do GitHub Copilot na nuvem |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Acompanhe e direcione sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha as próximas etapas |

## Pré-requisitos

Antes de participar deste workshop, confirme que você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] O agente do Copilot na nuvem habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e recursos do Copilot CLI usados neste workshop — então você pode concluir todos os ambientes de trabalho com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente na nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — em todos, exceto no Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este ambiente não aborda MCP. Para explorar o uso de servidores MCP com o Copilot, consulte o [ambiente do CLI](../cli/) ou o [ambiente do VS Code](../vscode/).

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
