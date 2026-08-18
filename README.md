# Introdução ao Quarto

Material do minicurso **Introdução ao Quarto**, de 5 horas, ministrado no
**Simpósio Norte-Nordeste de Bioinformática**.

🌐 **Site do curso:** <https://juliaapolonio.github.io/intro-to-quarto/>

---

## Primeiros passos

1. Clone este repositório e entre nele:

   ```bash
   git clone https://github.com/juliaapolonio/intro-to-quarto.git
   cd intro-to-quarto
   ```

2. Crie um ambiente conda a partir da receita:

   ```bash
   conda env create -f environment.yml
   ```

3. Ative o ambiente:

   ```bash
   conda activate quarto-env
   ```

4. Abra o Positron:

   ```bash
   positron .
   ```

Para conferir se está tudo certo: `quarto check`.

---

## Conteúdo

| Página | Arquivo |
|:--|:--|
| Início | [`index.qmd`](index.qmd) |
| Setup e materiais | [`setup.qmd`](setup.qmd) |
| Material do curso (conteúdo + soluções) | [`material/minicurso-quarto.qmd`](material/minicurso-quarto.qmd) |
| Exercícios | [`exercicios.qmd`](exercicios.qmd) |
| Slides da aula | [`slides/minicurso-quarto.qmd`](slides/minicurso-quarto.qmd) |
| Recursos | [`recursos.qmd`](recursos.qmd) |

### Ementa

| Bloco | Conteúdo | Tempo |
|:--|:--|--:|
| 1 | Introdução | 20 min |
| 2 | O Quarto | 25 min |
| 3 | Notação Markdown (Exercícios 1 e 2) | 70 min |
| — | ☕ Intervalo | 15 min |
| 4 | Header (Exercício 3) | 35 min |
| 5 | Personalização (Exercício 4) | 50 min |
| 6 | Code chunks (Exercício 5) | 70 min |
| 7 | Encerramento | 15 min |

---

## Estrutura do repositório

```
intro-to-quarto/
├── _quarto.yml                  # configuração do site (navbar, tema, formatos)
├── index.qmd                    # página inicial
├── setup.qmd                    # instalação e links dos materiais
├── exercicios.qmd               # enunciados dos 5 exercícios
├── recursos.qmd                 # leituras e links
├── material/
│   └── minicurso-quarto.qmd     # conteúdo completo + soluções comentadas
├── slides/
│   ├── minicurso-quarto.qmd     # apresentação revealjs
│   └── custom.scss              # tema dos slides
├── img/
│   ├── logo.svg
│   └── favicon.svg
├── styles.css                   # CSS avulso — é a resposta do Exercício 4
├── styles.scss                  # tema do site
├── referencias.bib              # bibliografia
├── environment.yml              # receita do ambiente conda
├── CITATION.cff
└── .github/workflows/publish.yml
```

---

## Renderizando localmente

```bash
conda activate quarto-env

quarto preview                                # site inteiro, recarrega ao salvar
quarto render                                 # gera o site em _site/
quarto render material/minicurso-quarto.qmd   # só o material
quarto preview slides/minicurso-quarto.qmd    # apresentar / editar os slides
```

Para gerar uma versão **avulsa e autocontida** do material (um único `.html`
que você pode mandar por e-mail):

```bash
quarto render material/minicurso-quarto.qmd -M embed-resources:true
```

---

## Publicação

O site é publicado no GitHub Pages pela branch `gh-pages`.

### Configuração inicial (uma vez só)

1. No GitHub, vá em **Settings → Pages** e configure
   *Source: Deploy from a branch* → branch **`gh-pages`**, pasta **`/ (root)`**.
2. Em **Settings → Actions → General → Workflow permissions**, marque
   *Read and write permissions*.
3. Substitua `seu-usuario` por seu usuário do GitHub em:
   `_quarto.yml`, `README.md`, `setup.qmd`, `recursos.qmd` e `CITATION.cff`.

   ```bash
   grep -rl "seu-usuario" . --exclude-dir=.git \
     | xargs sed -i 's/seu-usuario/seu-usuario-real/g'
   ```

### Fluxo de publicação

O workflow em `.github/workflows/publish.yml` **não instala R**. Ele depende do
`execute: freeze: auto` (definido no `_quarto.yml`) e da pasta `_freeze/`, que
guarda os resultados já calculados.

Por isso, o fluxo correto é:

```bash
quarto render          # 1. renderiza localmente e atualiza _freeze/
git add _freeze/       # 2. commita os resultados congelados
git commit -m "Atualiza material"
git push               # 3. o Actions publica o site
```

> **Atenção:** se você alterar código nos `.qmd` e não renderizar localmente
> antes do push, o site publicado ficará com resultados desatualizados.

Se preferir que o próprio GitHub Actions execute o R (mais lento, porém à prova
de esquecimento), o workflow traz essa alternativa comentada ao final.

### Publicando manualmente

```bash
quarto publish gh-pages       # GitHub Pages
quarto publish quarto-pub     # Quarto Pub (mais simples, sem configuração)
quarto publish netlify        # Netlify
```

---

## Adaptando para outro evento

1. Troque o `title` e o `description` no `_quarto.yml`.
2. Ajuste `author` em `material/minicurso-quarto.qmd` e
   `slides/minicurso-quarto.qmd`.
3. Edite as cores em `styles.scss` e em `slides/custom.scss` — as variáveis
   estão no topo de cada arquivo.
4. Substitua `img/logo.svg` e `img/favicon.svg`.
5. Ajuste os tempos da agenda em `index.qmd` e no slide de agenda.

---

## Nota para quem for editar o material

Escrever um documento Quarto **sobre** Quarto tem uma armadilha: um chunk de
três crases dentro de outro bloco de código faz o `knitr` tentar executá-lo. O
material usa dois recursos de escape:

- ` ```{{r}} ` (chaves duplas) para **exibir** um chunk sem executá-lo;
- `knitr::inline_expr("...")` para exibir código inline sem executá-lo.

Se você adaptar o conteúdo, mantenha esse cuidado.

---

## Licença

- **Material didático** (textos, slides, exercícios):
  [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.pt-BR)
- **Código**: [MIT](https://opensource.org/licenses/MIT)

Reutilize, adapte e reministre à vontade — só cite a fonte.
