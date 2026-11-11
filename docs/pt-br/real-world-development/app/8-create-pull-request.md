---
title: "Lição 8 - Crie e mescle o PR da funcionalidade"
description: "Revise os filtros, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação dos filtros, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em um único branch. É hora de revisar tudo em conjunto e abrir um pull request. Você mesclou o pull request (PR) de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de trabalho de filtros e qualidade em uma única sessão e um único branch, e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar o PR completo da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de trabalho dos filtros, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para mesclar sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request, resolve o que estiver bloqueando — como verificações de integração contínua (CI) com falha, comentários de revisão e a necessidade de rebase — e faz a mesclagem assim que o GitHub permite. Ele trabalha em segundo plano, continua após a reinicialização do aplicativo e é desativado automaticamente quando o pull request é mesclado.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas sua capacidade de editar código e mesclar ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder a permissão de mesclagem.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge começará. Ele poderá:

- criar o pull request com título e descrição.
- mencionar a issue relacionada no corpo da descrição se você tiver iniciado a sessão a partir de uma issue.
- fazer rebase ou resolver possíveis conflitos de mesclagem com o branch de destino.
- acompanhar o processo de CI para confirmar que todas as verificações foram aprovadas.
- acompanhar o PR para detectar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer alterações para resolvê-los.
- mesclar o PR automaticamente quando todas as verificações forem concluídas com êxito, se você autorizar essa ação.

Vamos permitir que o Agent Merge também mescle o PR quando todas as verificações forem aprovadas.

5. Revise as diferenças do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para mesclar:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Antes de continuar, confirme que o status do PR é **Merged**; ter habilitado a permissão para mesclar ou ver um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximas etapas

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou o PR completo da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Na próxima lição, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
