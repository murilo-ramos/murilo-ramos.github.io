---
title: Processos de Software
author: Murilo Costa
date: 2021-06-25 08:00:00 -0300
categories: [Blogging]
tags: [processo, software, scrum, xp, kanban, management]
---

Há algum tempo fiz um [post](https://murilo.tech/posts/ferramentas-para-o-ambiente-de-desenvolvimento/){:target="_blank"} falando sobre as ferramentas que eu entendo serem as mais importantes em um ambiente de desenvolvimento de software, e acabei citando também que, além de ferramentas, os processos são outro ponto super importante em um time de desenvolvimento.
{: style="text-align: justify;"}

Desenvolver software, apesar de ter suas particularidades, é um trabalho como qualquer outro, que precisa de um mínimo de planejamento e organização para uma execução bem sucedida, caso contrário ele pode virar um caos e impactar diretamente nas entregas de uma equipe. Uma pessoa ou time pode até tentar desenvolver o trabalho de um jeito qualquer, mas eu garanto que os problemas no dia-a-dia serão intensos, e isso implicará em muito pouco progresso, baixa qualidade e muita dor de cabeça, principalmente quando já em fase avançada.
{: style="text-align: justify;"}

E para se alcançar uma organização no trabalho é que surgiram os processos de software, que de alguma forma tentam planejar as atividades e organizar a forma de realizá-las, provendo um balanço entre qualidade e produtividade. Mas não pense que existe uma forma fácil e pronta de chegar nesse objetivo, pois a verdade é que essa é uma área de estudo que está em constante aprimoramento desde o início da computação, e ainda tem muito o que evoluir.
{: style="text-align: justify;"}

Por isso o meu objetivo com esse post não é mostrar todos os processos de software que existem no mundo, nem muito menos tentar compará-los para dizer qual o melhor. A minha ideia aqui é apresentar um pouco dos mais conhecidos e tentar mostrar de alguma forma que realmente não existe uma bala de prata e que o melhor processo é aquele que melhor funciona para você e sua equipe.
{: style="text-align: justify;"}


## **O começo - Cascata**

O primeiro processo de desenvolvimento de software que apareceu foi o famoso modelo [Cascata](https://pt.wikipedia.org/wiki/Modelo_em_cascata){:target="_blank"} (ou waterfall), que é um modelo linear e segue a dinâmica utilizada em processos de construção das mais diversas áreas. Este processo divide todo o trabalho em grandes etapas, onde uma etapa só pode ser iniciada após a conclusão da anterior, e ao final da última etapa o trabalho está concluído.
{: style="text-align: justify;"}

![cascata](/assets/img/posts/processos-de-software/waterfall.png)

Mesmo sendo um processo bem óbvio e que funciona para a maioria das outras áreas, infelizmente para o desenvolvimento de software ele se mostrou ineficiente ao longo do tempo por uma série de motivos, como atrasos em etapas inicias que impediam o início das próximas etapas, o retrabalho constante que faz com que etapas precisem ser regredidas, e principalmente o tempo que se leva para construir algo que possa ser exibido para o requisitante do projeto, pois é somente no final que vamos ter algo que possa ser mostrado.
{: style="text-align: justify;"}

E justamente para tentar resolver os problemas do modelo cascata é que surgiram outros processos, mais modernos e que permitem uma melhor forma de trabalho. Veremos adiante alguns deles.
{: style="text-align: justify;"}

## **Os mais usados**

Atualmente os processos de software mais conhecidos e usados são o Extreme Programming (XP), Scrum e Kanban, e vamos falar um pouco sobre eles.
{: style="text-align: justify;"}

### XP - Extreme Programming (Programação Extrema)

O [extreme programming](https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_extrema){:target="_blank"} é um modelo de desenvolvimento ágil no formato iterativo e incremental, que tem como seus valores básicos a **comunicação** estreita e informal, **simplicidade** de fazer somente o necessário, o **feedback** constante, e a **coragem e respeito** para pensar sempre no presente e cultivar um bom relacionamento entre a equipe. O modelo iterativo e incremental do XP permite um ciclo menor de desenvolvimento, onde este ciclo é direcionado a somente uma parte do que é desenvolvido, para que sempre após uma iteração seja possível já mostrar algo pronto e permitir entregas rápidas.
{: style="text-align: justify;"}

![xp](/assets/img/posts/processos-de-software/xp.png)

O ponto forte do XP está na comunicação entre a equipe, que é sempre feita de forma informal, desenvolvendo uma boa relação entre as pessoas do time, e é por isso que a prática de programação em par é muito utilizada nesse modelo.
{: style="text-align: justify;"}

### Scrum

O [Scrum](http://www.desenvolvimentoagil.com.br/scrum/){:target="_blank"} também é um modelo de desenvolvimento ágil iterativo, possuindo algumas das mesmas características do XP com a diferença que ele preza por equipes multifuncionais, inclusive com membros da equipe tendo papéis bem definidos no processo.
{: style="text-align: justify;"}

Os principais papéis são o Scrum Master, que atua como líder da equipe e responsável por auxiliar os membros em suas atividades, organizar o trabalho e resolver conflitos e obstáculos, e o Product Owner, que atua como um representante do cliente, definindo prioridades, provendo feedbacks e auxiliando a equipe a ir na direção esperada, incluindo o **que** e **quando** deve ser entregue.
{: style="text-align: justify;"}

![scrum](/assets/img/posts/processos-de-software/scrum.png)

A grande vantagem do Scrum eu acredito estar em seu formato de trabalho, que já traz uma espécie de receita de bolo com todas as etapas a serem realizadas ao longo de um projeto, o que fica um pouco menos genérico que outras metodologias, permitindo uma rápida adoção.
{: style="text-align: justify;"}

Nesse formato de trabalho, as principais etapas de um  desenvolvimento com Scrum são:
{: style="text-align: justify;"}

- **Construção do backlog:** Criação de uma lista priorizada de tudo que deve fazer parte do produto/software construído. Essa lista geralmente é criada em um workshop envolvendo no mínimo o Scrum Master e Product Owner.
{: style="text-align: justify;"}
- **Planning:** Uma reunião com todos os membros da equipe para discutir e definir o que será desenvolvido na sprint (período de trabalho) atual. Essa reunião costuma ser um pouco demorada pois irá definir tudo em que a equipe irá trabalhar e entregar nas próximas semanas.
{: style="text-align: justify;"}
- **Daily meeting:** Reuniões diárias com a equipe sempre tendo em pauta o que foi feito anteriormente, o que está sendo feito agora, quais são os obstáculos, e o que será feito a seguir. Essas reuniões auxiliam na avaliação diária do andamento do trabalho, sendo reuniões curtas de 15 a 20 minutos entre a equipe, sem a necessidade do Product Owner.
{: style="text-align: justify;"}
- **Revisão e retrospectiva:** Uma reunião realizada com toda a equipe no final da sprint para apresentar o que foi feito, os acertos, os erros e obter mais feedback, obtendo experiência para a próxima sprint. Quando cabível são realizadas entregas também.
{: style="text-align: justify;"}

Uma curiosidade é que o termo Scrum vem de um movimento praticado pelos times de Rugby, onde os jogadores se unem abraçados e [empurram o time adversário](https://pt.wikipedia.org/wiki/Scrum_(rugby)){:target="_blank"} para ganhar a posse de bola.
{: style="text-align: justify;"}

*Obs: Alguns termos não foram traduzidos do inglês pois fazem parte da nomenclatura do método.*
{: style="text-align: justify;"}

### Kanban

Uma metodologia que ultimamente vem sendo muito adotada em projetos de software é o [Kanban](https://pt.wikipedia.org/wiki/Kanban){:target="_blank"}, sendo um processo bem simplificado e visual, inspirado no sistema de produção da [Toyota](https://www.toyota.com.br/mundo-toyota/toyota-production-system/){:target="_blank"} chamado JIT - Just in Time. A primeira coisa que precisamos esclarecer sobre o Kanban é que existe uma diferença entre a metodologia Kanban e o quadro Kanban, que faz parte do método e também é utilizada em outros metodologias, como é o caso do XP e Scrum. 
{: style="text-align: justify;"}

O método consiste em se criar um quadro contendo os estados das tarefas, sendo os estados iniciais mais a esquerda e os estados finais mais a direita, e então colocar as tarefas no quadro e no grupo que representa seu estado atual, tendo sempre o cuidado de verificar da direita para a esquerda para primeiro finalizar os itens pendentes e depois trabalhar em novos itens, e também visualizar possíveis tarefas que estejam sendo gargalo ou bloqueadas, assim como acúmulo de tarefas.
{: style="text-align: justify;"}

![kanban](/assets/img/posts/processos-de-software/kanban.jpg)

Na metodologia Kanban o quadro é um item fundamental pois é nele que os estados de progresso são estabelecidos, porém existe também todo um processo de utilização do quadro que não precisa seguir exatamente o clássico ToDo/Doing/Done, devendo sempre ser adaptado as suas necessidades, com os estados que forem necessários.
{: style="text-align: justify;"}

Enquanto outras metodologias se encaixam muito bem em projetos que tem um scopo plenamente definido, ou pelo menos semi definido, eu acredito que o Kanban acaba atendendo melhor projetos que tendem a ser muito mais dinâmicos, com tarefas que podem aparecer a qualquer momento e a equipe precisa se organizar para atendê-las. Usando outras metodologias seria necessário realizar um planejamento mais elaborado, reunir as tarefas e então começar uma sprint, porém o Kanban não dita essa regra, onde podemos simplesmente adicionar a tarefa no quadro e qualquer um da equipe pode a qualquer momento puxar a tarefa para si e começar a trabalhar.
{: style="text-align: justify;"}


## **Qual processo devo usar?**

A adoção de um processo de software na sua empresa ou projeto sempre vai depender de como você espera que o desenvolvimento seja conduzido, qual o perfil da sua equipe e até mesmo como espera trabalhar com o cliente, e com essas informações será possível escolher qual o modelo mais adequado.
{: style="text-align: justify;"}

Se sua equipe possui uma boa comunicação e os integrantes tem o hábito de trabalhar juntos, talvez seja bastante interessante adotar o XP praticando a programação em par, que é algo de fácil adoção nesse método e comprovadamente aumenta a produtividade e qualidade do desenvolvimento. Caso você já tenho um projeto pronto para ser iniciado e que pode ser realizado em fases, talvez o Scrum com suas sprints elaboradas e participação ativa do cliente possa ser mais interessante. E se você está em um ambiente de melhorias e implementações de novas funcionalidades em um sistema já existente, onde as demandas aparecem de forma dinâmica, então acredito que o Kanban pode se ajustar muito bem ao seu dia-a-dia.
{: style="text-align: justify;"}

Mas e se nenhum dos modelos e processos for interessante para o meu caso?
{: style="text-align: justify;"}

Sem problemas, elabore seu próprio processo!
{: style="text-align: justify;"}

Usar um modelo já existente não é uma regra absoluta, e por isso em muitos casos é extremamente aconselhável a criar o seu próprio processo, ou adaptar de processos existentes, para atender seus casos específicos sempre tendo o foco na produtividade da equipe e qualidade do software desenvolvido.
{: style="text-align: justify;"}

E baseado em minhas experiências, acredito que existem alguns atributos que todo bom processo de software deve contemplar, como veremos a seguir.
{: style="text-align: justify;"}

### Processo iterativo

Provavelmente você deve ter percebido que existe um atributo em comum com os processos mais usados, que é o fato de serem iterativos. Ser iterativo significa que o modelo trabalha com iterações, ou seja, ciclos com um início, meio e fim, que são aplicados a partes do desenvolvimento, e é exatamente isso que faz toda a diferença em um processo de desenvolvimento de software.
{: style="text-align: justify;"}

Apesar do modelo de desenvolvimento linear (cascata) ter uma lógica que aparentemente funcionaria para desenvolver um software, existem muitos problemas específicos que acontecem no desenvolvimento que atrapalham seu uso, como especificações que mudam a todo momento, falta de clareza nos detalhes e até o rápido avanço da tecnologia, que no fim acabam implicando na construção de um software que não atende ao objetivo do cliente.
{: style="text-align: justify;"}

É até um pouco difícil falar sobre isso pois não faz muito sentido um modelo linear não funcionar. Se você analisa, planeja, executa e entrega conforme combinado, por que iria dar errado? Mas infelizmente é o que acontece nesse modelo quando se fala de software, na maioria dos casos o projeto sofre muitos atrasos e acaba não entregando algo que ajude o cliente nem que agregue valor.
{: style="text-align: justify;"}

Por isso o modelo iterativo funciona melhor, pois como possui pequenos ciclos de desenvolvimento, no final de cada um deles é possível avaliar se está no caminho correto e até mesmo obter a aprovação do cliente, podendo inclusive adotar mudanças que antes não estavam presentes, mas que o modelo iterativo consegue incorporar sem refazer um novo planejamento massivo.
{: style="text-align: justify;"}

Então, seja lá como você esteja planejando seu processo, tente sempre seguir o modelo iterativo pois ele irá te livrar de muitas frustações ao longo dos projetos.
{: style="text-align: justify;"}

### Agilidade

A agilidade é algo que faz muita diferença em um processo de software, e se estudarmos a teoria da agilidade em processos de software com base em tudo que já foi publicado, como o [manifesto ágil](https://agilemanifesto.org/){:target="_blank"}, acho que eu teria que escrever pelo menos uns três posts para contemplar toda informação legal que existe sobre isso por aí, rs.
{: style="text-align: justify;"}

Assim vou falar sobre o que eu vejo como agilidade em um processo, que no meu ponto de vista está na simplicidade das ações e mínimo de burocracia, documentação simplificada e boas práticas de código.
{: style="text-align: justify;"}

Quanto mais simples forem as ações e menor a burocracia, sempre seu projeto irá avançar com mais rapidez. Eu já trabalhei com processos que infelizmente eram extremamente burocráticos, onde muitas vezes se levava mais tempo para fechar uma tarefa no sistema de gerenciamento de projetos do que realmente executar a tarefa, onde sempre depois de reuniões precisava-se preencher atas enormes que ninguém lia, e tudo isso é muito custoso, por isso evite isso ao máximo no seu processo. Uma forma de alcançar isso é definindo fluxos de trabalho mais simplificados e trabalhando com documentação simplificada.
{: style="text-align: justify;"}

A documentação em projetos de software sempre foi um assunto problemático pois é algo também bem custoso para a equipe, mas não precisa ser assim. Existem diversos estudos dentro da [modelagem ágil](http://www.agilemodeling.com/shared/AMPanfleto.pdf){:target="_blank"} que mostram que a documentação não precisa ser aquela coisa extremamente formal, e pode ser na verdade todo material produzido durante reuniões e conversas entre a equipe, como desenhos simples de arquitetura, design, fotos de quadros e etc, que quando devidamente colocados em um sistema para gerenciamento de conhecimento (ex: Wiki), ajuda e muito os membros da equipe.
{: style="text-align: justify;"}

E por último a agilidade também é alcançada quando é produzido código seguindo as boas práticas do desenvolvimento de software, como a criação de testes, rotinas e build automatizados, que vão maximizar muito o tempo do desenvolvedor, seja prevenindo a necessidade de muitas tarefas manuais ou evitando retrabalhos por conta de bugs, e também a prática do código limpo e legível, que irá ajudar muito na manutenção do software ao longo do projeto, e também no seu entendimento dos detalhes, algo que é extremamente custoso de se documentar. Geralmente a melhor forma de ter certeza sobre um detalhe é olhando diretamente no código, por isso a importância de se ter sempre um código limpo e legível.
{: style="text-align: justify;"}


### Feedback rápido e contínuo

Um processo de software muitas vezes não resolve o problema do seu projeto logo de início, e por isso é preciso insistir, analisar onde ocorreram as falhas e entender os acertos para sempre melhorar na próxima iteração, e o nome desse processo é **feedback contínuo**, algo que é indispensável em processos de software atuais.
{: style="text-align: justify;"}

Pense em feedback aqui como um retorno do progresso da equipe. Se esse retorno demorar a aparecer e existirem problemas atrapalhando o andamento do trabalho, esses problemas irão continuar existindo durante um bom tempo até que se tenha um retorno informando os problemas. Agora se for estabelecido o feedback contínuo em curtos períodos, fica muito mais fácil para a equipe ir tentando novas formas de realizar o trabalho, e assim eliminar ou aos menos minimizar os problemas.
{: style="text-align: justify;"}

As reuniões de retrospectiva realizadas no Scrum são um ótimo exemplo de como isso funciona bem.
{: style="text-align: justify;"}


### Comunicação

A comunicação é o maior fator de sucesso de um processo, pois se ela não for clara, sucinta e esclarecedora, a quantidade de desentendimentos nas tarefas e até mesmo entre pessoas pode tornar tudo mais complicado, e por isso que a minha dica aqui é sempre ter a comunicação mais simples e clara possível, colocando todos na mesma página sempre de uma forma que não onere ninguém.
{: style="text-align: justify;"}

Falar é fácil, mas como fazer?
{: style="text-align: justify;"}

Realmente, quando falamos de comunicação estamos falando de pessoas, que têm seus próprios pensamentos e opiniões, por isso manter uma comunicação dentro de um projeto que contribua e agrade a todos é algo bem difícil, mas não impossível. E isso pode ser alcançado abordando os assuntos sempre um por vez, sem tumultuar as reuniões, nunca adiantar assuntos que não são necessários no momento e tirar preocupações desnecessárias dos membros da equipe.
{: style="text-align: justify;"}

As reuniões sempre serão uma forma eficiente de manter a equipe informada e realizar discussões sobre decisões a serem tomadas, mas é preciso ter cuidado para não as realizar em excesso, pois uma equipe que só participa de reuniões acaba produzindo muito pouco (voltamos ao cascata aqui). O ideal é sempre realizar as reuniões iniciais e finais de um ciclo de desenvolvimento com a maior clareza de informações e detalhe possível, para que no restante do tempo os membros da equipe possam realmente atuar em suas tarefas e não fiquem presos em reuniões intermediárias para definição de itens que podiam ter sido definidos no início do trabalho. Caso algum assunto precise ser discutido durante o desenvolvimento, sempre existirão as dailys diárias que podem ser utilizadas para esse fim.
{: style="text-align: justify;"}

E por último:
 - Sempre analise se sua reunião não poderia ser um e-mail;
 {: style="text-align: justify;"}
 - Uma conversa cara-a-cara de quinze minutos muitas vezes evita ciclos intermináveis de e-mails (mas faça isso com moderação para não viver em reuniões);
 {: style="text-align: justify;"}
 - Comunicação sempre envolverá pessoas, então seja ponderado :).
 {: style="text-align: justify;"}


### Cultura

E para finalizar, não adianta ter o melhor processo do mundo se as pessoas inseridas nele não se sentem confortáveis, inclusas e parte dele, pois nesse caso elas estarão somente cumprindo as regras e deveres, e não realmente entendendo e apoiando os benefícios do processo.
{: style="text-align: justify;"}

Quando os membros da equipe entendem o propósito do processo e passam a incentivá-lo e até mesmo melhorá-lo isso significa que a equipe começa a enxergar valor na metodologia usada, e partir daí se instaura um sentimento de cultura, que é algo praticado de dentro para fora, e não imposto, e assim todos passar a colaborar e se sentir parte do todo.
{: style="text-align: justify;"}

E eu acredito que em toda equipe esse deve ser o objetivo do processo, de criar uma cultura de trabalho em que todos se sintam confortáveis para desempenhar seu papel de forma clara, mantendo sempre a produtividade e qualidade que todo projeto merece.
{: style="text-align: justify;"}

Para quem acha que é impossível conseguir atingir esse estágio de maturidade em uma equipe e processo, eu indico aqui assistir o vídeo onde o [Spotify](https://www.spotify.com/){:target="_blank"} fala sobre sua engenharia de desenvolvimento, mostrando como eles evoluíram seu processo de software de equipes usando basicamente o Scrum para um processo que se tornou uma cultura dentro da empresa. O vídeo legendado pode ser visto [aqui](https://youtu.be/NoGhC1LaaAE){:target="_blank"}.
{: style="text-align: justify;"}

E é isso pessoal, apesar de eu estar longe de ser um especialista no assunto, com um pouco da minha experiência espero ter ajudado quem esteja procurando implementar um processo de software na sua equipe.
{: style="text-align: justify;"}

Obrigado, e se tiver alguma dúvida ou sugestão por favor deixe nos comentários.
{: style="text-align: justify;"}

Até o próximo post.
{: style="text-align: justify;"}

*As imagens foram extraídas da [wikipedia](https://en.wikipedia.org/){:target="_blank"}.*

