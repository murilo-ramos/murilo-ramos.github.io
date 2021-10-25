---
title: Causo - A minha primeira parada em produção
author: Murilo Costa
date: 2021-10-25 08:00:00 -0300
categories: [Blogging, Causo]
tags: [causo, historia, relato, java, mysql, fabrica, hibernate]
---

Hoje estou inaugurando uma seção nesse blog para contar e relatar histórias que vivi atuando como um profissional na área de desenvolvimento de software, ou como gosto de chamar: Os famosos causos!
{: style="text-align: justify;"}

A ideia é tentar contar relatos de situações inusitadas, problemas e desafios que enfrentei ao longo da carreira (por mais que ainda seja curta) e tentar passar as lições aprendidas com cada experiência obtida nesse mundo dinâmico e inesperado que é a tecnologia da informação.
{: style="text-align: justify;"}

O causo de hoje é a minha primeira experiência com uma parada brusca de software em produção, algo que ficou marcado na minha memória e me trouxe bastante experiência para lidar com problemas do tipo em situações posteriores.
{: style="text-align: justify;"}

Bom, é isso, chega de introdução e vamos lá!!!
{: style="text-align: justify;"}


## O causo

Como já mencionei em alguns posts anteriores, em 2011 eu trabalhei por um longo período em um fábrica, e lá foi meu primeiro emprego como desenvolvedor com carteira registrada, que entrei quando ainda estava no último ano da faculdade.
{: style="text-align: justify;"}

Ao começar a trabalhar nessa empresa eu acabei substituindo um funcionário que saiu para uma outra oportunidade, e, como em toda substituição, acabei 'herdando' algumas de suas responsabilidades lá dentro, onde uma dessas responsabilidades foi evoluir um software feito em Java, pois eu já tinha alguma experiência na linguagem que foi adquirida em um estágio. Como a equipe era pequena, nós trabalhávamos não só no código como também na implantação, monitoramento e suporte especializado de nossas aplicações.
{: style="text-align: justify;"}

Esse software era uma aplicação requisitada pela área de qualidade e engenharia de processos, sendo utilizado na linha de produção para fazer a conferência final dos produtos, validando suas informações com informações de entrega e garantindo que o produto estava ok e a entrega seria feita corretamente.
{: style="text-align: justify;"}

O software era bem simples, somente uma aplicação Java para desktop usando a plataforma [Swing](https://pt.wikipedia.org/wiki/Swing_(Java)){:target="_blank"}, uma conexão com banco de dados [MySQL](https://www.mysql.com/){:target="_blank"} usando [JPA/Hibernate](https://hibernate.org/){:target="_blank"} e nada mais que isso. Mas, como já mencionei anteriormente, não é por um software ser simples que ele não é problemático, e logo que comecei a realizar manutenções e adições de funcionalidades já me deparei com muito código mal escrito e arquitetado, onde inclusive presenciei uma das maiores maluquices na utilização de banco de dados que já vi na vida.
{: style="text-align: justify;"}

O ponto vital da aplicação era o registro de logs das conferências, que eram armazenados em uma tabela para saber o que já fora conferido e realizar consultas posteriores, e, como toda boa estrutura de log, existia um campo contendo a data da conferência, onde para a minha incredulidade essa data era armazenada em formato texto. Para ficar pior, como existiam consultas feitas por data nessa tabela, o código fazia uma busca com uma concatenação de cláusulas no `WHERE`, procurando dia por dia, por exemplo: `WHERE LOGDATE = '01/01/2012' OR LOGDATE = '02/01/2012' OR LOGDATE = '03/01/2012'` e assim por diante até chegar na data final, algo totalmente fora de qualquer padrão de desenvolvimento de software, por isso uma das primeiras coisas que fiz foi realizar uma boa refatoração no código, redesenhar processos e melhorar a estrutura de banco de dados da aplicação.
{: style="text-align: justify;"}

Quando passei a lidar com esse sofware ele não era algo de extrema importância na linha de produção, pois eram somente alguns modelos de produtos em que era necessário utilizá-lo, onde podemos dizer que no máximo 25% de toda a produção passava pelo software, mas um belo dia isso mudou. Devido a algumas falhas de qualidade, passaram a considerar esse software como a solução para diversos problemas que estavam ocorrendo, e então, em um período de dois meses mais ou menos, o software passou a ser utilizado para 100% de toda a produção da fábrica, aumentando significativamente a quantidade de máquinas utilizando a aplicação, a quantidade de dados, e principalmente a responsabilidade do software, pois agora se ocorresse alguma pane a linha de produção seria impactada pelo problema.
{: style="text-align: justify;"}

Mas, apesar dessa mudança radical, eu posso dizer que estávamos bem preparados para o que estava por vir. As refatorações e melhorias no software aumentaram consideravelmente sua performance, e com a ajuda da equipe foi possível redesenhar o estrutura do banco melhorando as definições de campos e criações de índices, o que deixou o software pronto para essa nova etapa, tudo isso alinhado ainda ao fato de que recentemente havíamos mudado o servidor de banco de dados MySQL para uma máquina nova, com mais memória, processador, disco e dessa vez com com o S.O. Linux, pois anteriormente era um Windows Server, então estava tudo bem. Após essas mudanças, progressivamente a produção foi aumentando e em torno de três meses depois mais ou menos o software já estava atendendo 100% da produção, o que garantiu muito mais qualidade aos produtos da empresa.
{: style="text-align: justify;"}

Foi então que em uma calma e tranquila quinta-feira eu passei pelo meu primeiro outage em produção.
{: style="text-align: justify;"}

Após mais um dia normal de trabalho, as 17:05 eu bati o ponto na empresa e fui embora como de costume, e ao chegar em casa descansei um pouco e comecei a me preparar para a aula de inglês que eu cursava na época, que começava as 19hrs. Quando foi em torno de 18hrs mais ou menos eu recebo uma ligação da equipe de suporte de T.I. do segundo turno, me perguntando se eu estava realizando alguma manutenção no software, pois algumas máquinas estavam com comportamento estranho. Esse tipo de contato do suporte era mais ou menos comum pois muitas vezes as máquinas (computadores) onde ficava instalado o software tinham problemas e precisavam ser reiniciadas (muitas máquinas já tinham alguma idade), e um problema que acontecia com frequência era a desconexão de uma unidade de rede necessária para o software, que impossibilitava o sistema de obter os dados de conferência via leitura de arquivos, e assim o software não podia ser utilizado (infelizmente nessa época as mensagens ao usuário estavam mais genéricas). Então quando me ligaram eu pedi para verificarem a questão da unidade de rede mapeada e pedi para reiniciarem as máquinas com problema, pois isso já resolvia qualquer erro de memória, sistema operacional, e o login no Windows após reset já refazia a conexão com a unidade de rede, por isso reiniciar os computadores era o melhor remédio. Passados uns quinze minutos dessa primeira ligação não tive mais nenhum retorno e então considerei que estava tudo certo, e fiquei esperando dar o horário de ir para a aula de inglês.
{: style="text-align: justify;"}

Quando foi chegando umas 18:40 o telefone toca e era o pessoal do suporte novamente dizendo que o problema persistia, e dessa vez foram mais específicos detalhando o problema: Algumas máquinas estavam funcionando normalmente enquanto outras não conseguiam abrir o software, exibindo a splash screen durante alguns minutos e então era exibido um erro 'Não foi possível iniciar o sistema', fechando a tela.
{: style="text-align: justify;"}

Apesar de eu já ter visto esse comportamento anteriormente esse problema era bem estranho, pois esse era um erro genérico que a aplicação exibia quando não conseguia obter os recursos necessários para iniciar, que basicamente eram inicializar alguns atributos internos, ler um arquivo de configuração e estabelecer a conexão com o banco de dados. Em alguns problemas de infraestrutura que ocorreram num passado recente, o servidor de banco de dados ficou offline durante um breve período, onde esse erro passou a ser exibido pela aplicação pois ela não conseguia mais conexão com o MySQL, mas nesse caso era muito estranha a situação de algumas máquinas conseguirem iniciar a aplicação e outras não, pois se o banco de dados estivesse offline nenhuma máquina iria conseguir realizar a conexão.
{: style="text-align: justify;"}

Nesse caso eu pedi para o suporte fazer um check do arquivo de configuração para ver se as informações estavam ok (isso bastava ser feito em uma só máquina pois elas acessavam o software a partir de uma unidade de rede, então o software/configuração era igual para todos) e também pedi para fazer um teste de ping para o servidor de banco de dados nas máquinas com problema, para verificar a questão da conectividade.
{: style="text-align: justify;"}

Apesar de agora o problema parecer um pouco mais sério e talvez requisitar minha presença lá na fábrica, resolvi esperar o retorno do pessoal do suporte, pois se caso fosse um problema de conectividade não existia muito o que eu pudesse fazer, visto que essa parte era comandada pela equipe de T.I./Infraestrutura, então entrei no carro e segui meu caminho para a aula de inglês.
{: style="text-align: justify;"}

Mas no meio do caminho recebo mais uma ligação do suporte dizendo que as configurações estavam corretas e os testes de ping também foram ok, com a informação adicional de que agora somente uma máquina estava conseguindo usar o software, e foi nesse momento que mudei a rota desistindo da aula de inglês e indo para fábrica ver o problema de perto. Com essa última informação eu vi que o problema podia ser algo mais grave, como o servidor de banco de dados rejeitando conexões ou com algum problema de lock, ou então poderia estar ocorrendo algum problema na aplicação que eu não previa.
{: style="text-align: justify;"}

Uma informação importante é sobre como funcionava a estrutura física da fábrica, que por trabalhar com uma tecnologia de alta segurança ela possuía um ambiente produtivo totalmente separado do ambiente administrativo, inclusive com controle de acesso por cartão (e posteriormente biometria), e por isso para ir até o ambiente onde estavam as máquinas de produção eu precisava ir para outro prédio, mas como a equipe de suporte já estava lá decidi ir até o nosso escritório de trabalho no prédio administrativo, onde eu poderia ver o código do sistema e, caso precisasse analisar o servidor de banco de dados, eu poderia usar uma sala especial que ficava anexada ao nosso escritório para acessar o ambiente de produção (também com controle de acesso). Então as 19:30 eu já estava no escritório e fui direto para essa sala especial para acessar e verificar o estado do servidor de banco de dados (como será visto no final, esse foi o meu maior erro).
{: style="text-align: justify;"}

Como aparentemente as configurações de inicialização da aplicação estavam corretas e não existiu nenhuma mudança no software que foi implantada naquele dia ou semana, a maior causa provável do problema estava relacionado ao banco de dados, onde a aplicação não estava conseguindo estabelecer uma conexão. Como o pessoal de suporte informou que a conectividade com o servidor estava ok a partir do ping, minha melhor sugestão é que estava ocorrendo algum problema no MySQL e ele não estava mais aceitando conexões. Verifiquei a utilização processador, memória e disco da máquina e estava tudo ok, verifiquei também seus logs para observar algum erro ou problema, mas para me deixar mais intrigado ainda não havia nenhum log de erro. Aparentemente o problema não estava no servidor, mas então por que somente uma máquina estava conseguindo se conectar ao banco de dados? Ao olhar no gerenciador de conexões do MySQL dava para ver as únicas duas conexões ativas lá (a única maquina funcionando e o próprio gerenciador de conexões), e parecia tudo bem. Fiz também um teste de conexão para esse servidor a partir de uma outra máquina em um terceiro ambiente também ligado a produção e o teste ocorreu com sucesso, e isso me fez pensar: será que pode estar existindo algum problema de firewall entre as redes que está causando isso? Se esse fosse o caso o teste de ping deveria falhar também, mas a esta altura do campeonato já nos aproximando das 20hrs eu resolvi fazer experimentações para tentar encontrar o problema.
{: style="text-align: justify;"}

A primeira experimentação foi pedir para reiniciar aquela única máquina que ainda estava com o software funcionando, e parecia estar conseguindo se conectar com o servidor. Este teste visava verificar se poderia ser algum problema de rede/rota/configuração IP, pois ao reiniciar a máquina as configurações de rede seriam atualizadas e poderíamos ver se o software iria começar a apresentar o mesmo problema das outras máquinas. Infelizmente não tivemos nenhuma mudança, após reiniciar essa máquina o software continuou funcionando normalmente nela, aumentando um pouco a minha frustração.
{: style="text-align: justify;"}

Agora eu já tinha chegado num ponto onde minhas alternativas já estavam se esgotando, e considerando que o problema parecia estar realmente no software (ou pelo menos no desespero eu estava enxergando assim rs) eu visualizei somente mais duas experimentações: Fazer uma análise no código para tentar encontrar algum problema ou então tentar a alternativa final que era reiniciar o banco de dados.
{: style="text-align: justify;"}

Mesmo descrente resolvi tentar a primeira alternativa, voltei para o escritório abri o código e comecei a analisar toda a parte de inicialização do software, os objetos instanciados de uso compartilhado, a leitura das configurações, principalmente as configurações de conexão com o banco de dados, e o famoso [singleton](https://www.onlinetutorialspoint.com/hibernate/singleton-hibernate-sessionfactory-example.html){:target="_blank"} de conexão com o banco utilizado no Hibernate, e não consegui encontrar nada de errado. Fiz pesquisas na internet comparando meu código com exemplos e não achei nenhuma divergência, tentei também fazer buscas por palavras-chave do tipo 'somente algumas máquinas conseguem se conectar ao servidor de banco de dados' e achei problemas dos mais diversos tipos, mas nenhum deles se encaixava na minha situação. Por fim me convenci que o problema não estava no software, pois realmente não fazia nenhum sentido ter várias máquinas iguais, mesmo sistema operacional, mesma rede, mesmas configurações e somente uma estar com a aplicação funcionando, e fazia menos sentido ainda isso ser um problema do software, então fui para a segunda alternativa que era reiniciar o banco de dados.
{: style="text-align: justify;"}

Apesar das análises anteriores não terem indicado nenhum problema no banco de dados, eu ainda desconfiava que podia estar acontecendo algum problema no servidor que não estava aparecendo nos logs e indicadores, por isso iria testar essa última alternativa.
{: style="text-align: justify;"}

Quando eu digo 'reiniciar o servidor do banco de dados' talvez não transpareça o tamanho do peso e receio que se têm ao realizar um processo desse, pois existem várias coisas que podem acontecer ao se tentar essa manobra, e somente uma delas é positiva: 
{: style="text-align: justify;"}

1. No melhor dos casos, após reiniciar o servidor o problema poderia ser resolvido, todas as máquinas voltariam a usar o software normalmente e caso encerrado;
2. Após reiniciar o servidor poderíamos não ter nenhuma diferença no resultado, continuando somente com aquela máquina funcionando e as outras continuando com o problema, o que seria ruim pois somente perderíamos tempo sem ganho algum;
3. Existia a possibilidade de acontecer também algo pior, onde após reiniciar o servidor aquela última máquina poderia parar de funcionar, complicando ainda mais a situação;
4. E por último poderíamos ter o problema mais caótico de todos, que é o banco de dados MySQL não iniciar mais no servidor, e se isso acontecesse teríamos um enorme problema nas mãos, podendo a coisa ficar ruim a ponto de ter que restaurar um backup, o que poderia levar horas.

Somente quem já lidou com operações em produção já passou por situações complicadas e tenebrosas como essa rsrs, mas considerando que esse servidor nunca tinha apresentado problemas desse tipo antes, resolvi seguir em frente e reiniciá-lo. Após reiniciar o servidor, para o bem e para o mal, acabamos tendo como resultado o item 2) da lista anterior, onde o MySQL subiu normalmente e somente aquela máquina continuou apta a utilizar o software, ou seja, continuamos na mesma e foi perdido somente  tempo.
{: style="text-align: justify;"}

Agora sim eu podia dizer que estava bem desesperado com a situação, pois tudo que eu havia tentado falhou miseravelmente e as alternativas se esgotaram. Nesse momento começou a ficar claro para mim que esse problema poderia não ser solucionado naquela noite e eu teria que dizer isso para meus superiores, que inclusive estavam lá me ajudando.
{: style="text-align: justify;"}

Tratando-se de uma fábrica isso tinha um grande impacto pois naquele horário, em torno de 20:30, a utilização do software era baixa devido a outros processos que estavam ocorrendo na linha de produção (por isso que eu ainda não tinha me desesperado até o momento rs), porém a partir das 4hrs da madrugada seu uso voltava a se intensificar, e se o software não voltasse ao normal até esse horário teríamos que comunicar os gestores e líderes das linhas e pensar em um plano alternativo para que elas não ficassem paradas.
{: style="text-align: justify;"}

Então nesse momento teríamos que chamar mais pessoas da equipe para tentar ajudar e tentar mais experimentações, como compilar uma versão alternativa do software com mensagens de debug e nível mais alto de log e colocar para rodar em produção para tentar obter informações adicionais que ajudem na solução do problema. Mas antes de realizar essas ações resolvi olhar o problema de perto, coloquei meus [EPIs](http://www.guiatrabalhista.com.br/tematicas/epi.htm#:~:text=O%20Equipamento%20de%20Prote%C3%A7%C3%A3o%20Individual,seguran%C3%A7a%20e%20a%20sua%20sa%C3%BAde.){:target="_blank"} e me dirigi até o prédio de produção, na tentativa de observar algum detalhe que a equipe de suporte de T.I. possa ter deixado passar. Depois dos vários testes e análises que fiz não fazia sentido algum o problema estar no software, no banco de dados ou nas configurações, e eu tinha uma forte suspeita que poderia ser algo da rede/infraestrutura, mas eu precisava de alguma evidência apontando nesse sentido.
{: style="text-align: justify;"}

Ao chegar no ambiente onde ficavam as máquinas me deparei com o cenário real, onde de 35 máquinas que poderiam estar usando o software somente uma estava em operação. Conversei com o pessoal do suporte e me disseram mais alguns testes que fizeram tentando conexão com outros caminhos, e disseram que estava tudo ok, e nada de encontrarmos o problema. Foi aí que eu tive a ideia de me sentar em uma das máquinas em que o software não estava funcionando e realizar uma análise pessoal.
{: style="text-align: justify;"}

Verifiquei a instalação do software e estava tudo ok, os arquivos estavam ok (apesar de eu não conseguir ver o conteúdo em texto das configurações por conta dos bloqueios de segurança), e ao iniciar ele ficava mostrando a splash screen por um minuto e então o erro 'Não foi possível iniciar o sistema' aparecia. E então fui fazer o teste final da minha análise naquela máquina que era disparar o ping para o servidor do banco de dados, e ao fazer isso:
{: style="text-align: justify;"}

    > ping mysql.database.local

    Pinging mysql.database.local with 32 bytes of data:
    Reply from 192.168.0.42: Destination host unreachable.
    Reply from 192.168.0.42: Destination host unreachable.
    Reply from 192.168.0.42: Destination host unreachable.
    Reply from 192.168.0.42: Destination host unreachable.

    Ping statistics for mysql.database.local:
        Packets: Sent = 4, Received  = 4, Lost = 0 (0% loss).

Não acreditei! 
{: style="text-align: justify;"}

Aquela máquina não estava encontrando o servidor de banco de dados e com certeza esse era o problema, mas o pessoal do suporte me havia informado que eles fizeram esse teste e ocorrido com sucesso. Fui em outras máquinas que estavam com o problema e todas elas apresentaram o mesmo erro no ping.
{: style="text-align: justify;"}

Diante dessa situação eu voltei correndo para o pessoal, expliquei a situação e questionei o porquê eles disseram que o teste de ping estava ok, onde recebi a resposta:
{: style="text-align: justify;"}

"Sim, está ok, nós testamos naquela máquina ali".
{: style="text-align: justify;"}

E apontaram justamente para a ÚNICA máquina onde o software estava funcionando...
{: style="text-align: justify;"}

<center><iframe src="https://giphy.com/embed/11ahZZugJHrdLO" width="480" height="428" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/what-the-fuck-11ahZZugJHrdLO">via GIPHY</a></p></center>

Sim, todos os testes realizados pela equipe de suporte de T.I. foram feitos somente na única máquina que estava funcionando, e não nas máquinas que não estavam funcionando.
{: style="text-align: justify;"}

...

Depois de um longo suspiro e um conflito de sentimentos de tristeza, felicidade, raiva e alívio, chamei todo o pessoal de T.I./Infraestrutura e mostrei que as máquinas que não conseguiam  abrir o software não estavam conseguindo conexão com o banco de dados, o que indicava um problema de rede, e então, depois de varrerem o hack de equipamento de rede, verificaram que o problema estava no switch em que os computadores estavam ligados, que estava apresentando mau funcionamento e provavelmente 'morrendo', por isso algumas coisas funcionavam na rede para algumas máquinas e outras não.
{: style="text-align: justify;"}

Após identificado o problema, em torno de 21hrs, a equipe de infraestrutura procedeu com a troca do switch e as 21:30 o software voltou a funcionar em todas as máquinas, encerrando o caso e eu podendo ir embora para casa.
{: style="text-align: justify;"}

## Lições aprendidas

### Veja o problema pessoalmente

Eu considero que o meu maior erro em toda a situação foi não ter ido primeiro no local de produção ver o problema acontecer de perto, talvez se eu tivesse ido lá primeiro e feito os testes eu poderia ter identificado o problema mais rapidamente e assim não impactado a produção por tanto tempo. Apesar de já existirem pessoas lá me passando as informações elas não tinham o domínio técnico sobre o software, por isso esse telefone-sem-fio de informações acabou complicando as coisas. Então, sempre que possível, tente ver o problema pessoalmente.
{: style="text-align: justify;"}

### Comunicação direta e específica

Em meu relato parece evidente que ocorreu uma falha de análise por parte da equipe de suporte de T.I. que fizeram os testes somente na máquina que estava funcionando, e não nas demais, porém não posso colocar a culpa neles pois eu não fiz uma comunicação clara dos objetivos do teste. Provavelmente eles entenderam que o teste do ping era para saber se o servidor estava de pé e este teste foi ok, mas não pensaram no teste como uma forma de verificar também se as máquinas conseguiam conexão com o servidor, o que eu podia ter deixado claro desde o início, e isso foi claramente uma falha de comunicação. Sempre que precisar passar instruções a outras pessoas seja bem direto e específico quanto ao que precisa ser feito e aos resultados esperados, incluindo contexto.
{: style="text-align: justify;"}

### Mensagens clara e objetivas fazem um software melhor

Ficou claro no relato que a falta de mensagens mais objetivas e menos genéricas poderiam ter feito a diferença em toda a análise. Se o software exibisse algo como 'Não foi possível se conectar ao banco de dados' ficaria muito mais evidente o problema e aceleraria a análise. Boas mensagens melhoram em muito o uso do seu software informando melhor o usuário e ajudando em problemas.
{: style="text-align: justify;"}

### Todo bom software possui log

Outro ponto que também ficou claro no relato foi a falta de um log da aplicação, que poderia ter auxiliado e muito na análise. No caso desse software até existia um log, porém para ativá-lo era necessário um processo super trabalhoso, e ainda após ativá-lo o log ia somente para o console, algo que era desabilitado a visualização nas máquinas de produção por questões de segurança (por conta do alto ambiente de segurança nem mesmo editores de texto podiam ser utilizados nessas máquinas), por isso também não adianta nada ter um log que não pode ser acessado. Assim, colocar um mecanismo de log que possa ser ativado facilmente, com informações importantes e que se adapte ao ambiente em que o software é executado é algo de extrema importância em casos de problemas na aplicação.
{: style="text-align: justify;"}

É isso aí pessoal, espero que tenham gostado desse causo e até a próxima.
{: style="text-align: justify;"}
