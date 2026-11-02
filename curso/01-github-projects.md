# Parte 1: GitHub Projects

[Slides desta parte (PDF)](slides/parte-1-github-projects.pdf)

## O problema

Todo trabalho em grupo tem esta conversa:

> Quem ficou com o login?
> Eu fiz ontem.
> Eu também fiz.
> E o README?

Duas pessoas fizeram a mesma coisa e ninguém fez a outra. O GitHub Projects resolve isso deixando visível, no mesmo lugar do código, **quem está fazendo o quê**.

## Os conceitos

**Repositório** guarda o código. Dentro dele ficam as **issues** (tarefas) e os **pull requests** (propostas de mudança no código).

**Project** é um quadro ligado ao repositório. Cada card do quadro é uma issue ou um PR.

**Issue** é uma tarefa com dono. Uma issue boa tem três coisas:

- um **responsável** (assignee)
- um **status** no project (Todo, In Progress, Done)
- um **Pronto quando**: a frase que diz como saber que a tarefa acabou

Veja a diferença:

```markdown
## Arrumar o login
```

```markdown
## Filtro por horário
Turma da noite quer só caronas noturnas.

**Pronto quando**
- [ ] Filtra por manhã, tarde ou noite
```

A primeira ninguém sabe quando termina. A segunda dá para testar.

**Board** é a visão em colunas: uma coluna por status. Arrastar o card muda o status.

**Tabela** é a visão em planilha. Nela você cria **campos próprios**, como Prioridade, e preenche rápido.

Veja o board deste repositório para ter uma referência: aba **Projects** do [mural-de-caronas-ifsp](https://github.com/natansalvadorligabo/mural-de-caronas-ifsp).

---

## Atividade 1: repositório e project

1. Uma pessoa do grupo cria o repositório **público** do grupo.
2. **Settings → Collaborators**: convide os colegas. Cada um precisa aceitar o convite (chega por e-mail e aparece em github.com/notifications).
3. Aba **Projects** do repositório → **New project** → escolha **Board**.
4. No project: **⋯ → Settings → Manage access**. Convide os colegas com permissão **Write**.

**Pronto quando:** todo mundo do grupo consegue abrir o board, e o project aparece na aba Projects do repositório.

> Se o project não aparece na aba do repositório, abra o project, vá em **⋯ → Settings** e confira se o repositório está vinculado. Também dá para vincular pela aba Projects do repositório em **Link a project**.

---

## Atividade 2: issues do projeto

1. Escolham um tema para o projeto do grupo.
2. Criem pelo menos **5 issues**, cada uma com um **Pronto quando**.
3. Cada issue ganha uma **label** (bug, enhancement, documentation...) e um **responsável**.
4. Confiram o board. Se alguma issue não apareceu: **+ Add item** no rodapé da coluna, digite `#` e o número da issue.

**Pronto quando:** o board tem 5 ou mais cards, todos com label e responsável.

Dica: na tela da issue, na lateral direita, o campo **Projects** já permite escolher o project na criação. Assim ela cai direto no board.

---

## Atividade 3: organizar o board

Três regras para o board não virar bagunça:

- um responsável por card
- prioridade definida
- pouca coisa em In Progress (se tudo está em andamento, nada está)

1. No project: **⋯ → Settings → Custom fields → +**. Crie o campo **Prioridade**, tipo **Single select**, com as opções Alta, Média e Baixa.
2. **New view → Table**. Mostre a coluna Prioridade e preencha em cada issue.
3. Salve a view: **View → Save view**. Se não salvar, a próxima pessoa que abrir não vê a coluna.
4. Cada pessoa move **uma** issue sua para **In Progress**.

**Pronto quando:** toda issue tem prioridade e ninguém tem mais de um card em In Progress.

---

## Atividade 4: fechar a issue pelo fluxo

O caminho de uma issue até o Done:

```
Issue #4  →  Branch docs/readme  →  Pull request "Closes #4"  →  Merge na main  →  Done no board
```

Ninguém arrasta o card para Done. O GitHub faz isso sozinho.

1. Pegue a sua issue que está em In Progress.
2. Crie uma branch, faça um commit pequeno e dê push:

   ```bash
   git switch -c docs/readme
   git add README.md
   git commit -m "docs(readme): adiciona instruções de execução e contribuição"
   git push -u origin docs/readme
   ```

   Padrão de nome da branch: `tipo/descricao`, por exemplo `feat/filtro-horario`, `fix/link-rodape`, `docs/readme`.

3. No GitHub, abra o PR para a `main` e escreva na descrição:

   ```
   Closes #4
   ```

4. **Outra pessoa do grupo** revisa e faz o merge.

**Pronto quando:** a issue fechou e o card foi para Done sem ninguém arrastar.

### Por que funcionou

- `Closes #4` (também vale `Fixes #4` ou `Resolves #4`) liga o PR à issue. A issue só fecha quando o PR entra na **branch padrão**, normalmente a `main`. Merge em outra branch não fecha.
- O project tem um **workflow padrão** ligado: item fechado vai para Done. Confira em **⋯ → Workflows → Item closed**.

Exemplo real neste repositório: o [PR #7](https://github.com/natansalvadorligabo/mural-de-caronas-ifsp/pull/7) fechou a [issue #4](https://github.com/natansalvadorligabo/mural-de-caronas-ifsp/issues/4).

---

## Bônus: Insights

No project, o botão **Insights** gera gráficos a partir dos cards. O **Burn up** mostra quanto trabalho foi concluído ao longo do tempo comparado com o total aberto. Se a linha de concluído fica parada por semanas, o grupo travou em algum lugar. Você também pode criar gráficos próprios, por exemplo **tarefas por pessoa**, agrupando pelo campo Assignees.

## Próximo passo

[Parte 2: GitHub CLI e GitHub MCP](02-parte-2-gh-cli-e-mcp.md)
