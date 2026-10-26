---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente do Copilot na nuvem →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada maneira — ou há informações de contexto que o Copilot precisa conhecer —, esse contexto deve estar acessível. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não só *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos de instruções associados a caminhos;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e do CLI, aqui você não executará um prompt *antes e depois*: o agente do Copilot na nuvem trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante neste ambiente, ao revisar os pull requests criados pelo agente na nuvem.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- O início de cada arquivo deve conter um bloco de comentários que explique sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso permite orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações do projeto relevantes para a maioria das solicitações de chat ou CLI enviadas ao Copilot, como a stack de tecnologia, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer orientações para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um conjunto de testes unitários.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente do Copilot na nuvem usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado em orientações para todo o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e padrões gerais de codificação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Escreva em linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não existe uma única maneira de criar arquivos de instruções, assim como não existe uma única forma de usar IA. Com a experimentação, você descobrirá o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto consistente de arquivos de instruções. Ao explorar os arquivos deste projeto, você poderá notar que há orientações para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece isso de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface usada explicará esse recurso quando for relevante.
>
> Quer encontrar modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas diversas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore o conteúdo. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob, relativo à raiz do repositório, que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript que corresponda a `**/*.test.ts` será abrangido.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos que ele pode seguir ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionará um padrão para todo o projeto que determina que a documentação deve ser incluída no código como comentários TSDoc e cabeçalhos de comentários no nível do arquivo. O agente na nuvem adotará esse padrão ao trabalhar nas issues que você atribuir a ele nas próximas lições.

Antes de editar, crie um branch para trabalhar. O agente na nuvem lerá as instruções do branch de destino da issue, mas você também fará commit das alterações de forma organizada:

1. No terminal do codespace, crie e alterne para um novo branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como vimos, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de codificação do projeto, mas ainda não contém regras para documentação no código, motivo pelo qual o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo de nível do repositório é o local certo para padrões que se aplicam a todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Faça commit, push e merge da atualização das instruções

O agente na nuvem lê os arquivos de instruções do branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, o Copilot criará um branch a partir de `main`; portanto, suas alterações nas instruções precisam chegar a `main` para serem usadas.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie o branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request do branch `custom-instructions-cloud` para `main` e faça merge. Assim, o agente na nuvem poderá ler essas instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore a etapa do branch e faça o commit diretamente em `main`. A etapa de criação do branch serve para reproduzir a forma de trabalhar em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente na nuvem ao longo deste ambiente de trabalho.

Em seguida, [atribua sua primeira issue ao agente do Copilot na nuvem][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
