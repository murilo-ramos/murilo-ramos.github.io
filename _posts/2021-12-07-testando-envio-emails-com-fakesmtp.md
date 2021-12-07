---
title: Testando envio de e-mails com Fake SMTP
author: Murilo Costa
date: 2021-12-07 17:20:00 -0300
categories: [Blogging]
tags: [e-mail, email, smtp, fakesmtp, java]
---

Como eu já mencionei anteriormente em alguns posts, a maioria dos softwares que desenvolvemos sempre possui algum tipo de dependência de outros serviços e mecanismos, onde precisamos realizar integrações para que o software faça um trabalho por completo, e geralmente uma coisa muito utilizada nos softwares são mecanismos de notificação de usuários, onde o e-mail é um dos grandes utilizados.
{: style="text-align: justify;"}

Por isso é normal que precisemos desenvolver mecanismos de envio de e-mail, o que com bibliotecas de terceiros se torna uma tarefa relativamente fácil, porém a parte muitas vezes complicada é como testar o envio de e-mails, pois para esse tipo de teste precisamos de um serviço de e-mail [SMTP](https://br.godaddy.com/blog/smtp-explicado-o-que-e-sua-importancia-e-para-que-serve/){:target="_blank"} (protocolo utilizado no envio de e-mails) no qual nosso software possa se conectar. Apesar de podermos tratar essa dependência como algo abstrato, ajudando inclusive nos testes unitários, não tem como fugir de um teste real de envio de e-mail, verificando se informações como remetente, destinatário, assunto, mensagem formatada e anexos estão sendo enviados corretamente.
{: style="text-align: justify;"}

E para se realizar esse tipo de teste não existe outra maneira a não ser realmente ter um serviço SMTP ativo que possa ser usado, o que nem sempre é a realidade do nosso ambiente de desenvolvimento.
{: style="text-align: justify;"}

Mas para resolver essa situação eu descobri uma ferramenta muito interessante que é o Fake SMTP, que tem me ajudado muito nesse contexto.
{: style="text-align: justify;"}

## Fake SMTP

O [Fake SMTP](http://nilhcem.com/FakeSMTP/){:target="_blank"} é uma ferramenta feita em Java que simula um serviço SMTP, que pode ser iniciada localmente e atua como um serviço local, podendo ser acessado a partir de seu software e exibe os e-mails que foram enviados junto com suas informações.
{: style="text-align: justify;"}

![fakesmtp](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp.png)

Nele podemos ver todas as informações como remetente, destinatários, anexos, mensagem seja formatada ou não, CC e CCO, tudo de forma muito fácil, e os e-mails enviados ainda ficam armazenados no formato [EML](https://fileinfo.com/extension/eml){:target="_blank"} no seu computador, podendo ser consultados posteriormente.
{: style="text-align: justify;"}

Bacana, não é mesmo?
{: style="text-align: justify;"}

E a fim de exemplificar o uso dessa ferramenta vamos então construir um software simples para envio de e-mails, que irá utilizar o Fake SMTP como serviço SMTP.
{: style="text-align: justify;"}

## Enviando e-mail com Java

Para essa demonstração vamos fazer um software Java usando o famoso [SpringBoot](https://spring.io/projects/spring-boot){:target="_blank"}, que permite fazer uma webapp com apenas alguns passos.
{: style="text-align: justify;"}

Como a ideia aqui é mostrar o Fake SMTP em funcionamento, eu não vou entrar muito a fundo na aplicação e vou focar somente na implementação do envio de e-mail, mas caso você queira ver o código completo vou deixar o link no final do post como sempre.
{: style="text-align: justify;"}

A aplicação será basicamente uma página web contendo as informações 'De', 'Para', 'Assunto' e a mensagem em si que será enviada no e-mail, e ao clicar em enviar será disparado um e-mail com essas informações.
{: style="text-align: justify;"}

![fluxo](/assets/img/posts/testando-envio-emails-com-fakesmtp/fluxo.png)

O envio de e-mail pelo software será feito usando a lib [commons-email](https://commons.apache.org/proper/commons-email/){:target="_blank"} da [Apache](https://www.apache.org/){:target="_blank"}, e a parte principal pode ser vista no código da classe `ApacheCommonsEmailService` abaixo, que implementa a interface `EmailService`.
{: style="text-align: justify;"}

```java
@Service
public class ApacheCommonsEmailService implements EmailService {
	
	private static final String HOST = "localhost";
	private static final int PORT = 25;
	
	@Override
	public void send(EmailData emailData) {
		var email = new SimpleEmail();
		email.setHostName(HOST);
		email.setSmtpPort(PORT);
		
		try {
			email.setFrom(emailData.getFrom());
			email.setSubject(emailData.getSubject());
			email.setMsg(emailData.getMessage());
			
			email.addTo(emailData.getTo());
			
			email.send();
		} catch (EmailException ex) {
			throw new EmailServiceException(ex.getMessage());
		}
	}
}
```

Super simples não é mesmo? Nessa implementação utilizamos a classe `SimpleEmail` presente na `commons-email`, definimos os parâmetros de conexão que no nosso caso são somente o host e porta e setamos as informações do e-mail, que são o remetente, assunto, mensagem e destinatários, e por último usamos o método `send()` para enviar o e-mail. A classe `EmailData` contém somente as informações que serão inseridas pelo usuário.
{: style="text-align: justify;"}

Como nesse caso estamos utilizando o Fake SMTP que irá rodar em nossa máquina local, estamos colocando o host como `localhost` e porta como `25`, que é a padrão no Fake SMTP, mas pode ser alterada caso seja necessário.
{: style="text-align: justify;"}

Por este código ser somente um exemplo eu deixei o host e porta como constantes no código, mas em um caso de software real esse tipo de configuração deve ser colocado em um arquivo de configuração. Também a fim de somente exemplificar estou usando aqui a forma mais simples de enviar e-mails com a lib da Apache, mas existem outras implementações como a `HtmlEmail` que permite mandar e-mails formatados, e é possível também enviar anexos e etc.
{: style="text-align: justify;"}

**Com nosso código pronto vamos então ao teste!**
{: style="text-align: justify;"}

Primeiro vamos iniciar o Fake SMTP, abrindo a aplicação e clicando em `Start server`. Esse software não precisa ser instalado na máquina, bastando somente ter o Java instalado em sua máquina para utilizá-lo. O único requisito adicional para utilizá-lo é executá-lo a partir de um diretório que possua permissão de escrita, pois ele irá criar uma pasta `received-emails` contendo os e-mails enviados.
{: style="text-align: justify;"}

![fakesmtp-folder](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp-folder.png)

![fakesmtp-started](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp-started.png)

O software pode ser baixado [aqui](http://nilhcem.com/FakeSMTP/download.html){:target="_blank"}.

Em seguida podemos iniciar nossa aplicação SpringBoot para vermos a tela de envio de e-mail ao acessar `http://localhost:8080`.
{: style="text-align: justify;"}

![tela-inicial](/assets/img/posts/testando-envio-emails-com-fakesmtp/tela-inicial.png)

Ao inserirmos as informações e clicar em `Enviar` teremos a mensagem `E-mail enviado com sucesso!`.
{: style="text-align: justify;"}

![tela-email-enviado](/assets/img/posts/testando-envio-emails-com-fakesmtp/tela-email-enviado.png)

E podemos ver agora na tela do Fake SMTP que um e-mail foi enviado.
{: style="text-align: justify;"}

![fakesmtp-email-enviado](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp-email-enviado.png)

Com dois cliques no registro podemos ver toda a mensagem (é preciso ter um software que lê arquivos `.eml` instalado na máquina, como por exemplo o Microsoft Outlook).
{: style="text-align: justify;"}

![fakesmtp-mensagem](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp-mensagem.png)

E assim podemos realizar quantos testes de envio de e-mails forem necessários, lembrando que os e-mails ficarão salvos na pasta `received-emails` (esse nome pode ser configurado na tela do Fake SMTP).
{: style="text-align: justify;"}

![fakesmtp-received-emails](/assets/img/posts/testando-envio-emails-com-fakesmtp/fakesmtp-received-emails.png)

E é isso aí. Caso tenha interesse em ver o código completo da aplicação ele pode ser visto no [github](https://github.com/murilo-ramos/murilo-tech/tree/master/fakesmtp){:target="_blank"}.
{: style="text-align: justify;"}

Até a próxima pessoal.
{: style="text-align: justify;"}

