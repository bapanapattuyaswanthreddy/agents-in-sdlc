---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados e adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções, práticas recomendadas, padrões de código e conhecimento específico do domínio para orientar o trabalho de forma mais eficaz. As equipes podem transformar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e são criados para coordenar skills — e até outros agentes — durante a execução do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Como parte desse trabalho, ele poderia chamar skills, como uma skill de fluxo de trabalho de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira "certa" de criar um agente personalizado. Como em qualquer aplicação de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai explorar:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Comentários recentes de usuários apontaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e as cores do plano de fundo. Para atender a essa necessidade de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar isso o mais rápido possível. Para gerar essa funcionalidade, você usará um agente personalizado.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` são obrigatórios no frontmatter dos agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - As principais responsabilidades ao gerar código para um site acessível.
   - As práticas recomendadas de acessibilidade.
   - Exemplos de código HTML, CSS e JavaScript.
   - Uma lista de problemas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o local central para trabalhar com todos os agentes do seu ambiente. Nele, você pode atribuir tarefas ao agente do Copilot na nuvem, monitorá-las e redirecioná-las ou fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar a issue em branco.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição de agente na nuvem, com o agente personalizado e a opção Accessibility em destaque](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano!

## Resumo e próximas etapas

Este exercício apresentou os [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar certos tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, vamos [monitorar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente do GitHub Copilot na nuvem][previous-lesson] | [Próxima lição: Monitoramento e gerenciamento de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
