---
name: atualizar-sistema
description: >
  Atualiza APENAS o sistema do Sabec/Os (server, UI, launchers, skills, templates)
  no diretório do cliente, baixando o último estado do repo central do GitHub
  (DiogoSabec/sabec-os) sem tocar em dados, brand, memória ou identidade do
  cliente. Use quando o usuário rodar `/atualizar-sistema` dentro da pasta de
  um cliente (ex: ~/Documents/Empresas/Vanessa) e quiser puxar melhorias.
---

# /atualizar-sistema — sync do sistema central

Cada cliente do Sabec/Os é um clone privado com brand próprio, dados próprios
e memória própria. O sistema em si (server, UI, skills, templates) evolui no
repo central `DiogoSabec/sabec-os`. Essa skill puxa essas melhorias pro
cliente sem sobrescrever nada que é dele.

## Pré-checagem

### 1. Confere se é um cliente, não o sabec-os

Roda:

```bash
git remote -v
```

Se a saída mostrar `origin` apontando pra `DiogoSabec/sabec-os` ou
`mazzeoia/MazyOS`, **PARA**:

> "Isso aqui parece ser o repo central do Sabec/Os, não um cliente.
>  `/atualizar-sistema` é pra rodar dentro da pasta de um cliente
>  (ex: ~/Documents/Empresas/Vanessa). Verifica a pasta atual."

Se for um cliente real (origin é DiogoSabec/clinica-vanessa, ou outro repo
de cliente), pode seguir.

### 2. Confere se tem mudanças não-commitadas

Roda:

```bash
git status --porcelain
```

Se a saída não for vazia:

> "Você tem mudanças não-commitadas. O `/atualizar-sistema` vai mexer em
>  arquivos do sistema e melhor commitar o que tá em aberto antes pra não
>  misturar diff. Quer commitar agora? Posso te ajudar."

Espera o usuário decidir. Não roda commit automático em cima de WIP.

### 3. Confere se o sabec-os central tá acessível

Roda:

```bash
git ls-remote https://github.com/DiogoSabec/sabec-os.git HEAD
```

Se falhar (offline, sem acesso, repo privado sem auth), reporta erro claro:

> "Não consegui acessar o repo central do Sabec/Os
>  (github.com/DiogoSabec/sabec-os). Verifica conexão e permissões de
>  acesso. Não foi alterado nada."

## Fase 1 — Clone temporário

Cria pasta temp única:

```bash
TIMESTAMP=$(date +%s)
TMP_DIR="/tmp/sabec-os-update-$TIMESTAMP"
git clone --depth 1 https://github.com/DiogoSabec/sabec-os.git "$TMP_DIR"
SABEC_HASH=$(git -C "$TMP_DIR" rev-parse HEAD)
SABEC_HASH_SHORT=$(git -C "$TMP_DIR" rev-parse --short HEAD)
```

Guarda `$SABEC_HASH_SHORT` pro commit message no fim.

## Fase 2 — Checa se cliente já tá na última versão

Olha o último commit do cliente que veio do sistema:

```bash
git log --oneline -n 50 | grep "chore: atualiza sistema do sabec-os" | head -1
```

Se a linha contém o mesmo `$SABEC_HASH_SHORT`, responde:

> "O cliente já está na última versão do Sabec/Os (`<hash>`). Nada a
>  atualizar."

Limpa o `$TMP_DIR` e encerra.

## Fase 3 — Whitelist e blacklist

### SISTEMA (atualiza)

Arquivos individuais:

- `sabec-server.mjs`
- `sabec-ui.html`
- `Abrir sabecOS.command`
- `Abrir sabecOS.bat`
- `.gitignore`

Pastas (full sync — copia tudo, com merge especial pra skills):

- `templates/`
- `.claude/skills/`

Arquivos com merge especial (regras abaixo):

- `package.json`
- `CLAUDE.md`

### CLIENTE (não toca)

NUNCA copia nem sobrescreve:

- `brand.config.js` — cada cliente tem o próprio
- `_memoria/*` — dados do cliente
- `identidade/*` — cores, fontes, logo, brand book
- `REFERENCIAS/` — docs do cliente
- `marketing/`, `saidas/`, `dados/`, `pacientes/`, `clientes/` — conteúdo
  gerado e dados operacionais
- `*.code-workspace`
- `package-lock.json`
- `node_modules/`
- `.git/`
- `_inbox/`, `_arquivo/` (se existirem)

## Fase 4 — Detecta mudanças

Pra cada arquivo/pasta da whitelist, compara temp vs cliente e classifica:

- **modificado** — existe nos dois, conteúdo difere
- **novo** — existe no temp, não existe no cliente
- **removido** — existe no cliente, foi removido do sabec-os (só
  notifica, NÃO remove automaticamente — pode ser skill customizada do
  cliente)

Forma de comparar (pra arquivos individuais):

```bash
diff -q "$TMP_DIR/sabec-server.mjs" ./sabec-server.mjs
```

Pra pastas, usa `diff -rq` e parseia.

### Regra especial: skills customizadas

Pra `.claude/skills/<nome>/`:

- Se existe **só no cliente** (não no sabec-os) → **preserva** (é skill
  customizada do cliente, geralmente criada por `/mapear-rotinas`)
- Se existe **só no sabec-os** (não no cliente) → **adiciona** (skill nova
  do sistema)
- Se existe **nos dois** → marca como "modificada" se conteúdo difere e
  pergunta antes (pode ser que o cliente customizou a skill do sistema)

## Fase 5 — Resumo das mudanças

Mostra o resumo pro usuário:

```
sistema sabec-os: <hash atual no cliente> → <SABEC_HASH_SHORT> (último)

mudanças:
  modificados (N): sabec-server.mjs, sabec-ui.html, .claude/skills/carrossel/SKILL.md
  novos (M):       .claude/skills/<nova-skill>/SKILL.md, templates/<x>
  removidos (K):   templates/<y>  (não vou remover — só aviso)

  package.json:  vai atualizar type/devDependencies, preservar name
  CLAUDE.md:     vai atualizar parte genérica, preservar bloco do cliente

quer ver detalhes (diff arquivo por arquivo)? [s/n]
```

Se [s], mostra `diff` colorido por arquivo. Sugestão:

```bash
diff -u "<cliente-path>" "$TMP_DIR/<path>" | head -100
```

Depois pergunta:

> "Aplicar? [s/n]"

## Fase 6 — Aplicação

Só roda se o usuário disser [s].

### 6.1 — Arquivos individuais

```bash
cp "$TMP_DIR/sabec-server.mjs" ./sabec-server.mjs
cp "$TMP_DIR/sabec-ui.html" ./sabec-ui.html
cp "$TMP_DIR/Abrir sabecOS.command" "./Abrir sabecOS.command"
cp "$TMP_DIR/Abrir sabecOS.bat" "./Abrir sabecOS.bat"
cp "$TMP_DIR/.gitignore" ./.gitignore
```

Se algum arquivo do sistema foi editado manualmente no cliente (conflito
detectado na Fase 4 — o conteúdo do cliente diferia mas não tinha origem
upstream antes), AVISA antes de sobrescrever:

> "O arquivo `<x>` foi modificado manualmente no cliente. Sobrescrever
>  com a versão do sabec-os? Detalhe do diff: [...] [s/n]"

### 6.2 — Pastas (templates/, .claude/skills/)

Pra `templates/`: pode usar `rsync` com `--delete` cuidadoso, ou apenas
copiar over. Solução simples:

```bash
rsync -a --delete "$TMP_DIR/templates/" ./templates/
```

Pra `.claude/skills/`: NÃO usa `--delete` (pra preservar skills
customizadas do cliente). Faz cópia loop a loop, respeitando a regra de
skills customizadas da Fase 4:

```bash
for dir in "$TMP_DIR/.claude/skills/"*/; do
  skill_name=$(basename "$dir")
  rsync -a "$dir" "./.claude/skills/$skill_name/"
done
```

### 6.3 — package.json (merge)

Preserva o `name` do cliente, atualiza o resto. Usa `node` pra fazer um
merge limpo:

```bash
node -e "
const fs = require('fs');
const cliente = JSON.parse(fs.readFileSync('./package.json', 'utf8'));
const sistema = JSON.parse(fs.readFileSync('$TMP_DIR/package.json', 'utf8'));
sistema.name = cliente.name;
// preserva version do cliente se existir (cliente pode ter sua própria)
if (cliente.version) sistema.version = cliente.version;
fs.writeFileSync('./package.json', JSON.stringify(sistema, null, 2) + '\n');
"
```

### 6.4 — CLAUDE.md (merge com separador)

Convenção: o `CLAUDE.md` do sabec-os termina com uma linha `---` (três
hífens) como marcador de fim do bloco genérico. O cliente acrescenta o
bloco customizado abaixo desse separador (`/instalar` faz isso).

Lógica de merge:

1. Lê `CLAUDE.md` do sabec-os (`$TMP_DIR/CLAUDE.md`).
2. Lê `CLAUDE.md` do cliente.
3. No cliente, encontra a **última** linha `---` que aparece isolada
   (linha contendo só `---`).
4. Tudo depois dessa última linha é o "bloco do cliente" — preservar.
5. Resultado = conteúdo do sabec-os + bloco do cliente.

Edge case: se o sabec-os `CLAUDE.md` não termina com `---`, adiciona
`\n---\n\n` antes de prepender o bloco do cliente.

Edge case: se o cliente não tem nenhum `---` no `CLAUDE.md` (instalação
antiga), trata o `CLAUDE.md` atual do cliente como bloco genérico
(sobrescrevível), avisa e sobrescreve com o do sabec-os, depois
pergunta:

> "Esse cliente não tem bloco customizado de `CLAUDE.md` (sem `---`
>  separador). Vou sobrescrever com o do sabec-os. Se você tinha
>  customizações, elas estão no git histórico — posso restaurar.
>  Tudo certo? [s/n]"

Implementação com Node:

```bash
node -e "
const fs = require('fs');
let sistema = fs.readFileSync('$TMP_DIR/CLAUDE.md', 'utf8');
const cliente = fs.readFileSync('./CLAUDE.md', 'utf8');

// pega bloco do cliente (tudo após o último '---' isolado)
const lines = cliente.split('\n');
let sepIdx = -1;
for (let i = lines.length - 1; i >= 0; i--) {
  if (lines[i].trim() === '---') { sepIdx = i; break; }
}
const blocoCliente = sepIdx >= 0 ? lines.slice(sepIdx).join('\n') : '';

// garante que sistema termina com '---' isolado
if (!/\n---\s*$/.test(sistema.trimEnd())) {
  sistema = sistema.trimEnd() + '\n\n---\n';
}

const merged = blocoCliente
  ? sistema.trimEnd() + '\n\n' + blocoCliente.replace(/^---\n?/, '---\n')
  : sistema;

fs.writeFileSync('./CLAUDE.md', merged.endsWith('\n') ? merged : merged + '\n');
"
```

## Fase 7 — Commit

```bash
git add -A
git commit -m "chore: atualiza sistema do sabec-os $SABEC_HASH_SHORT"
```

Reporta sucesso:

> "Sistema atualizado pra `<SABEC_HASH_SHORT>`. Diff commitado.
>  Pra publicar: `git push`."

## Fase 8 — Limpeza

```bash
rm -rf "$TMP_DIR"
```

Sempre roda — mesmo se o usuário cancelou na Fase 5.

## Edge cases

- **Cliente offline**: erro na Fase Pré-checagem #3, nada é alterado
- **Sabec-os privado, sem auth**: mesma coisa, erro claro
- **Cliente sem .claude/skills/**: cria o diretório antes de sincronizar
- **Cliente sem templates/**: idem
- **Arquivo `Abrir sabecOS.command` perdeu o bit de executável após
  cópia**: roda `chmod +x "Abrir sabecOS.command"` no fim da Fase 6.1
- **Usuário cancela no meio**: não rola rollback (não foi commitado
  ainda). Próxima rodada vai detectar e oferecer de novo

## Regras

- Nunca toca em `brand.config.js`, `_memoria/`, `identidade/`,
  `REFERENCIAS/`, `marketing/`, `saidas/`, `dados/`, `pacientes/`,
  `clientes/`
- Não roda `git commit` automático em cima de mudanças não-relacionadas
  do cliente (a Pré-checagem #2 já bloqueia)
- Não remove arquivos do cliente que sumiram do sabec-os — só avisa.
  Skill customizada do cliente fica
- Sempre limpa `/tmp/sabec-os-update-*` no fim, mesmo em cancelamento
- Não atualiza `package-lock.json` — quem precisar roda `npm install`
- Toda saída direta, sem floreio, em português brasileiro
