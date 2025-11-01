+++
title = "Gerenciar várias árvores de trabalho com git"
date = "2025-08-12"
draft = false
+++

Você está imerso no desenvolvimento de uma funcionalidade, com várias alterações espalhadas por
diferentes arquivos, quando de repente: alguém grita "produção caiu!" ou seu colega pede
urgentemente uma revisão local de código. Talvez seja aquele pedido "super urgente" de produto que
não pode esperar.

Soa familiar?

Não é incomum na minha experiência surgir uma emergência no meio do desenvolvimento. Seja um erro
crítico em produção, um pedido de um colega para revisar seu código localmente, ou uma exigência
não planejada mais prioritária que a tarefa atual. Seja qual for a razão, preciso deixar as
alterações feitas até aquele momento para me dedicar à nova demanda.

O desafio é: como fazer isso sem perder o contexto do trabalho atual ou criar uma confusão no
versionamento?

Ao longo do tempo, experimentei diferentes abordagens para lidar com essa situação. Cada uma tem
seus prós e contras, e sua escolha depende do contexto e das suas preferências. Vamos a elas:

## Git stash: a solução rápida e prática

Esta foi minha primeira abordagem e, provavelmente, a mais conhecida entre os desenvolvedores. O
`git stash` permite armazenar temporariamente as alterações do seu diretório de trabalho em uma
espécie de "gaveta", liberando a árvore para que você possa trabalhar em outra coisa.

Quando a emergência passar, basta restaurar as alterações exatamente de onde parou. É simples,
rápido e não requer nenhuma configuração adicional.

**Ideal para:** interrupções rápidas onde você sabe que voltará ao trabalho original em breve.

Sugiro a leitura da [documentação oficial do git-stash](https://git-scm.com/docs/git-stash/pt_BR) —
é um conteúdo curto e suficiente para dominar o comando.

## Múltiplos git clones: isolamento completo

Conforme fui enfrentando situações mais complexas — onde precisava manter múltiplos contextos por
mais tempo — descobri que ter clones separados do mesmo repositório poderia ser útil.

A ideia é simples: cada clone é um ambiente completamente isolado. Você pode trabalhar na
branch principal em um diretório e na feature em outro, sem nenhuma interferência entre eles.

A desvantagem? Você precisará replicar configurações do projeto (variáveis de ambiente, dependências,
etc.) em cada clone. Além disso, não há integração entre os diretórios — são projetos independentes
do ponto de vista do sistema operacional.

**Ideal para:** quando você precisa manter múltiplos contextos ativos por períodos prolongados e não
se importa em gerenciar ambientes duplicados.

Consulte a [documentação oficial do git-clone](https://git-scm.com/docs/git-clone/pt_BR).

## Git worktree: o melhor dos dois mundos

Esta é, atualmente, minha abordagem favorita. O `git worktree` é como ter múltiplos clones, mas com
a inteligência do Git gerenciando tudo para você.

Com worktrees, você mantém o isolamento de diretórios separados, mas compartilha o mesmo repositório
Git subjacente. Isso significa que commits, branches e configurações do Git são compartilhados entre
todas as árvores de trabalho — você evita duplicação desnecessária.

Os comandos principais são intuitivos:
- `git worktree add` cria uma nova árvore de trabalho
- `git worktree list` lista todas as árvores existentes
- `git worktree remove` remove uma árvore quando não precisar mais dela

O que mais gosto é a facilidade de navegação. Criei um simples atalho para alternar entre worktrees
usando `fzf` e `awk`:

```bash
cdgw () {
  cd "$(git worktree list | fzf | awk '{print $1}')"
}
```

Com isso, posso visualizar e saltar entre contextos em segundos.

**Ideal para:** quem trabalha frequentemente com múltiplas branches e quer a conveniência de
diretórios separados sem a bagunça de clones duplicados.

Se quiser realmente dominar o uso de git-worktree, não deixe de consultar a [documentação
oficial](https://git-scm.com/docs/git-worktree/pt_BR).

## Git Butler: a interface gráfica que repensa o workflow

Para quem prefere interfaces gráficas ou quer experimentar um paradigma diferente, existe o GitButler.
Este aplicativo de terceiros introduz o conceito de "branches virtuais" com uma interface visual
intuitiva.

O GitButler oferece recursos interessantes: atualização simultânea de múltiplas branches locais com o
repositório remoto, resolução de conflitos facilitada, e uma visualização clara do estado de cada
contexto de trabalho.

Mas há um porém importante: o GitButler "assume o controle" do Git no seu projeto. Ele usa o Git como
plataforma subjacente, mas gerencia tudo através da própria interface. Isso significa que, uma vez
adotado, você deve usá-lo exclusivamente — misturar comandos Git via terminal com o GitButler não é
recomendável e pode causar problemas.

A boa notícia? Reverter o uso do GitButler e voltar ao Git tradicional é relativamente simples caso
você mude de ideia.

**Ideal para:** desenvolvedores que preferem interfaces gráficas, trabalham com muitas branches
simultaneamente, ou querem experimentar um workflow diferente do Git tradicional.

Confira mais sobre o [GitButler](https://gitbutler.com/) no site oficial.

## Qual escolher?

A resposta honesta? Depende do seu contexto e preferências.

Se você só precisa pausar rapidamente o trabalho atual, o **git stash** resolve na maioria dos casos.
É simples, direto, e não adiciona complexidade.

Se você trabalha constantemente alternando entre múltiplas tarefas e quer a conveniência de
diretórios isolados sem duplicação, o **git worktree** é provavelmente sua melhor opção (e minha
escolha pessoal).

Se prefere não usar a linha de comando ou quer experimentar um fluxo de trabalho mais visual, o
**GitButler** pode ser interessante.

O importante é conhecer as opções disponíveis e escolher conscientemente a ferramenta certa para cada
situação. Não existe uma resposta única — existe a resposta certa para você, no seu contexto, com
suas necessidades específicas.

E você, como lida com interrupções no meio do desenvolvimento? Usa alguma dessas abordagens ou tem
outra técnica? Adoraria saber nos comentários!
