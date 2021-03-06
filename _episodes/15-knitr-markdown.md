---
title: Producing Reports With knitr
teaching: 60
exercises: 15
questions:
- "How can I integrate software and reports?"
objectives:
- Value of reproducible reports
- Basics of Markdown
- R code chunks
- Chunk options
- Inline R code
- Other output formats
keypoints:
- "Mix reporting written in R Markdown with software written in R."
- "Specify chunk options to control formatting."
- "Use `knitr` to convert these documents into PDF and other formats."
---



## Relatórios de análise de dados

Os analistas de dados tendem a escrever muitos relatórios, descrevendo suas análises e resultados, para seus colaboradores ou para documentar seu trabalho para referência futura.

Quando eu estava começando, eu escrevia um script R com todo o meu trabalho, e apenas enviaria um e-mail para o meu colaborador, descrevendo os resultados e anexando vários gráficos. Ao discutir os resultados, muitas vezes haveria confusão sobre qual gráfico era qual.

Passei a escrever relatórios formais, com Word ou LaTeX, mas eu teria que gastar muito tempo para obter os números para olhar direito. Na maioria das vezes, a preocupação é sobre quebras de página.

Tudo é mais fácil agora quando eu crio uma página da web (como um arquivo html). Pode ser um fluxo longo, assim eu posso usar figuras altas que não caberiam normalmente em uma página. Rolagem é seu amigo.


## Programação alfabetizada

Idealmente, esses relatórios de análise são documentos _reprodutíveis_: Se um erro for descoberto ou se alguns assuntos adicionais forem adicionados aos dados, basta recopilar o relatório e obter os resultados novos ou corrigidos (versus reconstruir figuras, colá-las Em um documento Word, e mais editar a mão vários resultados detalhados).

A ferramenta chave para R é [knitr](https://yihui.name/knitr/), que permite que você crie um documento que é uma mistura de texto e alguns pedaços de código. Quando o documento é processado por knitr, pedaços de código R serão executados, e gráficos ou outros resultados inseridos.

Esse tipo de idéia tem sido chamada de "programação alfabetizada".

Knitr permite que você misture basicamente qualquer tipo de texto com qualquer tipo de código, mas recomendamos que você use R Markdown, que mistura Markdown com R. Markdown é uma linguagem de marcação leve para a criação de páginas da web.


## Criando um arquivo R Markdown

No R Studio, clique em Arquivo &rarr; Novo Arquivo &rarr; R Markdown e você receberá uma caixa de diálogo como esta:

![](http://swcarpentry.github.io/r-novice-gapminder/fig/New_R_Markdown.png)

Você pode ficar com o padrão (saída HTML), mas dar-lhe um título.


## Componentes básicos de R Markdown

O pedaço inicial do texto contém instruções para o R: você dá à coisa um título, autor e data, e diz que você vai querer produzir a saída em html (em outras palavras, uma página da web).

```
---
title: "Initial R Markdown document"
author: "Karl Broman"
date: "April 23, 2015"
output: html_document
---
```

Você pode excluir qualquer um desses campos se não quiser que eles sejam incluídos. As citações duplas não são estritamente _necessárias_ neste caso. Elas são principalmente necessárias se você quiser incluir dois pontos no título.

RStudio cria o documento com algum texto de exemplo para você começar. Observe abaixo que existem pedaços como

<pre>
&#96;&#96;&#96;{r}
summary(cars)
&#96;&#96;&#96;
</pre>

Estes são pedaços de código R que serão executados por knitr e substituídos por seus resultados. Mais sobre isso mais tarde.

Observe também o endereço da Web colocado entre colchetes angulares (`< >`) bem como os asteriscos duplos em `**Knit**`. Este é o [Markdown](http://daringfireball.net/projects/markdown/syntax).

## Markdown

Markdown é um sistema para escrever páginas da web, marcando o texto muito como você faria em um e-mail em vez de escrever código html. O texto marcado é _convertido_ em html, substituindo as marcas pelo código html apropriado.

Por enquanto, vamos excluir todas as coisas que estão lá e escrever um pouco de markdown.

Você faz coisas **em negrito** usando dois asteriscos, como este: `**negrito**`,
e você faz as coisas em _itálico_ usando underscores, como este:
`_itálico_`.

Você pode criar uma lista com marcadores escrevendo uma lista com hifens ou asteriscos, como este:

```
* negrito com asteriscos duplos
* itálico com sublinhados
* tipo de letra de código com acento grave
```

ou assim:

```
- negrito com asteriscos duplos
- itálico com sublinhados
- tipo de letra de código com acento grave
```

Cada um aparecerá como:

- negrito com asteriscos duplos
- itálico com sublinhados
- tipo de código fonte com acento grave

(Eu mesmo prefiro hífens do que asteriscos.)

Você pode fazer uma lista numerada apenas usando números. Você pode usar o mesmo número repetidamente se você quiser:

```
1. negrito com asteriscos duplos
1. itálico com sublinhados
1. fonte de tipo de código com acento grave
```

Isso aparecerá como:

1. Negrito com asteriscos duplos
1. Itálico com sublinhados
1. Tipo de código fonte com backticks

Você pode criar cabeçalhos de seção de tamanhos diferentes iniciando uma linha com algum número de `#` símbolos:

```
# Title
## Main section
### Sub-section
#### Sub-sub section
```

Você _compila_ o documento R Markdown para uma página da Web html clicando no "Knit HTML" no canto superior esquerdo. E observe o pequeno ponto de interrogação ao lado dele; clique no ponto de interrogação e você obterá uma "Referência Rápida de Markdown" (com a sintaxe de Markdown) bem como para a documentação do RStudio em R Markdown.

> ## Desafio
>
> Crie um novo documento R Markdown. Exclua todos os pedaços de código R > e escreva um pouco de Markdown (algumas seções, algum texto em itálico > e uma lista detalhada).
>
> Converter o documento em uma página da Web.
{: .challenge}


## Um pouco mais de Markdown

Você pode fazer um hyperlink como este:
`[text to show](http://the-web-page.com)`.

Você pode incluir um arquivo de imagem como este: `![caption](http://url/for/file)`

Você pode fazer subscritos (p.e., F~2~) com `F~2` e sobrescritos (p.e.,
F^2^) com `F^2^`.

Se você sabe como escrever equações no
[LaTeX](http://www.latex-project.org/), você ficará feliz em saber que você pode usar `$ $` e `$$ $$` para inserir equações matemáticas, como
`$E = mc^2$` e

```
$$y = \mu + \sum_{i=1}^p \beta_i x_i + \epsilon$$
```



## Pedaços de código R

Markdown é interessante e útil, mas o poder real vem de mixagem markdown com pedaços de código R. Este é R Markdown. Quando processado, o código R será executado; Se eles produzem números, os números serão inseridos no documento final.

Os pedaços de código principal têm esta aparência:

<pre>
&#96;&#96;&#96;{r load_data}
gapminder <- read.csv("~/Desktop/gapminder.csv")
&#96;&#96;&#96;
</pre>

Ou seja, você coloca um pedaço de código R entre <code>&#96;&#96;&#96;{r chunk_name}</code>
e <code>&#96;&#96;&#96;</code>. É uma boa idéia dar um nome a cada pedaço, pois eles o ajudarão a corrigir erros e, se houver algum gráfico, os nomes dos arquivos baseiam-se no nome do pedaço de código que os produziu.

> ## Desafio
>
> Adicionar pedaços de código a
>
> - Carregar o pacote ggplot2
> - Ler os dados do gapminder
> - Criar um gráfico
{: .challenge}

## Como as coisas são compiladas

Quando você pressiona o botão "Knit HTML", o documento R Markdown é processado por [knitr](https://yihui.name/knitr/) e um documento Markdown simples é produzido (bem como, potencialmente, um conjunto de arquivos de figuras): o código R é executado e substituído tanto pela entrada quanto pela saída; se os números são produzidos, as ligações a esses números estão incluídas.

Os documentos Markdown e figura são processados pela ferramenta [pandoc](pandoc.org), que converte o arquivo Markdown em um arquivo html, com as figuras incorporadas.


![](http://swcarpentry.github.io/r-novice-gapminder/fig/rmd-15-rmd_to_html_fig-1.png)


## Opções de bloco

Há uma variedade de opções para afetar como os pedaços de código são tratados.

- Use `echo=FALSE` para evitar que o próprio código seja exibido.
- Use `results="hide"` para evitar a impressão de resultados.
- Use `eval=FALSE` para ter o código mostrado, mas não avaliado.
- Use `warning=FALSE` e `message=FALSE` para ocultar quaisquer avisos ou mensagens produzidas.
- Use `fig.height` e `fig.width` para controlar o tamanho das figuras produzidas (em polegadas).

Então você pode escrever:

<pre>
&#96;&#96;&#96;{r load_libraries, echo=FALSE, message=FALSE}
library("dplyr")
library("ggplot2")
&#96;&#96;&#96;
</pre>

Muitas vezes haverão opções específicas que você vai querer usar repetidamente; para isso, você pode definir opções _globais_ de pedaços, assim:

<pre>
&#96;&#96;&#96;{r global_options, echo=FALSE}
knitr::opts_chunk$set(fig.path="Figs/", message=FALSE, warning=FALSE,
                      echo=FALSE, results="hide", fig.width=11)
&#96;&#96;&#96;
</pre>

A opção `fig.path` define onde os números serão salvos. O `/`
aqui é realmente importante; sem ele, as figuras seriam salvas no lugar padrão, mas apenas com nomes que estão com `Figs`.

Se você tiver vários arquivos R Markdown em um diretório comum, você pode querer usar `fig.path` para definir prefixos separados para os nomes de arquivo de figura, como `fig.path="Figs/cleaning-"` e `fig.path="Figs/analysis-"`.


> ## Desafio
>
> Use opções de pedaço para controlar o tamanho de uma figura e ocultar > o código.
{: .challenge}


## Código R em linha

Você pode tornar reproduzíveis _todos_ os números do seu relatório. Use
<code>&#96;r</code> e <code>&#96;</code> para um pedaço de código em linha, da seguinte forma: <code>&#96;r round(some_value, 2)&#96;</code>. O código será executado e substituído pelo _valor_ do resultado.

Não deixe que esses pedaços em linha fiquem divididos entre linhas.

Talvez preceda o parágrafo com um pedaço de código maior que faz cálculos e define coisas, com `include=FALSE` para esse pedaço maior (que é o mesmo que `echo=FALSE` e `results="hide"`).

Eu sou muito particular sobre o arredondamento em tais situações. Eu posso querer 
`2.0`, mas `round(2.03, 1)` vai dar apenas `2`.

A função 
[`myround`](https://github.com/kbroman/broman/blob/master/R/myround.R)
no meu pacote [R/broman](https://github.com/kbroman) lida com isso.

> ## Desafio
>
> Experimente um pouco de código R em linha.
{: .challenge}


## Outras opções de saída

Você também pode converter R Markdown para um PDF ou um documento do Word. Clique no pequeno triângulo ao lado do botão "Knit HTML" para obter um menu suspenso. Ou você poderia colocar `pdf_documento` or `word_documento` no cabeçalho do arquivo.

> ## Dica: Criando documentos PDF
>
> A criação de documentos .pdf pode exigir a instalação de algum
> software extra. Se necessário, isso é detalhado em uma mensagem de 
> erro.
>
> Tex para windows está disponível [aqui](http://miktex.org/2.9/setup).
>
> Tex para mac está disponível [aqui](http://tug.org/mactex).
{: .callout}


## Bibliografia

- [Knitr in a knutshell tutorial](http://kbroman.org/knitr_knutshell)
- [Dynamic Documents with R and knitr](http://www.amazon.com/exec/obidos/ASIN/1482203537/7210-20) (livro)
- [R Markdown documentation](http://rmarkdown.rstudio.com)
- [R Markdown cheat sheet](http://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf)
* [Getting started with R Markdown](https://www.rstudio.com/resources/webinars/getting-started-with-r-markdown/)
* [Reproducible Reporting](https://www.rstudio.com/resources/webinars/reproducible-reporting/)
* [The Ecosystem of R Markdown](https://www.rstudio.com/resources/webinars/the-ecosystem-of-r-markdown/)
* [Introducing Bookdown](https://www.rstudio.com/resources/webinars/introducing-bookdown/)
