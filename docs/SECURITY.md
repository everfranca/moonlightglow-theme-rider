# SECURITY.md - MoonlightGlow for Rider

Politica e resultado de auditoria de seguranca deste repositorio.
Ultima auditoria completa: 2026-09-22. Escopo: todo o repo (sem .git).

## 1. Resultado resumido

Nenhum segredo, senha, token, e-mail ou path local encontrado nos arquivos.
Nenhum binario versionado. Plugin 100 por cento estatico
(JSON, XML, SVG), sem codigo executavel, sem rede, sem dependencias.
Risco geral: BAIXO. Dois pontos de atencao abaixo (itens 3 e 4).

## 2. Checagens executadas e resultado

| Numero | Checagem | Resultado |
|--------|----------|-----------|
| 1 | Segredos e senhas (password, secret, api key, private key, bearer) | LIMPO. Matches apenas na palavra token dentro do texto de politica do agents.md, sem valor real |
| 2 | E-mails nos arquivos | LIMPO, nenhum encontrado |
| 3 | Identidade git local (nome e e-mail) | ATENCAO, ver item 3 |
| 4 | Arquivo .gitignore | ATENCAO, nao existe, ver item 4 |
| 5 | Historico git (log, reflog, stash) | LIMPO, repo sem commits, sem stash |
| 6 | Remote origin | OK, git@github.com:everfranca/moonlightglow-theme-rider.git via SSH, sem token embutido |
| 7 | Hooks customizados em .git/hooks | LIMPO, apenas samples padrao |
| 8 | SVG ativo (script, handlers on*, href http, ENTITY, foreignObject) | LIMPO |
| 9 | XML com DOCTYPE ou ENTITY (risco XXE) | LIMPO, nenhum nos dois XMLs |
| 10 | theme.json com paths locais ou URLs | LIMPO |
| 11 | Usuario Windows ou paths absolutos nos arquivos | LIMPO, apenas o identificador publico everfranca no plugin id |
| 12 | Binarios (.jar, .zip, .exe, .dll) versionados | LIMPO, nenhum |
| 13 | plugin.xml (id unico, versao, idea-version 262 a 262.*) | OK |

## 3. Atencao: identidade git expoe e-mail pessoal

A identidade git local usa nome real e e-mail pessoal (nao noreply).
Todo commit publicado no GitHub expora esse e-mail publicamente.
Acao recomendada, no GitHub e local:

1. GitHub Settings, Emails: ativar Keep my email addresses private e
   Block command line pushes that expose my email.
2. Local, configurar o e-mail noreply do GitHub:
   git config user.email SEU_USUARIO@users.noreply.github.com
3. Fazer isso ANTES do primeiro push, pois historico publicado nao tem
   como ser apagado por completo.

## 4. Atencao: falta .gitignore

Sem .gitignore, um git add -A futuro pode versionar por engano o .jar
do plugin, pastas backup-*, .idea/ ou out/. Conteudo minimo sugerido:

```
*.jar
*.zip
backup-*/
.idea/
out/
*.iml
```

## 5. Regras permanentes

1. Nunca versionar ids pessoais, senhas, tokens, chaves de API ou
   qualquer informacao sensivel, em codigo, docs, logs, screenshots ou
   nomes de arquivo.
2. Token do Marketplace somente em variavel de ambiente ou secret de CI,
   com escopo minimo e rotacao periodica. Nunca no repositorio.
3. Screenshots para o Marketplace: revisar antes de cada upload
   (sem nome de usuario na title bar, sem paths locais, sem e-mails,
   sem conteudo proprietario, usar codigo de exemplo).
4. Build do .jar sempre a partir de src/main/resources, de forma
   reproduzivel. Publicar o hash SHA-256 junto da release.
5. Revisar idea-version a cada release nova do Rider para nao distribuir
   plugin incompatível.

## 6. Checagens manuais no GitHub (fora do alcance local)

1. Ativar secret scanning e push protection no repositorio.
2. Proteger a branch main (exigir pull request para merge).
3. Conta com 2FA ativo e minimo de colaboradores com escrita.
