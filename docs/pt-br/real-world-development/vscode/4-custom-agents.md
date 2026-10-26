---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados e adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções, práticas recomendadas, padrões de código e conhecimento específico do domínio para orientar o Copilot a executar certos tipos de trabalho com mais eficiência. As equipes podem transformar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e são criados para coordenar skills — e até outros agentes — durante a execução do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Como parte desse trabalho, ele poderia chamar skills, como uma skill de fluxo de trabalho de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira "certa" de criar um agente personalizado. Como em qualquer aplicação de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Comentários recentes de usuários apontaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e as cores do plano de fundo. Para atender a essa necessidade de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar isso o mais rápido possível. Para gerar essa funcionalidade, você usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e alterne para um novo branch baseado em `main` para trabalhar na acessibilidade. Assim, o PR de filtragem do Exercício 3 permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` são obrigatórios no frontmatter dos agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - As principais responsabilidades ao gerar código para um site acessível.
   - As práticas recomendadas de acessibilidade.
   - Exemplos de código HTML, CSS e JavaScript.
   - Uma lista de problemas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará as opções relevantes.

1. Se **Agent** ainda não estiver selecionado, selecione-o no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho do arquivo e um diff para você revisar.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar os arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório — editará arquivos existentes, como componentes Astro e CSS, e talvez testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Este exercício apresentou os [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar certos tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson]: revisará as alterações enquanto acontecem e adicionará uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
