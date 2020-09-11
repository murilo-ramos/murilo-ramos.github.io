---
title: Jekyll, GitHub Pages e blog de cara nova
author: Murilo Costa
date: 2020-09-11 17:00:00 -0300
categories: [Blogging]
tags: [murilo, murilo-tech, jekyll, github, githubpages, markdown]
---

Estes dias eu estava olhando esse blog e percebi que fiquei mais de um ano sem postar nada por aqui, só pagando o domínio de bobeira rsrs, por isso resolvi dar um reavivada nesse blog dando uma repaginada, pois a plataforma que estava usando antes (blogger) não me deixou ter o design bacana que eu queria, e que agora consegui.
{: style="text-align: justify;"}

E como consegui isso? A resposta é `Jekyll` + `GitHub Pages`: A forma mais fácil de se ter um site estático na internet hoje.
{: style="text-align: justify;"}

Mais o que é o Jekyll, GitHub Pages e um site estático?
{: style="text-align: justify;"}

Vamos explicar, e na ordem rs.
{: style="text-align: justify;"}

## **Site estático**

Um site estático é um site que não possui nada rodando do lado servidor, sendo uma aplicação simples com somente `HTML5`, `Javascript` e `CSS`. Por não possuir uma camada rodando no servidor, este tipo de site não possui dependências mais complexas, como acesso a banco de dados, serviços internos e etc, obtendo maior segurança e simplicidade para disponibilização na web. É claro que este tipo de aplicação não pode ser usado em todos os casos, por isso se restringe mais a landing pages, páginas institucionais, portfólios pessoais e blogs, que é o meu caso.
{: style="text-align: justify;"}

E a maior facilidade de um site estático é que ele é compatível com praticamente todos os ambientes de host disponíveis na internet, uma vez que sua aplicação é um FrontEnd puro.
{: style="text-align: justify;"}

O desenvolvimento de sites estáticos pode ser feito geralmente usando html puro mesmo, mas em muitos casos é utilizado o `markdown` que também faz parte do ecossistema de geradores de sites estáticos, como o Jekyll.
{: style="text-align: justify;"}

## **Jekyll**

O Jekyll é um gerador feito em Ruby que facilita muito a criação desse tipo de site, provendo uma forma fácil de desenvolver seu site, inclusive com várias opções de temas grátis que podem ser encontrados na sua [página oficial](https://jekyllrb.com/){:target="_blank"}.
{: style="text-align: justify;"}

Basicamente o que a ferramenta faz é te dar uma estrutura semi-pronta para criar o seu site de acordo com que você está buscando, para não precisar sair do zero. No meu caso por exemplo como este site é um blog, eu utilizei um tema de blog e simplesmente customizei a meu gosto, onde agora para manter o site só preciso criar os posts dentro da estrutura existente.
{: style="text-align: justify;"}

O Jekyll não é o único nesse ramo, existindo outras boas alternativas como o [Gatsby.js](https://www.gatsbyjs.com/){:target="_blank"} e o [HUGO](https://gohugo.io/){:target="_blank"}.
{: style="text-align: justify;"}

Uma coisa que me motivou a usar o Jekyll também foi a fácil integração que ele tem com o GitHub Pages.
{: style="text-align: justify;"}

## **GitHub Pages**

Não preciso dizer que o [GitHub](https://github.com/){:target="_blank"} é o novo ponto e encontro da galera de tecnologia pois isso todo mundo já sabe, sendo atualmente o serviço de repositório de códigos-fonte mais utilizado no planeta. Mas algo que talvez alguns não percebem é que o GitHub tem um preocupação a mais com os desenvolvedores de software, que é encontrar uma forma de que eles possam mostrar seu trabalho utilizando a plataforma e criando uma identificação profissional (ou pessoal) ali dentro, e daí apareceu o [GitHub Pages](https://pages.github.com/){:target="_blank"}.
{: style="text-align: justify;"}

Esse serviço permite que você crie um site estático a partir do seu perfil que ficará disponível na internet, bastando somente criar um repositório chamado `seulogin.github.io` que automaticamente o GitHub disponibiliza a página `https://seulogin.github.io` na web com o conteúdo do seu repositório, permitindo que você tenha esse espaço provido por eles sem custo algum, e ainda pode utilizar um domínio customizado, que foi o que fiz usando o [murilo.tech](http://murilo.tech).
{: style="text-align: justify;"}

Assim, para ter meu blog atualizado acabei utilizando o Jekyll com deploy no GitHub Pages para ter como resultado essa página que você está lendo :)
{: style="text-align: justify;"}

Neste exato momento já estou fazendo outro post para explicar como configurei o meu blog usando essas ferramentas, então até a próxima!
{: style="text-align: justify;"}