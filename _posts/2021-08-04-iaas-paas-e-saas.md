---
title: IaaS, PaaS e SaaS
author: Murilo Costa
date: 2021-08-04 17:00:00 -0300
categories: [Blogging, Infraestrutura]
tags: [software, deploy, implantacao, on-premises, iaas, paas, baas, saas]
---

A maioria das pessoas que trabalham com tecnologia da informação já devem ter visto siglas e termos como SaaS, IaaS e On-Premises por aí, e com certeza ficaram com dúvida do que se tratava e tiveram que pesquisar a respeito para entender o que são. Mas se você ainda não sabe ou não pesquisou, não se preocupe, pois, nesse post vou explicar um pouco sobre isso.
{: style="text-align: justify;"}

Essas siglas e termos estão relacionadas a hospedagem de uma aplicação, que é onde o software irá ser implantado. Qualquer software que seja desenvolvido irá precisar mais cedo ou mais tarde de uma implantação, que basicamente é colocar o software para ser executado em algum ambiente e então poder ser utilizado pelos usuários, um processo chamado coloquialmente de *ir para produção*.
{: style="text-align: justify;"}

Para fazer essa implantação precisamos sempre verificar qual a melhor forma de fazê-lo, pois cada software precisa de uma implantação diferente e as opções de hospedagem existentes hoje são bem variadas.
{: style="text-align: justify;"}

Por isso neste post irei falar um pouco sobre modos de implantação e hospedagem, falando um pouco sobre o modelo tradicional baseado em servidores físicos e/ou locais e indo até as propostas mais atuais baseadas em nuvem.
{: style="text-align: justify;"}

Para auxiliar nas explicações vou utilizar um case de uma empresa do tipo loja de departamento que quer colocar um sistema para ser utilizado em suas lojas espalhadas pelo país. Visando alcançar esse objetivo, a loja adquiriu um sistema web feito com as tecnologias MySQL para banco de dados, Java para backend e Angular para frontend e agora precisa de uma hospedagem para implantar o software.
{: style="text-align: justify;"}

## Modelo de hospedagem tradicional On-Premises

O modelo de hospedagem tradicional é um modelo onde a empresa acaba ficando responsável por manter toda a infraestrutura de hospedagem do sistema, geralmente cuidando não somente da aplicação em si mas também dos servidores, sistemas operacionais e em muitos casos até mesmo das máquinas físicas, que podem ficar em um ambiente da própria empresa e conectadas em uma intranet para todos utilizarem o sistema, ou podem ser máquinas alugadas na internet, mas que vão ficar sob administração da empresa que contratou o aluguel.
{: style="text-align: justify;"}

A imagem a seguir mostra como seria o cenário de hospedagem desse sistema.
{: style="text-align: justify;"}

![on-premises](/assets/img/posts/iaas-paas-e-saas/on-premises.jpeg)

Repare que nesse modelo a empresa seria responsável por administrar as máquinas, instalar o sistema operacional, subir a plataforma Java nas máquinas, instalar dependências, servidores de aplicação, servidores web, banco de dados, configurações de rede e por último a aplicação em si.
{: style="text-align: justify;"}

Até algum tempo atrás esse era o único modelo disponível para hospedagem de sistemas baseados em web e por isso ele é chamado de tradicional, porém ultimamente ele passou a ser chamado de On-Premises.
{: style="text-align: justify;"}

Nesse modelo é possível observar também que o pagamento de recursos é feito por toda a infraestrutura de uma forma fixa e limitada, onde se forem necessários mais recursos, deverão ser alugadas e adquiridas novas máquinas e então feita novamente toda a configuração mencionada acima.
{: style="text-align: justify;"}

Considerando o nosso case a loja de departamentos teria que ter seu próprio datacenter com as máquinas físicas para hospedar o sistema ou então alugar máquinas de um provedor na internet, o que em ambos os casos ainda existe a necessidade de realizar toda a configuração mencionada anteriormente.
{: style="text-align: justify;"}

A hospedagem On-Premises possui o mais diverso leque de opções como o [Hostgator](https://www.hostgator.com.br/){:target="_blank"}, [Locaweb](https://www.locaweb.com.br/){:target="_blank"}, [DreamHost](https://www.dreamhost.com){:target="_blank"}, [TIVIT](https://www.tivit.com/){:target="_blank"}, dentro outros.
{: style="text-align: justify;"}

## IaaS

Com o surgimento da computação em nuvem apareceram novas formas de implantar sistemas que se adequam melhor em alguns cenários, onde surgiu o *IaaS - Infrastructure as a Service* (infraestrutura como serviço), proporcionando uma forma bem diferenciada de colocar o seu software em produção.
{: style="text-align: justify;"}

Esse modelo permite tratar toda a infraestrutura como um serviço, onde o utilizador não acessa mais máquinas físicas e sim máquinas virtuais que podem ser criadas sob demanda, e são disponibilizadas totalmente configuradas a nível de rede e sistema operacional, sendo necessário somente instalar e configurar a plataforma, dependências e aplicações.
{: style="text-align: justify;"}

Atualizando nossa imagem de demonstração podemos ver que a infraestrutura física agora está cinza, o que significa que não atuamos mais sobre ela.
{: style="text-align: justify;"}

![iaas](/assets/img/posts/iaas-paas-e-saas/iaas.jpeg)

A maior vantagem desse modelo está na facilidade de escalar seu sistema, onde a partir de máquinas virtuais pré-configuradas é possível subir quantas instâncias forem necessárias para a sua aplicação, podendo remanejar a quantidade de servidores de acordo com o uso.
{: style="text-align: justify;"}

Pensando no nosso case por exemplo, a loja de departamentos poderia utilizar um número fixo de servidores no dia-a-dia comum e utilizar servidores adicionais em dias de promoção nas lojas, o que poderá ser feito com muita facilidade, algo muito diferente do que ocorre no On-Premises.
{: style="text-align: justify;"}

Outra vantagem do modelo IaaS está relacionado também ao quanto pagamos para disponibilizar a aplicação em produção, pois nesse modelo pagamos somente pelo que é utilizado, ao contrário do modelo On-Premises que você sempre irá pagar por algo fixo.
{: style="text-align: justify;"}

Este modelo é atualmente o mais usado quando falamos de implantação de sistemas na internet, pois ele elimina necessidade de administração de servidores em um nível mais baixo e permite às empresas trabalharem em um nível mais alto e automático, sem perder o poder de configuração e personalização do ambiente, já que ainda temos que lidar com a plataforma, banco de dados e etc.
{: style="text-align: justify;"}

O serviço IaaS mais conhecido e usado atualmente é o [Amazon AWS](https://aws.amazon.com/pt/){:target="_blank"} que oferece diversos tipos de infraestrutura para as necessidades dos utilizadores, mas temos outras boas opções também como o [Microsoft Azure](https://azure.microsoft.com/pt-br/){:target="_blank"} e o [Google Cloud](https://cloud.google.com){:target="_blank"}.
{: style="text-align: justify;"}

## PaaS

O modelo PaaS vem de *Platform as a Service* (plataforma como serviço) e atua em um nível acima do IaaS, permitindo ao utilizador implantar seu sistema sem nenhuma (ou quase nenhuma) preocupação com o ambiente, bastando somente desenvolver o sistema ou comprá-lo e colocar para rodar na plataforma.
{: style="text-align: justify;"}

Esse modelo é recomendado para aplicações que não possuem muitas dependências de ambiente e conseguem funcionar de forma agnóstica, onde o importante é somente ter a aplicação em produção, sem ficar se preocupando com configurações adicionais. Nesse caso toda a plataforma será provida pelo serviço, como ambiente de execução (ex: JVM, NodeJS, .Net), banco de dados, ferramentas de envio de e-mails, rede, DNS e etc.
{: style="text-align: justify;"}

Atualizando nossa imagem de exemplo vemos que com esse modelo ficamos responsáveis somente pela aplicação.
{: style="text-align: justify;"}

![paas](/assets/img/posts/iaas-paas-e-saas/paas.jpeg)

Voltando ao nosso case, pensando nos cenários anteriores ainda temos a necessidade de uma equipe minimamente especializada em infraestrutura de T.I. para se implantar o software da loja, porém se for utilizado um PaaS essa necessidade diminui drasticamente, pois boa parte da configuração necessária da plataforma já estará disponível no serviço, sendo necessário somente a implantação da aplicação.
{: style="text-align: justify;"}

A desvantagem desse modelo está justamente na sua falta de possibilidade de customização a médio e baixo nível, que é algo que muitas aplicações necessitam. Se sua aplicação precisa de uma camada de middleware que necessita estar instalada no servidor que irá executar a aplicação, existe uma grande chance que não seja possível fazer isso com PaaS.
{: style="text-align: justify;"}

Como exemplos de PaaS temos o famoso [Heroku](https://www.heroku.com/){:target="_blank"} que é um PaaS que usa a infraestrutura da AWS e o [Google Cloud Plataform](https://cloud.google.com){:target="_blank"}.
{: style="text-align: justify;"}


## BaaS

O BaaS - *Backend as a Service* (backend como serviço) é um modelo relativamente novo que surgiu da necessidade de algumas aplicações não precisarem ter um backend complexo com muitas validações e processos customizados, visando somente a possibilidade de ações [CRUD](https://pt.wikipedia.org/wiki/CRUD){:target="_blank"} com validações e processos simplificados, podendo abstrair o backend para um serviço e focar somente no desenvolvimento de um frontend que atenda as necessidades da empresa.
{: style="text-align: justify;"}

No modelo BaaS o utilizador somente configura no serviço coisas como modelo de dados, validações e processos simplificados de autenticação, dentre outros, e então já possui um backend ativo, escalável e de fácil acesso, bastando somente ter um frontend que utilize esse backend como serviço.
{: style="text-align: justify;"}

Considerando novamente nossa imagem, podemos ver que agora ficamos responsáveis somente pelo frontend da aplicação.
{: style="text-align: justify;"}

![baas](/assets/img/posts/iaas-paas-e-saas/baas.jpeg)

Este modelo é atualmente muito utilizado por apps de smartphones que precisam de um backend para centralização e compartilhamento de dados, mas não necessitam de muita customização em seus processos, podendo utilizar um BaaS para resolver essa necessidade.
{: style="text-align: justify;"}

Em nosso case a loja de departamento poderia demandar a criação de um app para a loja usando um BaaS como backend, e então só seria necessário manter o app, sem se preocupar como toda infraestrutura de um servidor/serviço como ocorre ao se utilizar IaaS ou PaaS.
{: style="text-align: justify;"}

O BaaS mais utilizado no momento é um produto do Google chamado [Firebase](https://firebase.google.com/){:target="_blank"}, que possui um banco de dados NoSQL com opções de armazenamento realtime, opções de login, segurança e funções de automatização, e como alternativa ao serviço do Google temos o [Back4App](https://www.back4app.com){:target="_blank"}, [AWS Amplify](https://aws.amazon.com/pt/amplify/){:target="_blank"} e outros. 
{: style="text-align: justify;"}

## SaaS

E por último existe o SaaS - *Software as a Service* (software como serviço) que já provê a aplicação para uso sem precisar instalar, configurar ou gerenciar a hospedagem do software, basta somente acessar e utilizar. O SaaS é indicado quando o utilizador não tem nenhuma pretensão de manter um ambiente técnico para suportar um sistema e quer somente fazer uso de uma ferramenta, podendo assim assinar um serviço e utilizar da forma que preferir.
{: style="text-align: justify;"}

Analisando agora uma última vez nossa imagem podemos ver que não temos mais nenhuma responsabilidade de suporte a aplicação, não controlamos mais servidores, nem plataforma e nem mesmo a aplicação, e sim somente fazemos uso do que está disponível.
{: style="text-align: justify;"}

![saas](/assets/img/posts/iaas-paas-e-saas/saas.jpeg)

O modelo SaaS como podemos ver é o mais indicado para quem quer somente utilizar uma aplicação que atenda sua necessidade de negócio, sem precisar se preocupar com nenhuma questão técnica, e logicamente assim como os outros modelos *as a service* o pagamento é feito de acordo com o uso.
{: style="text-align: justify;"}

A maior desvantagem desse modelo está no caso de restrições que um utilizador pode ter, como segurança de dados, necessidade de armazenamento interno ou integrações customizadas, o que pode ser inviável no modelo SaaS visto que o acesso ao software se dá diretamente pela internet e a customização não é interna, e sim feita pela empresa ofertante.
{: style="text-align: justify;"}

E para finalizar o nosso case, para reduzir gastos a loja de departamento poderia se desfazer de qualquer infraestrutura e suporte a sua aplicação interna, mesmo que hospedada em um IaaS ou PaaS, e contrataria um SaaS que disponibiliza uma aplicação para gerenciamento de loja, podendo ser acessada pela internet de qualquer uma das lojas.
{: style="text-align: justify;"}

Os exemplos de SaaS são os mais variados possíveis pois qualquer empresa que ofereça um software que disponibiliza um serviço pode ser considerado um SaaS, então temos exemplos como [Github](https://github.com){:target="_blank"} que ofereço um serviço de repositório Git, temos o [Trello](https://www.trello.com){:target="_blank"} que oferece um serviço de organização de atividades e quadro Kanban, temos o [Spotify](https://www.spotify.com/){:target="_blank"} que oferece um serviço de música, temos a [Netflix](https://www.netflix.com/){:target="_blank"} que oferece um serviço de filmes e séries, etc.
{: style="text-align: justify;"}

E é isso pessoal, esses são os modelos mais atuais de implantação e hospedagem de sistemas.
{: style="text-align: justify;"}

Por hoje é só, até a próxima.
{: style="text-align: justify;"}

*Os exemplos de serviços foram citados observando suas especialidades, porém é preciso dizer que eles não se restringem a somente ao que foi informado no post.*