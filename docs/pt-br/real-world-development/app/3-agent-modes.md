---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar os filtros com base em uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para se integrar ao fluxo de trabalho existente da organização e ajudar a criar as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar uma nova funcionalidade, verificará se o código é válido e se a funcionalidade se comporta como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo de trabalho da funcionalidade. Em geral, você usaria sessões ou PRs diferentes para cada tipo de arquivo, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar a funcionalidade usando o modo **Autopilot**.
- revisar o código.
- validar manualmente a funcionalidade em um canvas de navegador.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Introduzir agentes de codificação com IA no seu fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo parecido:

1. Abra uma issue registrada com os detalhes do trabalho necessário.
2. Crie um plano do que será desenvolvido.
3. Crie e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, mescle o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você revisa e aprova o plano antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem aguardar orientação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Confirme que o PR das avaliações por estrelas foi mesclado e que a `main` local está atualizada antes de começar.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, em seguida, **Issues** na navegação superior do repositório. Abra **Allow users to filter games by category and publisher**. Essa issue já vem no backlog, portanto não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique a configuração do PATH ou do gerenciador de versões do aplicativo e feche-o completamente antes de reabri-lo. Tente novamente a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje a funcionalidade de filtros

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação da funcionalidade como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui filtros por categoria e editora, controles acessíveis, alterações na camada de acesso a dados e testes. Converse sobre qualquer comportamento que não esteja claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo restrito à implementação e aos testes dos filtros; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e as decisões combinadas para usá-las na validação posterior.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Verifique qual é o modo ativo em vez de presumir que a aprovação o altera. Confirme que o indicador de modo mostra **Autopilot**.

O Copilot começará a trabalhar na implementação. Você verá que ele seguirá o processo de forma iterativa, percorrendo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de mesclar, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação dos filtros e os testes.
2. Compare o resultado com a issue e com os esclarecimentos aprovados, incluindo combinações de várias categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Revise os resultados do lint, dos testes unitários, dos testes E2E e da verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Corrija as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a este worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use o seguinte prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro e, em seguida, selecione uma editora e confira os resultados.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado definido.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot para parar o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Não crie nem mescle o PR dos filtros ainda. Mantenha esta sessão, este worktree e este branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código dos filtros e adicionará melhorias para a skill e o agente de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximas etapas

Você usou diferentes modos de agente para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou a funcionalidade usando o modo **Autopilot**.
- revisou o código.
- validou manualmente a funcionalidade em um canvas de navegador.

Na próxima lição, vamos nos aprofundar na geração de código e garantir que ela siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
