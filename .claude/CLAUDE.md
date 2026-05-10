# GEMA Site — Constituição do Projeto

**Stack:** Jekyll 4 + Decap CMS v3 + GitHub Pages  
**URL:** https://glebstrauss.github.io/site_gema/  
**Repo:** https://github.com/Glebstrauss/site_gema  
**Branch de deploy:** `master` (GitHub Pages faz build direto)

---

## Mapa do Repositório

```
_layouts/default.html        # Layout base: nav, footer, scripts (JS de tema, hamburger, animações)
_includes/prof-card.html     # Card reutilizável de professor
_professores/*.md            # Um arquivo por membro da equipe (coleção Jekyll)
_data/
  publicacoes.yml            # Lista de publicações científicas
  premios.yml                # Lista de prêmios e distinções
  noticias.yml               # Lista de notícias
assets/css/style.css         # CSS único — ~370 linhas, variáveis em :root
assets/img/professores/      # Fotos dos professores
logo/logo-gema.svg           # Logo oficial
admin/index.html             # Painel Decap CMS
admin/config.yml             # Coleções e campos do CMS
index.html                   # Página inicial (hero + linhas + equipe + pubs + prêmios + notícias)
publicacoes.html             # Filtros JS + navegação por ano + scrollspy
professores.html             # Grid de membros + seção "faça parte"
premios.html                 # Stats bar + cards por ano
noticias.html                # Destaque + lista
404.html                     # Página de erro personalizada
IMPLEMENTACAO.md             # Plano original (histórico — já executado)
```

---

## Regras Invioláveis

### 1. `relative_url` em TUDO
```liquid
href="{{ '/pagina.html' | relative_url }}"   ✅
href="/pagina.html"                           ❌  quebra em /site_gema/
```

### 2. Strings vazias são truthy em Ruby/Jekyll
```liquid
{% if campo != "" %}   ✅  correto para strings opcionais
{% if campo %}         ❌  "" avalia como true no Ruby
```

### 3. Estrutura YAML — wrapper obrigatório
```yaml
items:              # obrigatório — Decap CMS depende disso
  - titulo: "..."
    ano: 2025
```

### 4. Tema escuro — não usar `--branco`/`--preto` como texto em seções escuras
Seções com fundo escuro (prêmios, footer) usam cores hardcoded (`rgba(255,255,255,.5)`) ou recebem `!important` no bloco `html.dark`. Ver `.css` linha ~310.

---

## Variáveis CSS

```css
--azul: #1C4EDD          /* cor primária */
--azul-escuro: #0f2e8a   /* hover do azul */
--azul-claro: #e8edfb    /* fundos claros / badges */
--azul-mid: #5578e8      /* texto em seções escuras */
--preto: #0d0d0d         /* texto principal (vira #e2e2ec no dark) */
--cinza: #555            /* texto secundário */
--cinza-claro: #f5f5f3   /* fundos de seção alternados */
--branco: #fff           /* fundo padrão (vira #0d0d18 no dark) */
--borda: rgba(28,78,221,0.12)
```

---

## Tema Escuro

- Classe `.dark` em `<html>` — aplicada via inline script no `<head>` (FOUC prevention)
- Toggle: botão `#nav-theme-btn` na nav
- Persistência: `localStorage.getItem('gema-theme')`
- Seções com `style="background:var(--preto)"` recebem `background:#09090f !important` no dark
- Texto branco em seções escuras: `.premio-nome`, `.noticia-destaque .noticia-titulo` — forçados com `!important`

---

## Convenções CSS

- Classes kebab-case com prefixo por domínio: `pub-`, `prof-`, `premio-`, `noticia-`, `nm-`, `nd-`
- Breakpoints: `@media(max-width:900px)` tablet, `@media(max-width:600px)` mobile
- Animações: `fade-in` + `fade-in--visible` via IntersectionObserver no `default.html`
- `@media(prefers-reduced-motion:reduce)` desabilita todas as transições

---

## Dados — Estrutura por Coleção

### `_data/publicacoes.yml`
```yaml
items:
  - titulo: "Título do Artigo"
    autores: "Sobrenome A, Sobrenome B"
    periodico: "Nome do Periódico"
    ano: 2025
    qualis: "Q1"          # Q1 | Q2 | Q3 | Q4 | ""
    doi: "https://doi.org/10.xxxx/xxxx"  # ou ""
```

### `_data/premios.yml`
```yaml
items:
  - nome: "Nome do Prêmio"
    entidade: "Organização"
    ano: 2025
    icone: "🏆"
    descricao: "Descrição breve."
```

### `_data/noticias.yml`
```yaml
items:
  - titulo: "Título"
    data: "Mai 2026"
    texto: "Texto resumido."
    link: ""              # URL ou ""
    destaque: false       # true em no máximo 1 item
```

### `_professores/nome-sobrenome.md`
```yaml
---
layout: null
nome: "Prof. Dr. Nome Sobrenome"
cargo: "Professor Titular"
tipo: "docente"           # docente | estudante
avatar: "NS"
foto: ""                  # caminho relativo ou ""
descricao: "Área de atuação."
tags: [Genética Quantitativa]
lattes: "http://lattes.cnpq.br/..."
email: "nome@ufba.br"
ordem: 1
---
```

---

## Deploy

```bash
git add <arquivos>
git commit -m "mensagem em português"
git push origin master
# GitHub Pages publica em ~60 segundos
```

**Nunca:** `--no-verify`, `--force`, `git reset --hard` sem confirmação explícita.  
**Commits:** sempre em português. Co-Authored-By ao final.

---

## Comandos disponíveis (`/nome`)

| Comando | O que faz |
|---|---|
| `/melhorar` | Ciclo de melhorias iterativas no site |
| `/publicacao` | Adiciona publicação científica |
| `/professor` | Adiciona/atualiza membro da equipe |
| `/noticia` | Adiciona notícia ao site |
| `/revisar` | Auditoria completa do site |
