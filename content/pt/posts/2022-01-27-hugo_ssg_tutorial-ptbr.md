+++
title = "Hugo Static Site Generator (SSG): um tutorial"
description = "Tutorial passo-a-passo de como eu criei meu site e blog mirianbr.com, usando Hugo com o tema Hello Friend NG"
type = "post"
tags = [
    "web-dev",
    "ssg",
    "hugo",
    "tutorials"
]
date = "2022-01-27"
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++

Este post é a tradução (minimamente adaptada) do [meu tutorial original em inglês](../../../posts/2021-12-17-hugo_ssg_tutorial).

Perguntaram para mim um dia desses sobre Geradores de Sites Estáticos, ou [Static Site Generators](https://jamstack.org/generators/) (SSGs) em inglês, e naquela hora eu tive que confessar que tinha pouca experiência prática com esse tipo de ferramenta.  *Sim*, eu até tinha criado meu [blog anterior](https://github.com/mirianbr/old-mirianbr.github.io) usando [Jekyll](https://jekyllrb.com/) e sua integração com o [Github Pages](https://pages.github.com/). Eu mantinha este blog atualizado online, criando e publicando diretamente na interface do Github. Mas é uma coisa muito diferente criar meu conteúdo localmente e ter mais controle da hospedagem do meu site (que hoje tenho, aqui no [mirianbr.com](https://mirianbr.com)).

Então, dessa vez decidi testar o SSG Hugo depois de testar [tantas](https://jekyllrb.com/) [opções](https://gohugo.io/) [tão](https://www.gatsbyjs.com/) [legais](https://vuepress.vuejs.org/) [disponíveis](https://blog.getpelican.com/).  Foi mais fácil do que imaginei que seria ter algo em pé que fosse simples, mas ainda assim visualmente atraente.

Neste post, eu documento todos os passos para mim mesma e para todos que quiserem fazer algo parecido sem muito esforço.  Continue lendo para ver!

![Vamos?](https://media.giphy.com/media/CjmvTCZf2U3p09Cn0h/giphy.gif)

## Configuração inicial básica

1. Faça download do [instalador do Hugo](https://github.com/gohugoio/hugo/releases) para sua plataforma; eu uso Windows, e acabei usando a versão estendida depois de tentar a versão padrão, por causa deste erro: ["TOCSS … this feature is not available in your current Hugo version"](https://gohugo.io/troubleshooting/faq/#i-get-this-feature-is-not-available-in-your-current-hugo-version)
2. Crie seu site digitando em sua linha de comando o comando a seguir: `hugo new site <nome de seu site>` (no meu caso, usei `home` como o nome de meu site)
3. Vá para o recém criado diretório `themes/`
3. Escolha e instale [um tema](https://themes.gohugo.io/); para [mirianbr.com](https://mirianbr.com), eu usei o tema minimalista [Hello Friend NG](https://github.com/rhazdon/hugo-theme-hello-friend-ng)
4. Configure o tema de seu site, autor, taxonomias, suas redes sociais para aparecerem na página de início e mais, tudo isso no arquivo `config.toml`, que é criado no diretório raiz de seu site (você pode ver [o meu arquivo config](https://github.com/mirianbr/mirianbr.com/blob/main/config.toml), e [este outro arquivo config de exemplo](https://github.com/rhazdon/hugo-theme-hello-friend-ng/blob/master/exampleSite/config.toml) para este tema que eu estou usando)

## Estrutura do site

Agora, pense sobre como você quer estruturar seu site.  Em meu caso, eu queria três páginas básicas além da inicial: Sobre, Blog e Projetos.  Isso é configurado no arquivo `config.toml` também.  Abaixo você pode ver um trecho do meu arquivo, onde eu criei esta estrutura:

```
[menu]
  [[menu.main]]
    identifier = "about"
    name       = "About"
    url        = "/about"
  [[menu.main]]
    identifier = "blog"
    name       = "Blog"
    url        = "/posts"
  [[menu.main]]
    identifier = "projects"
    name       = "Projects"
    url        = "/projects"
```

ATUALIZAÇÃO DE 27/01/2022: Este excerto é de antes de eu criar duas versões para meu site, uma em inglês e outra em português. A criação do menu agora com a localização é duplicada, como você pode ver [no meu arquivo config atual](https://github.com/mirianbr/mirianbr.com/blob/main/config.toml).

## Crie seu conteúdo!

Você agora tem tudo que precisa para começar a criar as páginas do seu site.  Seu conteúdo estará dentro do diretório  `content/`.  Armazene seus arquivos estáticos (suas imagens, mídia ou javascript, por exemplo) dentro de `static/`.

No meu site, criei dois arquivos Markdown em `content/`: `about.md` e `projects.md`.  O diretório `posts/` contém meus blog posts, um por arquivo.  Você pode ver todos eles no [repositório Github](https://github.com/mirianbr/mirianbr.com).

Usando Hugo, você pode criar suas páginas usando o comando `hugo new <nome-de-arquivo.md>`.  Você também pode criar os novos arquivos diretamente e adicionar um cabeçalho a eles, para que o Hugo saiba como interpretar este novo conteúdo.  Pode ser como o curto exemplo abaixo, do meu arquivo `about.md`:

```
---
title: "About"
date: 2021-12-12T15:48:31-03:00
draft: false
---
```

Também pode ser um pouco mais complicado e longo, como neste blog post:

```
+++
title = "Hugo Static Site Generator (SSG): a tutorial"
description = "A walkthrough on how I put my mirianbr.com website + blog online, using Hugo SSG with the Hello Friend NG theme"
type = "post"
tags = [
    "web-dev",
    "ssg",
    "hugo"
    "tutorials"
]
date = "2021-12-17"
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++
```

## Teste seu site localmente

Depois de você ter uma primeira versão pronta, é a hora de testar seu site localmente.  Você pode fazer isso com o comando abaixo em sua linha de comando:

```
hugo server -D
```

A flag `-D` é de draft, ou rascunho.  Ela vai fazer com que o Hugo apresente a você mesmo páginas que você marcou como draft.

Você verá então logs, que terminam com esta mensagem:

```
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Você poderá então testar o seu novo site no navegador, usando essa URL.

## Gere as páginas do seu site e publique-as

Depois de testar, você está feliz com o que vê?  Você pode então gerar as páginas HTML do seu site para publicá-lo externamente. Execute o comando abaixo no diretório raiz de seu site:

```
hugo
```

E... é isso!  O Hugo cria um diretório chamado `public/` que contém todo seu site.  Ele vai criar os arquivos HTML a partir de seus arquivos em Markdown.  Se você tem um blog e usa tags (ou categorias, que é outra forma de taxonomia com que o Hugo trabalha), ele vai criar páginas índice para todas as suas tags.

Você pode agora enviar os arquivos para seu servidor ou serviço de armazenagem.  Lembre de verificar as permissões do diretório do seu site e suas páginas -- e setar como [755 (executável)](https://stackoverflow.com/a/21344137) se necessário, para que as pessoas consigam acessá-lo.  E pronto, terminado!

![Super fácil!](https://media.giphy.com/media/3oKHWAk16aLhunLALm/giphy.gif)

Você gostou desse guia?  Lembra de mais alguma coisa que poderia ajudar mais pessoas neste tutorial?  Ou quer compartilhar seu próprio site feito com Hugo?

Envie um alô para hello@mirianbr.com! 👋🏼  Eu vou adorar saber o que você achou! 😊