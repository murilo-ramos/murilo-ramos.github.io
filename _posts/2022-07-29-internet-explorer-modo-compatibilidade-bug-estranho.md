---
title: Internet Explorer, modo de compatibilidade e um bug estranho
author: Murilo Costa
date: 2022-07-29 08:00:00 -0300
categories: [Blogging, Frontend, Dicas]
tags: [historia, bug, internet-explorer, ie, modo-compatibilidade, frontend, html]
---

Talvez tenha passado despercebido para muitos, mas o mês passado foi uma data de alívio para desenvolvedores e usuários da internet, pois finalmente a Microsoft [aposentou seu navegador mais famoso](https://www.cnnbrasil.com.br/business/apos-27-anos-em-atividade-microsoft-aposenta-navegador-internet-explorer/){:target="_blank"}: O Internet Explorer.
{: style="text-align: justify;"}

Infelizmente essa fama nunca foi a das melhores. O Internet Explorer sempre foi um browser problemático, com vários problemas de performance, segurança e compatibilidade que causavam enormes dores de cabeça aos desenvolvedores que ainda precisavam suportá-lo em suas aplicações, o que era incrivelmente comum dado a quantidade de pessoas que ainda insistiam em usá-lo.
{: style="text-align: justify;"}

E aproveitando que estamos próximos a esse marco da internet, vou falar sobre uma situação um tanto quanto bizarra que eu tive com o Internet Explorer e qual é a solução para esse problema, que levei algum tempo para entender o que estava acontecendo.
{: style="text-align: justify;"}

![Internet Explorer](/assets/img/posts/internet-explorer-modo-compatibilidade-bug-estranho/internet-explorer.png)

## O problema

Atualmente eu tenho trabalhado desenvolvendo mais no back-end de aplicações, porém em grande parte do meu histórico profissional acabei atuando também com desenvolvimento web full-stack, que é quando desenvolvemos tanto o código que irá rodar no servidor quanto o código que irá construir e manipular a tela do usuário, conhecido também como front-end.
{: style="text-align: justify;"}

Nesse tipo de desenvolvimento muitas vezes precisamos desenvolver uma feature de forma completa, onde desenvolvemos a tela que será utilizada pelo usuário, a integração entre tela e back-end e o processo que irá ser executado no servidor, e isso é algo que já tive a oportunidade de executar várias vezes.
{: style="text-align: justify;"}

Em uma dessas oportunidades fui direcionado para realizar uma atualização em uma feature, onde foi necessário adicionar um campo em um formulário, mudar o layout e mecanismo da tela desse formulário e então adequar o processamento para contemplar esse novo campo, algo que é relativamente simples de ser feito, e consegui executar com certa facilidade.
{: style="text-align: justify;"}

Por ser uma aplicação web, durante o desenvolvimento eu tenho o costume de ir testando sempre no navegador Google Chrome, pois já tenho uma certa familiaridade com ele, principalmente com o Dev Tools que ajuda muito a debugar a tela, e com isso eu conseguia a garantia que o software funcionava corretamente no Chrome.
{: style="text-align: justify;"}

Porém, ao final do desenvolvimento era sempre necessário testar a aplicação também no Mozilla Firefox e no Internet Explorer, pois eram os navegadores que a empresa dava suporte conforme necessidade dos clientes, então, para garantir o funcionamento, sempre testávamos nos três navegadores: Chrome, Firefox e Internet Explorer.
{: style="text-align: justify;"}

Seguindo então essa linha, eu realizei os testes em localhost nos três navegadores e os testes foram OK, onde então subi para o ambiente de homologação/staging, realizei mais alguns testes, e estava tudo ok, onde pude finalmente subir a alteração em produção.
{: style="text-align: justify;"}

<center><iframe src="https://giphy.com/embed/111ebonMs90YLu" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/thumbs-up-111ebonMs90YLu">via GIPHY</a></p></center>

Seguindo as boas práticas de deploy eu subi a alteração em produção durante a manhã, pois se caso ocorresse algum problema eu poderia corrigir durante o resto do dia, e para minha surpresa algumas horas depois do deploy eu recebo um chamado dizendo que a aplicação estava quebrando.
{: style="text-align: justify;"}

No ticket do chamado estava um print screen da tela que alterei **inteiramente quebrada**, de uma forma que parecia um erro de abre/fecha tags ou código javascript incompleto, algo que era impossível de ter acontecido, pois, mesmo se algum problema de merge de código tivesse ocorrido, eu notaria nos testes em ambientes de homologação ou staging.
{: style="text-align: justify;"}

Para desencargo de consciência e ter a certeza de que não deixei passar nada nos testes, decidi testar novamente em localhost e no ambiente de staging, tomando novamente o cuidado de limpar o cache dos navegadores, e os testes foram ok, onde infelizmente não consegui reproduzir o problema.
{: style="text-align: justify;"}

Nesse meio tempo apareceram mais alguns tickets de suporte informando o mesmo erro, o que já acendeu o alerta de que o problema não era só com um usuário específico, então existia algo sério realmente ocorrendo.
{: style="text-align: justify;"}

Analisando melhor os tickets e print screens dos erros pude notar um padrão: as imagens mostravam o software sendo usado somente no navegador Internet Explorer, e o problema ocorria somente para usuários internos.
{: style="text-align: justify;"}

Esse era um software de nível nacional, acessado pelos clientes a partir da internet, porém a própria empresa que desenvolvia o software também o utilizava, então por isso existiam os chamados usuários internos. Assim, pedi mais informações a esses usuários, como qual a versão do Internet Explorer sendo usada e se, caso esses usuários utilizassem outro navegador, o problema continuava, e como resposta obtive que o problema realmente só acontecia no Internet Explorer, e a versão que estavam utilizando era a mesma que eu: Internet Explorer 11.
{: style="text-align: justify;"}

Essa situação me deixou bem intrigado, pois a versão do Internet Explorer usada era a mesma e de forma alguma eu conseguia reproduzir o erro.
{: style="text-align: justify;"}

Foi então que, esfriando um pouco mais a cabeça, comecei a olhar mais detalhadamente para os print screens e vi algo que chamou minha atenção: A URL que esses usuários estavam acessando estava diferente!
{: style="text-align: justify;"}

Como mencionei anteriormente, por esse software ser acessado de forma interna ele possuía dois tipos de acesso, um pela internet com uma URL pública `https://www.meusistema.com.br`, e outra URL de acesso interno pela intranet, que era `https://meusistema.intranet`. Eu já sabia dessa diferença, e na prática não havia diferença, pois as duas URLs apontavam para exatamente o mesmo servidor, como pode ser visto na imagem a seguir.
{: style="text-align: justify;"}

![Diagrama de acesso](/assets/img/posts/internet-explorer-modo-compatibilidade-bug-estranho/diagrama-acesso-aplicacao.png)

Então a aplicação era a mesma, o código era o mesmo, e a infraestrutura era a mesma, nada podia explicar esse problema, mesmo assim resolvi então realizar o teste com essa URL interna, e para a minha surpresa **finalmente consegui reproduzir o erro**.
{: style="text-align: justify;"}

## Internet Explorer: O browser quântico

Por quê? Essa era a única coisa que pairava na minha cabeça: Por que quando acesso a URL pública o software funciona normalmente, e ao acessar a URL interna ele quebra? Isso não fazia o menor sentido para mim, tentei limpar cache, atualizar o navegador, limpar dados e nada.
{: style="text-align: justify;"}

Foi aí que lembrei que o Internet Explorer na versão 11 também possui uma ferramenta de desenvolvedor, permitindo visualizar o código fonte da página acessada, javascript e etc, e ao abrir me veio a segunda surpresa.
{: style="text-align: justify;"}

![Modo de compatibilidade](/assets/img/posts/internet-explorer-modo-compatibilidade-bug-estranho/modo-compatibilidade-ie.png)

Por questões de compatibilidade com sites e aplicações antigas, o Internet Explorer permite que alguns sites sejam acessados utilizando o modo de compatibilidade, que interpreta o código com versões antigas de Html, CSS e Javascript, e por incrível que pareça isso estava acontecendo para a aplicação em questão, o navegador estava usando o modo de compatibilidade interpretando as páginas como Internet Explorer `7`, que é uma versão extremamente antiga, e por isso as páginas quebravam.
{: style="text-align: justify;"}

**Finalmente eu tinha encontrado o problema!**
{: style="text-align: justify;"}

Mas agora faltava entender o porquê de isso só acontecer na URL interna.
{: style="text-align: justify;"}

## Modo de compatibilidade e guerra dos navegadores

Depois de muitas buscas e consultas a documentações eu consegui entender a raiz do problema, e essa raiz é tão profunda que nos faz voltar a famosa [guerra dos navegadores](https://www.tecmundo.com.br/mercado/132493-historia-guerra-navegadores-video.htm){:target="_blank"}, que foi um evento ocorrido quando a internet como é hoje começou a se popularizar.
{: style="text-align: justify;"}

Para apresentar um pouco de contexto, basicamente a internet/web começou com somente um navegador famoso, o falecido [Netscape Navigator](https://pt.wikipedia.org/wiki/Netscape_Navigator){:target="_blank"}, e posteriormente apareceu o Internet Explorer, onde em sua versão 6.0 ele se consolidou como o navegador de sua época, entre os anos 2000 e 2002, e o motivo dessa consolidação é que o IE apresentou uma série de melhorias e recursos novos que não existiam em seus poucos concorrentes, permitindo que os desenvolvedores criassem aplicações muito mais dinâmicas, porém isso teve um custo, que foi a falta de padronização, fazendo com que as aplicações desenvolvidas para o IE funcionassem somente nesse navegador, e não nos demais.
{: style="text-align: justify;"}

Com a popularização da internet, começaram a surgir então outros navegadores como o Opera e principalmente o Mozilla Firefox, que passaram a lutar por uma fatia de usuários e desbancando o IE, principalmente no final da guerra com o surgimento do Google Chrome. E nesse final acabamos conseguindo finalmente ter uma padronização da web, onde os navegadores passaram a ter suas implementações respeitando os padrões da [W3C](https://www.w3c.br/Padroes/){:target="_blank"} e permitindo que os sites funcionassem normalmente independentemente da plataforma, e é claro que o Internet Explorer precisou também se adequar a esses padrões, o que ocorreu mais ou menos a partir de sua versão 8.0.
{: style="text-align: justify;"}

![Gráfico de utilização de navegadores](/assets/img/posts/internet-explorer-modo-compatibilidade-bug-estranho/utilizacao-navegadores.png)
*Fonte [Wikipedia](https://upload.wikimedia.org/wikipedia/commons/7/70/BrowserUsageShare.png){:target="_blank"}.*

E o que isso tudo tem a ver com o nosso problema?
{: style="text-align: justify;"}

O grande ponto aqui é que essas mudanças aconteceram de forma muito rápida, e muitas aplicações já existentes na internet foram desenvolvidas seguindo o modo de funcionamento do Internet Explorer 6 e 7, que foi radicalmente afetado a partir da padronização seguida na versão 8.0. Então, para não afetar as aplicações existentes na época e oferecer um tempo para que as empresas se adequassem, a Microsoft passou a embutir o chamado **Modo de compatibilidade** nas versões mais novas de seu navegador, que ao ser ativado interpretava o site em questão como se estivesse na versão 7 do navegador, permitindo que as aplicações mais antigas continuassem funcionando.
{: style="text-align: justify;"}

**E é exatamente esse modo de compatibilidade que era ativado ao acessar a aplicação pela URL interna**, onde o Internet Explorer passava a interpretar as páginas como se estivesse em sua versão 7.0, e então quebrava.
{: style="text-align: justify;"}

Porém, como pode ser visto anteriormente, em nenhum momento eu ativei esse modo de compatibilidade, então por que mesmo assim ele entrou em funcionamento? Foi aí que finalmente encontrei a informação precisa em um artigo (como [esse](https://www.leapinggorilla.com/Blog/Read/1016/ie-ate-my-css---disabling-compatability-mode){:target="_blank"}) explicando a situação.
{: style="text-align: justify;"}

<center><iframe src="https://giphy.com/embed/WRQBXSCnEFJIuxktnw" width="480" height="307" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/math-lady-meme-WRQBXSCnEFJIuxktnw">via GIPHY</a></p></center>

**O ponto de convergência aqui está no fato de a Microsoft ter deliberadamente incluído nas versões mais recentes do navegador uma feature que ativa automaticamente o modo de compatibilidade para sites quando são acessados a partir de uma intranet!**
{: style="text-align: justify;"}

De acordo com minhas pesquisas, isso foi feito devido ao fato de que, quando a Microsoft padronizou seu navegador, a maioria das aplicações internas das grandes empresas já haviam sido desenvolvidas para as versões anteriores do IE, e forçar essas empresas a terem que ativar o modo de compatibilidade ao acessar esses sites na intranet seria um problema, então, para facilitar, a Microsoft resolveu colocar essa feature, onde todo site carregado a partir de uma intranet no Internet Explorer ativa o modo de compatibilidade automaticamente, e essa feature perdurou até sua última versão, que foi a 11.
{: style="text-align: justify;"}

E foi exatamente isso quebrou o software que eu desenvolvia.
{: style="text-align: justify;"}

## Solução

Existem basicamente duas soluções para esse problema.
{: style="text-align: justify;"}

### 1ª solução: Use o Edge

Com o fim do suporte ao Internet Explorer pela Microsoft, a melhor solução é usar seu novo navegador, o [Microsoft Edge](https://www.microsoft.com/pt-br/edge){:target="_blank"}, ou qualquer outro navegador mais recente que não irá apresentar esse comportamento de modo de compatibilidade, garantindo que seu site/aplicação sempre será executado conforme os padrões mais modernos da web.
{: style="text-align: justify;"}

### 2ª solução: Force a versão mais nova do IE

Mas, como nem tudo são flores e muitas vezes precisamos continuar com suporte a coisas antigas, se ainda assim existir a necessidade de continuar dando suporte ao Internet Explorer, existe uma opção de header que pode ser colocada nas páginas Html que irá forçar o IE a interpretar sua página sempre utilizando a versão mais recente, que é a `X-UA-Compatible`.
{: style="text-align: justify;"}

```html
<!DOCTYPE html>
  <html>
    <head>
      <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
      <meta charset="utf-8">
      ....
```

Repare no trecho de código anterior que existe a tag `meta` com o `X-UA-Compatible` setado para `IE=Edge`, que de acordo com a [documentação da Microsoft](https://docs.microsoft.com/en-us/openspecs/ie_standards/ms-iedoco/380e2488-f5eb-4457-a07a-0cb1b6e4b4b5){:target="_blank"} irá fazer com que o IE sempre interprete essa página utilizando os padrões mais recentes disponíveis, não importa se é internet ou intranet.
{: style="text-align: justify;"}

E é isso! Problema resolvido.
{: style="text-align: justify;"}

## Uma leve reflexão

Acredito que tenha ficado nítido o fato de que toda essa situação ocorreu pela decisão da Microsoft no passado de fazer com que seu navegador oferecesse mais recursos aos desenvolvedores e usuários indo pelo caminho mais fácil, que foi a falta de padronização, e vemos que isso acabou custando caro ao produto ao longo do tempo.
{: style="text-align: justify;"}

Aí fica a dúvida se realmente esse acabou sendo o melhor caminho a ser tomado na época, pois com certeza foi muito bom para o produto durante um tempo, onde ele se tornou referência de mercado, mas isso fez também com que ele acabasse se isolando ao passo que outros navegadores foram se padronizando e modernizando, ao ponto de muitas empresas passarem a desenvolver seus sites e aplicações seguindo esses padrões sem se preocupar se iria funcionar no IE ou não, onde ele perdeu uma enorme fatia de mercado.
{: style="text-align: justify;"}

Então será que não seguir padrões tendo em troca a rapidez e flexibilidade é uma boa jogada? Me parece que não...
{: style="text-align: justify;"}

Inclusive, existe um [outro navegador que vem tomando decisões](https://www.theverge.com/2018/1/4/16805216/google-chrome-only-sites-internet-explorer-6-web-standards){:target="_blank"} bem parecidas com o Internet Explorer durante os últimos anos.
{: style="text-align: justify;"}

Será que isso está correto?
{: style="text-align: justify;"}

Só o tempo irá dizer :).
{: style="text-align: justify;"}

Fica aqui essa reflexão, principalmente para nós que desenvolvemos software e temos que lidar com questões como essa todos os dias.
{: style="text-align: justify;"}



