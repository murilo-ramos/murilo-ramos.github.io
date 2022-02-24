---
title: Causo - O dia que apaguei o banco de dados de produção
author: Murilo Costa
date: 2022-02-24 12:00:00 -0300
categories: [Blogging, Causo]
tags: [causo, historia, relato, java, producao, banco-de-dados, desktop]
---

Sei que o título desse post está bem sensacionalista, mas para saber se o fato realmente aconteceu será preciso ler todo o post. Será mesmo que eu apaguei o banco de dados de produção? 
{: style="text-align: justify;"}

Esse causo aconteceu quando ingressei como novato em uma empresa e, por já ter um pouco mais de experiência, decidi que alguns cuidados não precisavam ser levados tão a sério, o que me causou uma situação que poderia ter levado a um infarto pessoas um pouco mais cardíacas que eu rs.
{: style="text-align: justify;"}

Vamos ao causo!
{: style="text-align: justify;"}

## O causo

Depois de conseguir uma boa experiência com desenvolvimento de software e T.I. em geral eu decidi embarcar em uma oportunidade que julguei melhor na época, e então entrei em uma nova empresa para encarar um novo desafio profissional.
{: style="text-align: justify;"}

Nessa empresa o ambiente era mais ou menos parecido com o que eu já conhecia, com equipes que, de forma técnica, desenvolviam o software da empresa e o gerenciavam, incluindo o gerenciamento dos servidores de produção, então uma vez dentro da equipe já tínhamos acesso a todos ambientes incluindo o banco de dados de produção.
{: style="text-align: justify;"}

Eu já vi muitas pessoas dizendo que não é saudável e nem deveria ser permitido aos desenvolvedores acessarem o ambiente produtivo, pois este deveria ser controlado especificamente por uma equipe de operações e isso até faz algum sentido, porém raramente foi realidade nos lugares que trabalhei, o que sempre trouxe muita flexibilidade e facilidade para resoluções de problemas que muitas vezes só acontecem em produção, mas traz também uma enorme responsabilidade de saber que suas ações nesse ambiente podem trazer consequências diretas para o usuário e clientes, entretanto, no geral eu sempre soube lidar bem com isso.
{: style="text-align: justify;"}

Logo na minha primeira semana de trabalho na empresa nova, me pediram para resolver um bug que estava acontecendo em uma das partes do software, que no caso era uma [aplicação desktop](https://xgen.com.br/blog/aplicacao-web-ou-desktop-qual-a-melhor-solucao){:target="_blank"} que baixava informações (incluindo arquivos) de um servidor a partir de uma [API web](https://pt.wikipedia.org/wiki/Interface_de_programa%C3%A7%C3%A3o_de_aplica%C3%A7%C3%B5es){:target="_blank"} backend feita em Java. Ao verificar o problema, rapidamente consegui identificar que o problema estava na API e efetuei a correção no código, partindo então para o teste da correção com o intuito de ver se aquela mudança realmente tinha corrigido o problema.
{: style="text-align: justify;"}

Para realizar os testes infelizmente não existiam nenhum tipo de testes automatizados, como testes unitários ou de integração, e também nesse caso em específico era complicado utilizar ferramentas para chamadas em API como o glorioso [Postman](https://www.postman.com/){:target="_blank"} pois algumas das chamadas não eram no padrão [REST](https://pt.wikipedia.org/wiki/REST){:target="_blank"}, assim fui aconselhado por outros colegas de trabalho a utilizar a própria aplicação desktop (cliente) para realizar o teste, bastando tê-la em minha máquina e apontá-la para a URL da minha própria máquina ([localhost](https://www.hostinger.com.br/tutoriais/o-que-e-localhost){:target="_blank"}).
{: style="text-align: justify;"}

Segui esse conselho e fui efetuar o teste utilizando a aplicação desktop, o que era realmente mais fácil pois esta aplicação já estava presente em minha máquina (a máquina que me foi designada não foi formatada previamente). Porém, ao abrir a aplicação percebi que ela estava com um pouco de 'sujeira', que eram dados antigos presentes ali e que poderiam me atrapalhar nos testes, por isso resolvi que era melhor limpar os dados atuais antes de continuar, para garantir que nenhum dado antigo possa causar uma situação inesperada e que não tenha nenhuma relação com o teste que precisava ser feito.
{: style="text-align: justify;"}

Essa aplicação possuía uma espécie de base de dados local contendo informações utilizadas pelo usuário e ela funcionava off-line, onde os dados eram sincronizados com o servidor justamente a partir do backend que eu precisava testar, e era essa base de dados local que eu precisava apagar para proceder com o meu teste, porém eu não sabia como fazer isso.
{: style="text-align: justify;"}

Conforme mencionei no início desse causo, essa era minha primeira semana na empresa e eu ainda não conhecia muita gente ali, e como eu entrei lá com uma certa experiência de trabalho, achei incômodo pedir ajuda para alguém na tarefa de limpar essa base de dados local da aplicação (olha o meu erro aparecendo rsrs), por isso decidi descobrir sozinho como fazer isso, e logo achei algo que poderia me ajudar.
{: style="text-align: justify;"}

Dentro da pasta de instalação dessa aplicação existiam vários arquivos executáveis que pareciam ser usados para manutenção da aplicação, e um deles se chamava `CleanDatabase.exe` e neste exato momento me pareceu que minha busca tinha terminado, pois com certeza esse era um utilitário que apagaria essa base de dados local, onde logo fui abrindo-o. Este utilitário era bem simples e composto somente de uma tela bem com um botão de `Start` e uma mensagem bem grande escrita em vermelho dizendo que essa operação iria apagar todos os dados do banco de dados, algo que eu já esperava.
{: style="text-align: justify;"}

Apesar de me parecer extremamente claro que essa ferramenta utilitária iria apagar os dados locais da aplicação (e eu tenho certeza de que isso ficou claro também para quem está lendo), antes de clicar no botão `Start` me passou um lampejo de pensamento indagando o seguinte: 
{: style="text-align: justify;"}

*Será que essa base de dados local é realmente um banco de dados, como um [MySQL](https://pt.wikipedia.org/wiki/MySQL){:target="_blank"} por exemplo? Ou será um [SQLite](https://pt.wikipedia.org/wiki/SQLite){:target="_blank"}? Ou será um sistema de arquivos simples mesmo? Se for um banco de dados real, será que existe a chance desse utilitário estar com sua configuração apontada para um banco de dados em outro local que não minha máquina? Como, por exemplo, apontado para o banco de dados de produção?*
{: style="text-align: justify;"}

Apesar de serem questionamentos válidos, esse pensamento logo sumiu da minha cabeça pois isso não fazia sentido algum, e dado o cenário que me era apresentado essa possibilidade não existia, então não havia perigo algum de executar esse utilitário, apesar da minha consciência ter achado melhor consultar alguém no momento (o que infelizmente não fiz), e então acionei o botão de `Start`.
{: style="text-align: justify;"}

E aqui começou a situação que quase me causou um infarto.
{: style="text-align: justify;"}

O software começou a executar a limpeza e, para variar, não tinha nenhuma barra de progresso ou algo do tipo indicando a execução, somente uma mensagem piscando de `Wait`. Apesar de eu não saber ao certo qual era o tamanho dessa base de dados local, eu imaginei que por ser uma base local ela com certeza não seria muito pesada, então essa limpeza duraria somente alguns segundos, quem sabe no máximo um minuto e meio, por isso não me preparei nem um pouco para contar quanto tempo havia decorrido desde que acionei o botão de `Start` (e o utilitário também não exibia o tempo decorrido), por isso fiquei somente aguardando o processo terminar.
{: style="text-align: justify;"}

Mas ele não terminava...
{: style="text-align: justify;"}

Por demorar mais que os segundos que eu esperava, resolvi monitorar o tempo a partir do relógio do computador e partir desse momento vi que a execução já tinha ultrapassado os cinco minutos, o que indicava que a quantidade de dados sendo apagada era grande, e novamente tive aquele lampejo de pensamento: *Será que existe a chance de este utilitário estar apagando dados do banco de dados de produção e por isso estar demorando mais que o normal?* Mas logo espantei esse pensamento da cabeça e continuei no aguardo.
{: style="text-align: justify;"}

Passados mais três minutos comecei a ficar incomodado com essa demora e resolvi encerrar o utilitário, e para a surpresa de zero pessoas o utilitário travou (valeu Windows 7!!!), e resolvi aguardar mais um pouco para ver se ele destravava sozinho.
{: style="text-align: justify;"}

Nessa exato momento dei uma respirada e olhei por cima da minha [baia](https://blog.marelli.com.br/pt/baias-para-escritorio){:target="_blank"} (tempo de trabalho presencial) e vi alguns rostos aflitos e com cara de interrogação, o que achei bem estranho, e comecei a perceber que o colega de trabalho na baia ao lado estava soltando vários 'carambas', e resolvi perguntar o que estava acontecendo, onde ele me respondeu que precisava conectar no banco de dados de produção para ver uma informação e não estava conseguindo, o banco parecia estar offline.
{: style="text-align: justify;"}

É claro que meu corpo gelou por inteiro nesse momento, pois aquele lampejo de pensamento voltou como uma bala.
{: style="text-align: justify;"}

Essa situação poderia ser alguma coisa específica da máquina dele e então resolvi fazer um teste na minha, onde para minha infelicidade o mesmo problema ocorreu, o banco de dados de produção realmente parecia não estar mais respondendo.
{: style="text-align: justify;"}

<center><iframe src="https://giphy.com/embed/3ohhwF34cGDoFFhRfy" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/hulu-scared-3ohhwF34cGDoFFhRfy">via GIPHY</a></p></center>

O pensamento pairou como um elefante na minha cabeça: *Meu Deus!!! Fiz uma merd\* gigante!!!*. 
{: style="text-align: justify;"}

Então olhei para os lados e tive a sensação que todos no escritório estavam com cara de desespero e vi alguns também tentando conexão com o banco de dados de produção, mas sem sucesso, onde eu fiquei mais desesperado ainda, e resolvi fechar o utilitário na força bruta com o famoso [CTRL + ALT + DEL](https://pt.wikipedia.org/wiki/Ctrl%2BAlt%2BDel){:target="_blank"}, mas mesmo assim eu não conseguia fechar, aumentando consideravelmente a palpitação no peito.
{: style="text-align: justify;"}

Nesse momento, apesar de parecer uma situação impossível, eu já tinha quase certeza de que eu havia cometido o maior erro da minha vida, pois realmente eu estava apagando o banco de dados de produção. No meu pensamento provavelmente aquele utilitário era para outros fins que não a limpeza da base de dados local da aplicação, e com certeza alguém no passado deixou ele configurado para apagar o banco de dados de produção, e eu, sem analisar previamente, acabei executando-o.
{: style="text-align: justify;"}

Mas, assim como você deve estar pensando, isso parecia ser uma situação muito absurda, com certeza era somente uma coincidência que estava acontecendo, e por algum motivo não estávamos conseguindo acessar o banco de produção, mas poderia ser somente um problema interno e aplicação estaria funcionando normalmente para os clientes na internet (era uma aplicação de uso nacional).
{: style="text-align: justify;"}

E nesse momento me lembrei que existiam painéis digitais bem atrás da minha estação de trabalho com informações de saúde dos softwares da empresa, incluindo a quantidade de usuários conectados no momento, e se as informações ali estivessem normais isso indicaria que estávamos somente tendo um  problema interno e o banco não estava passando por nenhum problema, e então ao virar e visualizar o painel eu realmente quase tive um infarto, pois a quantidade de usuários ativos estava bem baixa e caindo de dez em dez, o que significava algum problema mesmo no software, mais especificamente usuários desconectando por falta de dados no banco de dados.
{: style="text-align: justify;"}

Agora eu já não sabia mais o que fazer, não conseguia pensar nem raciocinar, só consegui puxar da tomada meu computador na esperança de que a limpeza do banco de dados pudesse rodar em alguma espécie de transação, então se a transação não fosse finalizada talvez os dados não fossem apagados por definitivo.
{: style="text-align: justify;"}

Porém eu sabia que isso não ia adiantar, pois era fato que eu realmente tinha cometido um erro enorme, o maior da minha vida, e agora não tinha mais volta. Fiquei tão desesperado que já comecei a pensar em como faria para arranjar outro emprego, visto que por conta desse erro provavelmente eu seria demitido por justa causa. Fiquei realmente sem chão.
{: style="text-align: justify;"}

Mas como eu sou uma pessoa de princípios, eu não podia ver aquela situação toda ocorrendo e não fazer absolutamente nada, eu precisava avisar alguém sobre o ocorrido e ver quais eram os próximos passos para nos recuperarmos do problema, pois o banco de dados possuía backup diário, e então teríamos que restaurar esse backup e fazer tudo voltar ao normal (quase normal, pois os dados recentes seriam perdidos nesse caso).
{: style="text-align: justify;"}

Decidi comunicar a situação ao meu superior e fiquei um tempo bolando um discurso do que falar, e esse tempo me pareceu uma eternidade, até que resolvi criar coragem para ir até a estação de trabalho dele. Então tomei um grande gole de água para acalmar, me levantei e comecei a sair da baia, onde a cada passo dado parecia que eu estava carregando um peso de trezentos quilos nas pernas.
{: style="text-align: justify;"}

Saindo da minha baia dei uma olhada de leve no meu colega ao lado e vi que ele estava com a ferramenta para gerenciamento de banco de dados aberta, e parecia estar conectado ao banco. Corri até ele e, como quem não quer nada, é claro rs, perguntei se ele tinha conseguido voltar a conectar, e para a minha surpresa a resposta foi sim!!!
{: style="text-align: justify;"}

Sem pensar muito voltei voando para meu computador (que eu já tinha religado) e fui testar a conexão e estava tudo normal, consegui conectar normalmente, e logo já verifiquei os dados que eu sabia que seriam os mais recentes e estava tudo lá, tudo normal, não parecia ter acontecido nenhum problema, nenhuma limpeza, e ao olhar novamente no painel estava sendo exibida uma quantidade padrão de usuários conectados.
{: style="text-align: justify;"}

<center><iframe src="https://giphy.com/embed/j6aoUHK5YiJEc" width="480" height="466" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/mrw-friday-rei-j6aoUHK5YiJEc">via GIPHY</a></p></center>

Fiquei parado olhando para a tela do meu computador por pelo menos uns cinco minutos tentando entender o que aconteceu e então resolvi conversar com um outro colega que era mais velho de casa, onde falei que não estava conseguindo conectar no banco um pouco mais cedo e o painel estava mostrando zero usuários conectados, e perguntei se ele sabia o que tinha acontecido, onde ele disse:
{: style="text-align: justify;"}

*\- Ah sim, ficou um tempinho fora mesmo, o prestador de serviços onde ficam nossos servidores anda tendo instabilidade, já aconteceram outras vezes, mas agora normalizou. Infelizmente eles raramente nos avisam dessas caídas repentinas.*
{: style="text-align: justify;"}

...

Eu não sabia se ria ou se chorava com essa informação depois de todo o stress que passei, mas só o fato de ter a confirmação que o banco de dados de produção não foi apagado já foi o suficiente para eu ficar de 'ressaca laboral' no resto do dia, e comemorar o fato de não precisar procurar outro emprego rsrs, e assim se encerrou a maior coincidência que já aconteceu na minha vida profissional.
{: style="text-align: justify;"}

## Lições aprendidas

Nesse causo só existiu uma lição aprendida, que é algo que eu já falei anteriormente nesse post [aqui](https://murilo.tech/posts/peca-ajuda-ofereca-ajuda-nao-seja-bobo/){:target="_blank"}: 
{: style="text-align: justify;"}

### Na dúvida, pergunte!

Por mais que possa parecer incômodo, por mais que pareça ser algo simples e por mais que as consequências possam ser muito pequenas, se você está em dúvida sobre alguma coisa, vá e peça a informação, tire essa dúvida para ter a certeza de que você está fazendo a coisa certa e fique livre de suposições caso algo dê errado, que foi exatamente o que aconteceu comigo.
{: style="text-align: justify;"}

Apesar de essa ter sido uma coincidência terrível, existem vários casos onde desenvolvemos ferramentas utilitárias para realizar manutenções em banco de dados, arquivos, serviços etc, que não são possíveis de se realizar manualmente, onde primeiro testamos em nossa máquina para depois alterar a configuração e apontar para o servidor de produção, e se algum desavisado executa esse utilitário sem obter informações precisas, pode acabar transformando a coincidência por qual eu passei em realidade, por isso sempre devemos buscar essa informações.
{: style="text-align: justify;"}

Inclusive, para finalizar a história, após o ocorrido eu fui até um colega e perguntei como limpar essa base de dados local da aplicação e ele me mostrou que ela é composta somente por uma pasta contendo vários arquivos, e bastava apagá-la para limpar essa base.
{: style="text-align: justify;"}

Se eu soubesse disso antes com certeza não teria quase infartado nesse dia.
{: style="text-align: justify;"}

Até a próxima!
{: style="text-align: justify;"}