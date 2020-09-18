---
title: Usando Jekyll e GitHub Pages
author: Murilo Costa
date: 2020-09-18 08:20:00 -0300
categories: [Blogging, Tutorial]
tags: [murilo, murilo-tech, jekyll, github, githubpages]
---

Como falei no meu último post, vou colocar aqui o passo a passo que utilizei para usar o Jekyll no blog. No meu caso eu usei um tema específico para blogs que é o [Chirpy](https://chirpy.cotes.info/){:target="_blank"}, que possui uma interface mais voltada para texto e esse tema (assim como a maioria deles) já possui um tutorial próprio para configuração.
{: style="text-align: justify;"}

Então vamos lá!

## **Instalando o Jekyll**

Para instalar o Jekyll é só seguir as instruções [aqui](https://jekyllrb.com/docs/installation/){:target="_blank"} que dependem do seu sistema operacional. O Jekyll é feito em cima da linguagem [Ruby](https://www.ruby-lang.org/pt/){:target="_blank"}, que precisa estar instalada no sistema operacional (inclusive é ensinado a instalá-la no site do Jekyll) e a minha dica aqui para usuários de Mac é sempre instalar o Ruby de forma local, para que a versão de Ruby utilizada pelo sistema operacional não seja sobrescrita e cause problemas. Isso pode ser facilmente alcançado com o [Homebrew](https://brew.sh/index_pt-br){:target="_blank"}, que é mostrado também no site.
{: style="text-align: justify;"}

## **Escolhendo o tema**

Uma vez instalado o Jekyll você já está pronto para gerar seu site estático e isso pode ser feito do zero ou então utilizando um tema. Para facilitar as coisas eu preferi usar um tema pronto ([aqui](https://jekyllrb.com/resources/){:target="_blank"} existem links para coleções deles) e o escolhido foi o Chirpy.
{: style="text-align: justify;"}

Este tema é bem interessante e possui vários recursos avançados para distribuição de posts em categorias, tags, já possui integração e otimização para o [Google Analytics](https://pt.wikipedia.org/wiki/Google_Analytics){:target="_blank"}, [Disqus](https://disqus.com/){:target="_blank"} e várias outras coisas de forma automática.
{: style="text-align: justify;"}

## **Obtendo o tema**

Pelo que eu pude perceber todos os temas que achei tem seu código hospedado no GitHub, então para obter um basta pegar de lá, seguindo o tutorial que geralmente o mantenedor disponbiliza no README.md do repositório.
{: style="text-align: justify;"}

O tema que utilizei está [aqui](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"}. Eu acabei fazendo um pouco diferente do que está no tutorial deste tema pois ele indica que você deve fazer um 'fork' do repositório, coisa que eu não fiz pois preferi baixar o conteúdo direto na minha máquina e utilizar o repositório no GitHub que eu já tinha `murilo-ramos.github.io`, mas os próximos passos são iguais.
{: style="text-align: justify;"}

Mas se for sua primeira vez fazendo algo do tipo eu sugiro seguir os passos do tutorial, que vou seguir daqui pra frente, então vamos fazer um fork do repositório `https://github.com/cotes2020/jekyll-theme-chirpy` e ter o repositório `murilo-ramos/jekyll-theme-chirpy`.
{: style="text-align: justify;"}

## **Inicializando o tema**

Daqui pra frente os passos serão bem específicos para o tema que escolhi e por isso algumas premissas são necessárias:
{: style="text-align: justify;"}

- O Git deve estar instalado;
- Para usuários de Debian ou MacOS, o CoreUtils deve estar instalado (o tutorial do tema ensina a instalar);
- Para usuários de Windows aconselho seguir o tutorial usando o terminal do Git pois vários comandos bash são utilizados;

Este tema possui uma série de processos extras que fazem a categorização, conjunto de tags, atualização de posts e etc, por isso ele precisa dessas coisas a mais.
{: style="text-align: justify;"}

Para baixarmos o tema basta um git clone do nosso repositório:

    $ git clone https://github.com/murilo-ramos/jekyll-theme-chirpy.git

    $ cd jekyll-theme-chirpy/
    $ ls -ltr
    total 112
    -rw-r--r--   1 murilo  staff   620 Sep 16 21:42 404.html
    -rw-r--r--   1 murilo  staff   233 Sep 16 21:42 Gemfile
    -rw-r--r--   1 murilo  staff  1078 Sep 16 21:42 LICENSE
    -rw-r--r--   1 murilo  staff  8929 Sep 16 21:42 README.md
    -rw-r--r--   1 murilo  staff  5241 Sep 16 21:42 _config.yml
    drwxr-xr-x   8 murilo  staff   256 Sep 16 21:42 _data
    drwxr-xr-x  25 murilo  staff   800 Sep 16 21:42 _includes
    drwxr-xr-x   9 murilo  staff   288 Sep 16 21:42 _layouts
    drwxr-xr-x   6 murilo  staff   192 Sep 16 21:42 _posts
    drwxr-xr-x   3 murilo  staff    96 Sep 16 21:42 _scripts
    -rw-r--r--   1 murilo  staff   267 Sep 16 21:42 app.js
    drwxr-xr-x   5 murilo  staff   160 Sep 16 21:42 assets
    drwxr-xr-x   3 murilo  staff    96 Sep 16 21:42 docs
    -rw-r--r--   1 murilo  staff  2064 Sep 16 21:42 feed.xml
    -rw-r--r--   1 murilo  staff   166 Sep 16 21:42 index.html
    -rw-r--r--   1 murilo  staff   257 Sep 16 21:42 robots.txt
    -rw-r--r--   1 murilo  staff  2316 Sep 16 21:42 sitemap.xml
    -rw-r--r--   1 murilo  staff  1400 Sep 16 21:42 sw.js
    drwxr-xr-x   6 murilo  staff   192 Sep 16 21:42 tabs
    drwxr-xr-x   9 murilo  staff   288 Sep 16 21:42 tools

Seguindo o tutorial do tema, temos que terminar de instalar as dependências rodando o seguinte comando:
{: style="text-align: justify;"}

    $ bundle install

E como meu computador é um Mac, precisei instalar o `CoreUtils`

    $ brew install coreutils

E assim estamos prontos para começar.

Nesse momento já é possível rodar o comando abaixo, que irá subir o serviço em `http://localhost:4000` permitindo ver o site funcionando.
{: style="text-align: justify;"}

    $ bash tools/run.sh

![site-working](/assets/img/posts/usando-jekyll-e-github-pages/site-working.png)

É preciso lembrar que ao rodar o comando no terminal ele ficará travado no serviço, então para sair aperte `CTRL+C`.
{: style="text-align: justify;"}

Como você deve ter percebido, o tema vem com vários arquivos de exemplos e precisamos limpá-los para iniciar nossa configuração.
{: style="text-align: justify;"}

Para fazer isso basta rodar o comando abaixo:

    $ bash tools/init.sh
    [INFO] Initialization successful!

Este comando irá remover os arquivos de exemplos e deixará o repositório limpo para começar as configurações, e de quebra ainda irá realizar um commit de inicialização para você :)
{: style="text-align: justify;"}

Neste comando existe uma opção para que sejam excluídas as configurações de GitHub, porém como a idéia e utilizarmos o GitHub Pages não vou falar sobre ela aqui.
{: style="text-align: justify;"}

## **Configurando e personalizando o tema**

As configurações principais ficam no arquivo `_config.yml` e vou colocar aqui as que achei mais importantes.

    title: Chirpy                          # Título do site que será exibido na barra lateral abaixo do avatar

    tagline: A text-focused Jekyll theme.  # O texto que será exibido abaixo do título

    description: >-                        # Descrição do site usada em SEO e feed
    A minimal, portfolio, sidebar,
    bootstrap Jekyll theme with responsive web design
    and focuses on text presentation.

    # Coloque o seu site ex: 'https://username.github.io'
    url: 'protocol://domain'

    author: your_full_name                  # Coloque seu nome

    avatar: /assets/img/sample/avatar.jpg   # Coloque seu avatar, lembrando que é preciso colocar a imagem em /assets/img/caminho. Recomenda-se um avatar de aproximadament 400x400

    github:
    username: github_username             # Coloque seu usuário do GitHub

    twitter:
    username: twitter_username            # Coloque seu usuário do Twitter

    social:
    name: your_full_name                  # Coloque seu nome (esta informação irá aparecer no copyright do rodapé)
    email: example@doamin.com             # Coloque seu e-mail
    links:
        # O primeiro elemento da lista irá ser usado como link para autor no copyright
        - https://twitter.com/username      # Coloque o link do seu Twitter
        - https://github.com/username       # Coloque o link do seu GitHub
        # Descomente e altere os links abaixos se achar necessário
        # - https://www.facebook.com/username
        # - https://www.linkedin.com/in/username

    # Altere para seu timezone ex: America/Sao_Paulo › http://www.timezoneconverter.com/cgi-bin/findzone/findzone
    timezone: Asia/Shanghai

    # Escolhe a configuração de cores utilizando as opções:
    #
    #     light  - Usar tema claro
    #
    #     dark   - Usar tema escuro
    #
    #     dual   - Usa o padrão do sistema e habilita um botão na barra latera para escolher a configuração de cores
    #
    theme_mode: dual


É possível alterar também as configurações de contato que aparecem na barra lateral esquerda no arquivo `_data/contact.yml`. No meu caso eu removi o RSS e coloquei o LinkedIn.
{: style="text-align: justify;"}

    -
    type: github
    icon: 'fab fa-github-alt'
    -
    type: twitter
    icon: 'fab fa-twitter'
    -
    type: email
    icon: 'fas fa-envelope'
    noblank: true            # open link in current tab
    -
    type: linkedin
    icon: 'fab fa-linkedin'   # icons powered by <https://fontawesome.com/>
    url:  'https://www.linkedin.com/in/seu perfil'  # Fill with your Linkedin homepage

E dentro de `_data/share.yml` alterei também as configurações de compartilhamento de um post, adicionando o LinkedIn.
{: style="text-align: justify;"}

    label: "Share"

    platforms:
    -
        type: Twitter
        icon: "fab fa-twitter"
        link: "https://twitter.com/intent/tweet?text=TITLE&url=URL"
    -
        type: Facebook
        icon: "fab fa-facebook-square"
        link: "https://www.facebook.com/sharer/sharer.php?title=TITLE&u=URL"
    -
        type: Telegram
        icon: "fab fa-telegram"
        link: "https://telegram.me/share?text=TITLE&url=URL"
    -
        type: Linkedin
        icon: "fab fa-linkedin"
        link: "https://www.linkedin.com/sharing/share-offsite/?url=URL"

Podemos alterar também o favicon do site de acordo com o tutorial, que basicamente consiste em usar o seu avatar para gerar as imagens no pelo site [Favicon Generator](https://www.favicon-generator.org/){:target="_blank"}.
{: style="text-align: justify;"}

Após a geração, baixe o arquivo gerado e descompacte em sua máquina e exclua os arquivos `browserconfig.xml` e `manifest.json` que vieram junto com o arquivo. Por fim copie todas as imagens para o local `assets/img/favicons` do seu repositório.
{: style="text-align: justify;"}

![favicon](/assets/img/posts/usando-jekyll-e-github-pages/favicon.png)

E para finalizar a personalização eu também coloquei o site todo em português, porém infelizmente não existe uma forma fácil de fazer isso então precisei fazer buscas nos arquivos e encontrar e alterar cada label do site para mudar de inglês para português.
{: style="text-align: justify;"}

## **Alterando o About (Sobre)**

Você deve ter percebido que existe uma seção 'About' no menu da esquerda, e essa seção é uma página para falar um pouco sobre você.
{: style="text-align: justify;"}

Para colocar um texto lá é só adicionar no arquivo `tabs/about.md`. O texto deve ser feito em `Markdown`.
{: style="text-align: justify;"}

![about](/assets/img/posts/usando-jekyll-e-github-pages/about.png)

## **Criando um post**

Agora com todas as configurações prontas podemos rodar o comando `bash tools/run.sh` para ver como o site ficou.
{: style="text-align: justify;"}

Mas você deve ter notado que ainda não temos nenhum post no site, então vamos fazer um de exemplo.
{: style="text-align: justify;"}

Para se fazer um post é necessário colocar um arquivo em `_posts` tendo o nome como `data-nome-do-post.md`.
{: style="text-align: justify;"}

Exemplo: `2020-09-20-meu-primeiro-post.md`

A data precisa sempre seguir o padrão `YYYY-MM-DD` para que o Jekyll consiga ordenar corretamente os posts, e a segunda parte do nome do arquivo irá definir a url do post. Neste exemplo a url seria `http://xxxx/posts/meu-primeiro-post`.
{: style="text-align: justify;"}

O conteúdo deve seguir uma estrutura com cabeçalho e corpo como no exemplo abaixo.
{: style="text-align: justify;"}
```markdown
---
title: Meu primeiro post
author: Murilo Costa
date: 2020-09-16 20:55:00 -0300
categories: [Blogging]
tags: [blog]
---

Este é meu primeiro post!
```

Alguns pontos sobre o cabeçalho:

`title`: É o nome que irá aparecer no topo do post

`author`: O nome do autor do post (geralmente você mesmo rs)

`date`: Data do post, adicionando uma data específica no padrão YYYY-MM-DD HH:MM:SS GMT

`categories`: As categorias em que o post se encaixa, onde essas categorias serão agrupadas com outros posts

`tags`: As tags que identificam o post, e que também serão agrupadas com outros posts

O conteúdo precisa ser feito em markdown e neste tema é usado o kramdown que possui vários artifícios de estilização de texto, veja na [documentação oficial](https://kramdown.gettalong.org/syntax.html){:target="_blank"}.
{: style="text-align: justify;"}

Depois de salvar o arquivo é só rodar `bash tools/run.sh` novamente para já visualizar o post na tela principal, e verificar também que as páginas de categorias, tags e arquivo foram populadas.
{: style="text-align: justify;"}

**Dica:** É possível rodar o comando `bash tools/run.sh` somente uma vez e então ir fazendo as modificações necessárias que o jekyll irá atualizar o site automaticamente. Se isto não acontecer verifique no tutorial oficial utilização do `fswatch`, que funcionou pra mim.
{: style="text-align: justify;"}

## **Subindo no GitHub Pages**

Agora que o site está configurado e com conteúdo, já podemos colocá-lo no GitHub Pages e visualizá-lo na internet como `https://seulogin.giothub.io`.
{: style="text-align: justify;"}

Antes de prosseguir, vamos commitar nossas mudanças e colocar no nosso repositório.
{: style="text-align: justify;"}

    $ git add .
    $ git commit "Finalizando configuração e escrevendo o primeiro post"
    $ git push

Para colocar no GitHub Pages será necessário primeiro uma modificação inconveniente porém necessária, que é trocar o nome do seu repositório (fork) para `seulogin.github.io`, pois só assim o GitHub consegue publicar o seu site neste endereço.
{: style="text-align: justify;"}

![seulogin-github-io](/assets/img/posts/usando-jekyll-e-github-pages/seulogin-github-io.png)

E o inconveniente aqui é que o endereço do seu repositório foi alterado, obrigando você a trocar o `remote` (endereço no GitHub) no seu git. Se você não quer perder tempo com isso sugiro simplesmente fazer novamente um clone do seu repositório, que já terá o remote correto.
{: style="text-align: justify;"}

Após essa configuração, se estivessemos usando algum outro tema de Jekyll mais simples o site já estaria no ar e nenhuma configuração adicional seria necessária, porém como o tema que estamos utilizando é um pouco mais completo, é necessária mais uma configuração no GitHub. Procure nas configurações do seu repositório na seção 'GitHub Pages' e selecione o branch 'gh-pages' como Origem/Source, pois esse é um branch digamos que 'compilado' pelo tema e já pronto para visualização. Esse branch é gerado toda vez que um commit/push acontece no repositório, iniciado através do mecanismo de [GitHub Actions](https://github.com/features/actions){:target="_blank"}.
{: style="text-align: justify;"}

![gh-pages](/assets/img/posts/usando-jekyll-e-github-pages/gh-pages.png)

E agora estamos prontos, é só acessar o seu site em `http://seulogin.github.io`.
{: style="text-align: justify;"}

![site-final](/assets/img/posts/usando-jekyll-e-github-pages/site-final.png)

Com o site publicado você pode também fazer configurações adicionais, como habilitar o https (o GitHub gera um certificado automaticamente) e até mesmo usar um domínio customizado.

É isso aí pessoal, se tiverem alguma dúvida é só deixar um comentário.
{: style="text-align: justify;"}



