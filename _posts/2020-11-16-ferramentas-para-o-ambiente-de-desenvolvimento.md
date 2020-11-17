---
title: Ferramentas para o ambiente de desenvolvimento
author: Murilo Costa
date: 2020-11-16 17:30:00 -0300
categories: [Blogging]
tags: [desenvolvimento, ambiente, ferramentas, github, trello, jenkins, wiki]
---

Em algumas entrevistas de emprego que já fiz por aí apareceu por diversas vezes uma pergunta que acho bem interessante: Se você fosse o dono da empresa e precisasse montar o time de desenvolvimento de software, o que você faria?
{: style="text-align: justify;"}

Eu sei que esse tipo de pergunta tem mais o intuito de medir o nível de autonomia do candidato, ver quais seriam suas ações como líder e principalmente medir o quanto ele está disposto a 'abraçar e tomar conta do produto' daquela empresa, porém esse tipo de pergunta me faz pensar também sobre o que seria o mínimo necessário para se ter um ambiente de desenvolvimento de software, que na minha ótica é formado por pessoas, processos e ferramentas.
{: style="text-align: justify;"}

E por isso resolvi fazer esse post para falar um pouco sobre quais tipo de ferramentas eu considero essenciais para se ter um ambiente de desenvolvimento de software completo, que podem ajudar em todo seu ciclo de vida, desde análise até implantação.
{: style="text-align: justify;"}

## **Gerenciamento de código**

Ter uma ferramenta para gerenciar o código é, na minha opinião, a coisa mais importante que existe em um ambiente de desenvolvimento, pois é nela que você irá guardar o código-fonte de tudo que é produzido por você ou sua equipe, e perder esse código pode muitas significar o fim do seu produto, do seu sustento ou até mesmo pode te fazer encarar uma ação judicial por não poder mais prover o suporte acordado em contrato com seu cliente.
{: style="text-align: justify;"}

Pode parecer que estou pegando um pouco pesado, mas acredite em mim, não estou. Infelizmente muitas pessoas e empresas só conseguem enxergar o valor no produto, e não se dão conta que sem o código-fonte não existe o produto, e é por isso que devemos nos preocupar e muito com ele, tanto que existem diversas [normas nacionais](https://softex.br/mpsbr/){:target="_blank"} e [internacionais](https://pt.wikipedia.org/wiki/CMMI){:target="_blank"} que podem ser seguidas para se ter um bom ambiente de desenvolvimento.
{: style="text-align: justify;"}

Algumas pessoas pensam também que em alguns casos não é preciso ter realmente uma ferramenta para gerenciar o código, pois pode-se usar a técnica 'controle de versão em arquivos zip' que resolve o problema, porém precisamos lembrar que estamos em 2020 ([ano apocalíptico](https://pt.wikipedia.org/wiki/Pandemia_de_COVID-19){:target="_blank"} em que escrevo esse post) e existem formas muito melhores de fazer esse gerenciamento usando ferramentas mais modernas, como é o caso do [Git](https://git-scm.com/){:target="_blank"}.
{: style="text-align: justify;"}

As vantagens que são obtidas com essas ferramentas são:
{: style="text-align: justify;"}

 - Facilidade na visualização de alterações no código e seus responsáveis;
 - Facilidade para integrar códigos que são desenvolvidos por diferentes desenvolvedores (merge);
 - Facilidade na visualização do código de aplicações, uma vez que essas ferramentas permitem ver o código direto na ferramenta, seja em um aplicativo ou em formato web no navegador;
 - Backup do código em um servidor confiável, e não na máquina do desenvolvedor.

Existem diversas ferramentas que fazem esse trabalho, como o [SVN](https://subversion.apache.org/){:target="_blank"} e [Mercurial](https://www.mercurial-scm.org/){:target="_blank"}, porém a mais utilizada hoje é o [Git](https://git-scm.com/){:target="_blank"}, que podem ser utilizado com diversos serviços, seja na internet com o [GitHub](https://github.com/){:target="_blank"}, ou instalado dentro de seu ambiente com o [GitLab](https://about.gitlab.com/){:target="_blank"} ou [BitBucket](https://bitbucket.org/){:target="_blank"}.
{: style="text-align: justify;"}

Na minha opinião a maior vantagem que o [Git](https://git-scm.com/){:target="_blank"} tem em relação a outras ferramentas é seu modo 'descentralizado' de trabalhar, onde o seu código não precisa ficar 100% do tempo atrelado ao servidor, funcionando de forma assíncrona, o que te da vantagens como ter todo o histórico do seu repositório localmente (clone) e poder trabalhar de forma offline, somente fazendo a sincronização quando for preciso (apesar de ser altamente recomendável não ficar muito tempo sem sincronizar).
{: style="text-align: justify;"}

Atualmente o serviço de [Git](https://git-scm.com/){:target="_blank"} mais usado no mundo é o [GitHub](https://github.com/){:target="_blank"}, que fica hospedado na internet e possibilita a criação de repositório públicos e privados, possuindo também um plano pago para empresas que precisam de mais recursos. O [GitHub](https://github.com/){:target="_blank"} possui uma interface muito boa que pode ser acessada direto no navegador, facilitando bastante a vida do desenvolvedor além de possuir várias outras features indispensáveis como mecanismos de merge de código (pull request) e mecanismos adicionais, como as [Issues](https://guides.github.com/features/issues/){:target="_blank"} que permitem gerenciar tarefas relacionadas ao código dentro da própria ferramenta.
{: style="text-align: justify;"}

Se você procura algo para hospedar internamente em um servidor local, ai acredito que o [GitLab CE](https://about.gitlab.com/install/ce-or-ee/){:target="_blank"} pode ser a melhor opção, ou então o [BitBucket](https://bitbucket.org/){:target="_blank"} que faz parte do pacote [Atlassian](http://www.atlassian.com/){:target="_blank"}, caso sua empresa o possua.
{: style="text-align: justify;"}

## **Gerenciamento de tarefas e projeto**

Depois do código, outro item que considero muito importante no desenvolvimento de software é o controle de tarefas, e consequentemente do projeto que está sendo desenvolvido, pois se não existir uma organização dessas tarefas o desenvolvimento pode ser tornar meio caótico.
{: style="text-align: justify;"}

O gerenciamento de tarefas ajuda não só a determinar o tempo de trabalho da tarefa, mas também ajuda em saber quem está responsável pela mesma, qual o estado atual dela, se ela está bloqueando outras tarefas e etc, informações que são extremamente importantes para quem está gerenciando um projeto ou até mesmo tarefas individuais, pois assim pode-se estipular prazos, balancear carga e agir em casos de gargalos.
{: style="text-align: justify;"}

Um outro ponto é o fato é que sempre que um desenvolvimento é planejado, é difícil conseguirmos pensar nos detalhes de cada item que vai ser desenvolvido, onde geralmente pensamos no item A, B e C e ja partimos para o código, porém ao passo que ele vai sendo desenvolvido vão aparecendo detalhes que muitas vezes envolvem regras de negócio, onde decisões terão que ser tomadas e registradas em algum lugar, e é ai que as ferramentas ajudam, pois pode-se colocar todas essas informações na tarefa em questão, ou então criar uma outra tarefa contendo essas informações e então tudo fica registrado para posterior consulta.
{: style="text-align: justify;"}

A forma de se fazer este tipo de controle não precisa ser por meio de ferramentas do tipo software, pois é perfeitamente possível utilizar métodos visuais como quadros com post-its para distribuição de tarefas e acompanhamento (famoso quadro kanban) e isso funciona muito bem, porém o fato é que alguma hora isso vai precisar ficar catalogado em algum lugar, e eu acredito que simplesmente guardar fotos do quadro ou ter aquele monte de arquivos Word com as tarefas não é o melhor método.
{: style="text-align: justify;"}

E quais ferramentas podemos usar? A lista aqui é enorme: [JIRA](https://www.atlassian.com/br/software/jira){:target="_blank"}, [Trello](https://trello.com/){:target="_blank"}, [Microsoft Project](https://www.microsoft.com/pt-br/microsoft-365/project/project-management){:target="_blank"}, [Wekan](https://wekan.github.io/){:target="_blank"}, [Redmine](https://www.redmine.org/){:target="_blank"} e vários outros, por isso vou falar somente de três, que acredito serem ótimas opções.
{: style="text-align: justify;"}

### Trello

O [Trello](https://trello.com/){:target="_blank"} é o mais simples deles, podendo ser acessado diretamente da internet, permite criação de quadros e painéis personalizados e várias funcionalidades de apoio visual. A única desvantagem é que não permite uma instalação local (a não ser que você utilize o pacote completo da [Atlassian](http://www.atlassian.com/){:target="_blank"}, que vem com o [Trello](https://trello.com/){:target="_blank"} embutido agora que ele foi comprado por esta empresa).
{: style="text-align: justify;"}

### Wekan

Basicamente uma cópia do Trello, porém open-source. A maior vantagem do [Wekan](https://wekan.github.io/){:target="_blank"} é que justamente por ser open-source ele pode ser instalado localmente no seu ambiente de desenvolvimento. Já no ponto negativo está a sua interface, que não é tão amigável quanto a do seu primo rico.
{: style="text-align: justify;"}

### JIRA

Atualmente acredito que o [JIRA](https://www.atlassian.com/br/software/jira){:target="_blank"} deve ser a ferramenta de gestão de tarefas e projetos mais utilizada no mundo por desenvolvedores, e isso se deve ao fato de ser uma ferramenta que se integra muito bem dentro de seu pacote da [Atlassian](http://www.atlassian.com/){:target="_blank"}, que provê ferramentas para todo o ciclo de vida do software. O [JIRA](https://www.atlassian.com/br/software/jira){:target="_blank"} possui muitas funcionalidades bem legais como controle de workflow, possibilidade de organizar projetos de acordo com a metodologia usada, kanbans personalizados e várias outras coisas ausentes nos outros softwares, o que o torna uma escolha muito interessante. Mas é claro que tudo isso não vem de graça, pois o [JIRA](https://www.atlassian.com/br/software/jira){:target="_blank"} é um software pago e não muito barato.
{: style="text-align: justify;"}

Para sumarizar, se você tem uma equipe pequena ou média acredito que o [Trello](https://trello.com/){:target="_blank"} ou [Wekan](https://wekan.github.io/){:target="_blank"} já são o suficiente para suas necessidades, mas se estamos falando aqui de uma empresa grande ou projetos enormes ai é o [JIRA](https://www.atlassian.com/br/software/jira){:target="_blank"} quem vai se encaixar melhor.
{: style="text-align: justify;"}

## **Automação de tarefas**

Apesar de muitas equipes de desenvolvimento não considerarem esse tipo de ferramenta como essencial, eu acredito que ferramentas de automação de tarefas são indispensáveis no dia a dia de desenvolvedores, pois além de automatizar processos que geralmente são bem trabalhosos, ainda criam uma padronização dos projetos, removendo responsabilidades que oneram o trabalho do desenvolvedor (famosa convenção sobre configuração).
{: style="text-align: justify;"}

Essas ferramentas podem ser utilizadas para vários fins, como build automático da aplicação para distribuição, execução de testes, implantação, build personalizado, cópia de arquivos e várias outras tarefas que se realizadas manualmente acabam consumindo bastante tempo, além de estarem sujeitas a erro humano.
{: style="text-align: justify;"}

Vou colocar aqui os três tipos de automações que mais acho importantes em um ambiente de desenvolvimento.
{: style="text-align: justify;"}

### Build automático

Após a conclusão de um desenvolvimento é preciso sempre gerar o pacote que será distribuído, seja para o cliente implantar em seu ambiente ou para implantação interna, e a geração desse pacote (processo geralmente chamado de build) pode ser bem trabalhoso, por isso podemos automatizar essas tarefas criando scripts de processamento que já criam o pacote pronto para distribuição, economizando o tempo do desenvolvedor. A maioria das linguagens e plataformas de desenvolvimento já suportam esse tipo de build, salvo em casos muito específicos onde a geração do pacote fica ligada interface de desenvolvimento (IDE), como por exemplo no [Delphi](https://www.embarcadero.com/br/products/delphi){:target="_blank"} e [Xcode](https://developer.apple.com/xcode/){:target="_blank"} (mas existem alternativas).
{: style="text-align: justify;"}

### CI

O termo CI vem de Continuous Integration que traduzindo significa integração contínua, termo usado para processos que permitem um desenvolvimento seguro baseado em testes. Os processos de CI criam um fluxo automático de testes também automáticos no seu software durante e após o build, permitindo uma validação completa do software a cada desenvolvimento realizado, garantindo qualidade e rapidez.
{: style="text-align: justify;"}

### CD

Já o termo CD significa Continuous Deployment ou implantação contínua, sendo responsável por processos que visam disponibilizar o pacote de software automaticamente. Este tipo de processo fica responsável por realizar de forma automática a distribuição do pacote, enviando para o cliente ou já implantando em produção. Ele é fim do ciclo automatizado de um software, onde primeiro é realizado o build automático contendo testes de CI durante e após o build, e ocorrendo tudo com sucesso o processo de CD já disponibiliza o pacote em produção, sem precisar de nenhuma interferência manual.
{: style="text-align: justify;"}

Assim como nos outros itens, existem diversas ferramentas para se realizar este tipo de trabalho, porém a que se destaca aqui é o [Jenkins](https://www.jenkins.io/){:target="_blank"}, a mais famosa e usada por ser compatível com diversas linguagens e plataformas atuais, e também por possuir um mecanismo baseado em plugins que expande em muito suas possibilidades.
{: style="text-align: justify;"}

## **Gestão do conhecimento**

E para finalizar com o último assunto, porém não menos importante estão as plataformas de gestão do conhecimento, ou como geralmente chamamos as 'wikies'.
{: style="text-align: justify;"}

Todo mundo que já participou de um desenvolvimento de software sabe que sempre existem documentos de análise, especificação, arquitetura e também de design, que definem como as funcionalidades, telas e processos devem ocorrer dentro do software, por isso devem ser bem escritos e de fácil acesso.
{: style="text-align: justify;"}

Porém infelizmente nem sempre é isso que ocorre, essa documentação em muitos casos não é controlada, onde acaba ficando espalhada em documentos de texto, imagens e apresentações em diversos locais do ambiente, como pastas de redes, repositórios de arquivos e muitas vezes nas próprias máquinas das pessoas.
{: style="text-align: justify;"}

E é aqui que uma ferramenta para gerenciar o conhecimento faz a diferença, pois com ela pode-se organizar todas essas informações de forma fácil e eficiente, inclusive com mecanismos de busca que facilitam a consulta de tudo que está sendo desenvolvido, ou já foi feito a algum tempo. E não só informações de projeto podem ser guardadas nessas ferramentas, mas qualquer outro tipo de informação, como tutorias internos, exemplos de código, procedimentos e qualquer outra coisa que ajude a equipe em suas tarefas.
{: style="text-align: justify;"}

Até hoje foram somente duas ferramentas desse tipo que usei, que são o [Confluence](https://www.atlassian.com/br/software/confluence){:target="_blank"} e o [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki){:target="_blank"}. As duas cumprem muito bem o seu papel e possuem diversas funcionalidades para ajudar na gestão do conhecimento, como busca, criação de tópicos e etc. A maior diferença entre elas é que o [Confluence](https://www.atlassian.com/br/software/confluence){:target="_blank"} é pago e o [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki) não, porém a primeira possui uma interface mais amigável ao criar conteúdos enquanto que a segunda é um pouco mais trabalhosa, mas no final são duas grandes ferramentas.
{: style="text-align: justify;"}

E é isso pessoal, acredito que consegui falar um pouco sobre quais ferramentas eu considero essenciais para se ter um ambiente de desenvolvimento de software, seja para uma equipe com várias pessoas ou para a famosa equipe de uma pessoa só rs.
{: style="text-align: justify;"}

Lembrando que, como falei no início do post, um ambiente não é formado só por ferramentas, pois além das pessoas que são quem fazem o trabalho acontecer, temos também os processos, que espero em breve falar um pouco aqui também.
{: style="text-align: justify;"}

Obrigado e até a próxima.
{: style="text-align: justify;"}


