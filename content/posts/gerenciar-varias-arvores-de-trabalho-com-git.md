+++
title = "Gerenciar várias árvores de trabalho com git"
date = "2025-08-12"
draft = true
+++

Não é incomum na minha experiência surgir uma emergência no meio do desenvolvimento de uma
funcionalidade. Seja um erro crítico em produção, um pedido de um colega para revisar seu código
localmente, ou uma exigência não planejada de produto mais prioritária que a tarefa na qual estou
trabalhando.
Seja qual for a razão, devo deixar as alterações feitas até aquele momento para me dedicar à nova
demanda.

Ao longo do tempo lidei com esse tipo de situação de diferente maneiras, e aqui compartilho
brevemente cada uma delas.

## Git stash

Permite armazenar as alterações no diretório de trabalho em um espaço a parte que pode ser
restaurado mais tarde.

Sugiro a leitura da [documentação oficial do git-stash](https://git-scm.com/docs/git-stash/pt_BR), é
um conteúdo curto e que dá o necessário para dominar o comando.

## Múltiplos git clones do projeto

Permite criar um ambiente totalmente apartado para o desenvolvimento da nova demanda sem ter de
mexer em nada no diretório atual de trabalho.

Configurações do projeto deverão ser copiadas para o novo diretório caso se deva executar o projeto
localmente.

Consulte a [documentação oficial do git-clone](https://git-scm.com/docs/git-clone/pt_BR).

## Git worktree

Considero um passo além do uso de múltiplos git clones, pois o próprio git provê comandos para que
se possa navegar entre as árvores de trabalho e gerenciar todas as árvores de trabalho criadas.

Pode-se criar uma nova arvore de trabalho com `git worktree add`, listar as existentes com `git
worktree list` ou remover com `git worktree remove`.

Criei um simples atalho para automatizar a navegação entre as árvores de trabalho com o auxílio do `fzf` e `awk`.

```bash
cdgw () {
  cd "$(git worktree list | fzf | awk '{print $1}')"
}
```

Se quiser realmente dominar o uso de git-worktree, não deixe de consultar a [documentação
oficial](https://git-scm.com/docs/git-worktree/pt_BR).

## Git butler

Aplicativo de terceiro que permite o gerenciamento de branches virtuais através de interface gráfica.
Provê atualização das branches locais com o repositório remoto simultaneamente e resolução de
conflitos facilitada.

O git butler toma o uso do git, pois o utiliza como plataforma para prover suas funcionalidades. Isso
requer que a partir de sua instalação se utilize apenas ele para gerenciar o versionamento do projeto.

Não é tão mal quanto parece, pois é relativamente simples de reverter seu uso e voltar a utilizar o git
via terminal, por exemplo. O que não possível nem recomendável, é manter o uso do Git butler + git via
terminal.

Confira mais sobre o [Git butler](https://gitbutler.com/) no site oficial.


