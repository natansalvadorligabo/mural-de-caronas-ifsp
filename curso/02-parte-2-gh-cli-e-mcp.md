# Parte 2: GitHub CLI e GitHub MCP

[Slides desta parte (PDF)](slides/parte-2-ia-e-mcp.pdf)

## Onde paramos

No fim da Parte 1, a issue foi fechada pelo PR e o card foi sozinho para Done.

Esta parte tem dois blocos:

1. **GitHub CLI (gh):** o mesmo fluxo da Parte 1, pelo terminal
2. **GitHub MCP:** entregar tarefas repetitivas para uma IA, com a sua revisão

Nos exemplos, troque `SEU_USUARIO`, `SEU_REPO`, os números de issue e PR e o número do project pelos seus.

---

## GitHub CLI: o board pelo terminal

Instale o gh: https://cli.github.com/ (no Windows: `winget install --id GitHub.cli`; no macOS: `brew install gh`).

```bash
gh auth login
gh auth refresh -s project
gh project list --owner @me
```

No `gh auth login`, escolha **GitHub.com**, **HTTPS** e **Login with a web browser**.

O login padrão não tem permissão para mexer em Projects. **Sem o escopo `project`, o gh não enxerga o board.** O `gh auth refresh -s project` adiciona esse escopo. Confira com `gh auth status`.

O `gh project list` mostra seus projects e o **número** de cada um. Você vai usar esse número nos próximos comandos.

## Issue direto no board

```bash
gh issue create --title "Filtro por horário" \
    --label enhancement --assignee @me \
    --project "Mural de Caronas"
gh project item-list 1 --owner @me
```

A issue já cai no board com label e responsável. `--project` recebe o **título** do project, e `item-list` recebe o **número**.

Para escrever o "Pronto quando", use `--body`, ou `--body-file issue.md` se o texto for grande.

> No PowerShell, troque a `\` do fim da linha por crase (`` ` ``) ou escreva o comando numa linha só.

Se uma issue ficou de fora do board:

```bash
gh project item-add 1 --owner @me --url https://github.com/SEU_USUARIO/SEU_REPO/issues/3
```

## Prioridade e status sem abrir o board

```bash
gh project item-edit 1 --owner @me --url URL_DA_ISSUE \
    --field "Prioridade" --value "Alta"
gh project item-edit 1 --owner @me --url URL_DA_ISSUE \
    --field "Status" --value "In Progress"
```

O `1` é o número do project, que aparece em `gh project list`. `URL_DA_ISSUE` é o endereço completo, por exemplo `https://github.com/SEU_USUARIO/SEU_REPO/issues/3`.

O campo Prioridade é o mesmo criado na Atividade 3. Se ainda não existe, crie pelo terminal:

```bash
gh project field-create 1 --owner @me --name "Prioridade" \
    --data-type SINGLE_SELECT --single-select-options "Alta,Média,Baixa"
```

Se o `item-edit` reclamar de `--field` ou `--value`, atualize o gh (`winget upgrade GitHub.cli` ou `brew upgrade gh`).

## Da issue ao Done

```bash
gh issue develop 3 --name feat/filtro-horario --checkout
gh pr create --title "Filtro por horário" --body "Closes #3"
gh pr review 8 --approve
gh pr merge 8 --squash --delete-branch
```

É o mesmo fluxo da Parte 1: o merge fecha a issue e o card vai para Done.

- `gh issue develop` cria a branch já ligada à issue e troca para ela. Entre ele e o `gh pr create` vem o de sempre: editar, `git add`, `git commit` e `git push -u origin feat/filtro-horario`.
- `gh pr review` e `gh pr merge` são feitos por **outra pessoa do grupo**. O GitHub não deixa você aprovar o seu próprio PR.
- O 8 é o número do PR. Ele aparece quando você roda `gh pr create` ou `gh pr list`.

## Atividade 5: uma issue só pelo terminal

1. Rode `gh auth refresh -s project` e confira com `gh auth status`.
2. Crie uma issue com `gh issue create`, com label, responsável e project.
3. Defina Prioridade e Status com `gh project item-edit`.
4. Feche pelo fluxo: `gh issue develop`, PR com `Closes #N` e `gh pr merge`.

**Pronto quando:** a issue foi criada, priorizada e fechada sem abrir o navegador.

### Cola rápida do gh

| Quero... | Comando |
|---|---|
| ver minhas issues | `gh issue list --assignee @me` |
| ver PRs esperando minha revisão | `gh pr list --search "review-requested:@me"` |
| ver o que mudou num PR | `gh pr diff 8` |
| testar um PR na minha máquina | `gh pr checkout 8` |
| comentar numa issue | `gh issue comment 3 --body "Comecei"` |
| abrir o project no navegador | `gh project view 1 --owner @me --web` |
| ajuda de qualquer comando | `gh <comando> --help` |

Manual completo: https://cli.github.com/manual/

---

## Quatro tarefas para a IA

1. **Templates de issue e PR**
2. **Feature vira issues**
3. **Triagem do board**
4. **Resumo do andamento**

São tarefas repetitivas, com formato claro e fáceis de revisar.

## O caminho até o GitHub

```
Claude Code  →  MCP do GitHub  →  API  →  Repo e project do grupo
 terminal        ferramentas     seu token       do grupo
```

**MCP** (Model Context Protocol) é um padrão para dar ferramentas a um assistente de IA. O [GitHub MCP Server](https://github.com/github/github-mcp-server) é o servidor oficial do GitHub. Ele expõe ferramentas como criar issue, listar itens do project e abrir PR, e a IA chama essas ferramentas usando **o seu token**. A API é a mesma que o gh usa por baixo.

## Só o que o token permite

| | |
|---|---|
| Toolsets | `default, projects` |
| Escopos | `repo, project` |
| Acesso | convite do grupo |

**Escopos.** Crie um token classic em GitHub → **Settings → Developer settings → Personal access tokens**, com `repo` e `project` e uma expiração curta, como 30 dias. Nunca cole o token em issue, commit ou chat.

**Toolsets.** Ligue o servidor no Claude Code rodando no terminal, fora do Claude Code:

```bash
claude mcp add-json github '{"type":"http","url":"https://api.githubcopilot.com/mcp/","headers":{"Authorization":"Bearer SEU_TOKEN","X-MCP-Toolsets":"default,projects"}}'
```

Se der `Invalid input` no Windows, use o formato antigo:

```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ -H "Authorization: Bearer SEU_TOKEN" -H "X-MCP-Toolsets: default,projects"
```

`default` traz repositórios, issues, pull requests e usuários. `projects` adiciona o GitHub Projects, que não vem ligado por padrão. Confira com `claude mcp list` e, dentro do Claude Code, com `/mcp`.

**Acesso.** O token só alcança o que a sua conta alcança. Para mexer no repositório do grupo, você precisa ter aceitado o convite de colaborador da Atividade 1.

Outros assistentes, como o VS Code com Copilot e o Cursor, usam o mesmo servidor: [guias de instalação](https://github.com/github/github-mcp-server/tree/main/docs/installation-guides).

## Demo: templates

```
> Crie templates de issue e PR com "Pronto quando"
  e "Closes #". Abra um PR.
```

Resultado: um PR com `.github/ISSUE_TEMPLATE/tarefa.md` e `.github/pull_request_template.md`.

## Demo: feature em issues

```
> Quebre "avaliar o motorista" em issues
  com "Pronto quando" no project.
```

Resultado: cards novos no board.

## Revise antes de aceitar

1. **Nada duplicado:** a IA não criou uma issue que já existia?
2. **Cabe numa branch:** cada issue é pequena o bastante para um PR só?
3. **Pronto quando testável:** dá para olhar e dizer se está pronto ou não?

## Demo: triagem

```
> Sugira Prioridade e Status das issues abertas.
  Aplique só após meu ok.
```

O board só muda depois do seu ok.

## Demo: resumo

```
> Resuma o project: feito, em andamento
  e parado. Publique como status update.
```

O resumo fica no project, visível para o grupo.

## Issue vaga, resposta vaga

```markdown
## Arrumar o login
```

```markdown
## Filtro por horário
**Pronto quando**
- [ ] Filtra por manhã, tarde ou noite
```

A IA não tem como adivinhar o que é "arrumar". Com o "Pronto quando", ela sabe onde parar.

## Discussão

1. O que a IA pode fazer sozinha?
2. O que passa por revisão?
3. Onde ela errou no seu teste?

## Leve para o projeto

1. **Templates no repositório**
2. **Board com prioridade**
3. **Toda mudança por PR**, inclusive as da IA

---

[Voltar ao início do curso](README.md)
