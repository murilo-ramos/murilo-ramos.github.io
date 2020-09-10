---
title: Chrome Web Server
author: Murilo Costa
date: 2018-07-04 18:47:00 -0300
categories: [Blogging, Frontend, Javascript]
tags: [chrome-web-server, http, http-server, javascript, json]
---

Acredito que a maioria das pessoas atualmente usa o Google Chrome como navegador padrão no dia-a-dia, e muitas dessas pessoas são desenvolvedores que, obviamente, usam o Chrome para testar suas aplicações, visto que ele é um dos browsers mais usados e oferece ferramentas muito boas para dev, como é o caso do Developer Tools, um grande amigo na hora do debug.
Mas o que talvez muitos não sabem é que o próprio Google disponibiliza uma outra ferramenta em forma de App para o browser que pode ajudar muito os desenvolvedores, principalmente front-ends: O Chrome Web Server !!
Este App é um servidor http bem light em que você rodar seus arquivos estáticos em uma porta http, facilitando testes como consumo de dados e uso de urls, e que roda diretamente a partir do seu browser.
{: style="text-align: justify;"}

Quer ver um exemplo?
{: style="text-align: justify;"}

Imagine que você está fazendo uma pagina que vai consumir dados JSON de um serviço http e mostrar na tela, e você ficou responsável de fazer o front-end. Para simular o comportamento do serviço, você vai simplesmente colocar um arquivo JSON na raiz do seu projeto e seu código irá consumir esse arquivo como se fosse um serviço http.
{: style="text-align: justify;"}

fruits.json

```javascript
[{
 "name": "Pera",
 "count": 2
}, {
 "name": "Uva",
 "count": 3
}, {
 "name": "Maça",
 "count": 10
}, {
 "name": "Salada Mista",
 "count": 1
}]
```

index.html

```html
<!DOCTYPE html>
<html>
<head>
 <title>Frutas</title>
</head>
<body>
 <div id="fruits"></div>

 <script type="text/javascript">
  var xhttp = new XMLHttpRequest();

  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
     let fruits = JSON.parse(this.responseText);
     let text = "";

     fruits.forEach(function(item) {
      text += "<p>"
      text += 'Fruta: ' + item.name +', Quantidade: ' + item.count;
      text += "</p>"
     })

     document.getElementById("fruits").innerHTML = text;
    }
  };
  
  xhttp.open("GET", "fruits.json", true);
  xhttp.send();
 </script>
</body>
</html>
```

Agora, ao tentar abrir o arquivo index.html no browser você irá perceber que nada foi carregado, e ao abrir o developer tools temos:
{: style="text-align: justify;"}

![error](/assets/img/posts/chrome-web-server/error.png)

Isso acontece pois o navegador não deixa que requests http sejam feitas para protocolos que não sejam os listados na imagem, o que inviabiliza o desenvolvimento visto que acessando desse modo o protocolo tentado foi o file://.
{: style="text-align: justify;"}

Para resolver isso, basta instalarmos o app do WebServer.

![web-server-install](/assets/img/posts/chrome-web-server/web-server-install.png)

Iniciá-lo a partir dos apps do Chrome.

![apps](/assets/img/posts/chrome-web-server/apps.png)

E carregar a pasta em que está o nosso projeto.

![web-folder](/assets/img/posts/chrome-web-server/web-server.png)

![load-folder](/assets/img/posts/chrome-web-server/load-folder.png)

Assim o app vai subir um servidor http em http://127.0.0.1:8887/ e basta acessar este endereço para ver sua aplicação funcionando corretamente:
{: style="text-align: justify;"}

![success](/assets/img/posts/chrome-web-server/success.png)

Este é um exemplo bem básico do uso desta app, mas quem está acostumado com desenvolvimento front-end já pode ver que ela é uma mão na roda para desenvolvimento rápido, sem precisarmos de um servidor http completo ou até mesmo de um servidor de aplicação para execução de nosso código.
{: style="text-align: justify;"}

Para baixar o Chrome Web Server é só acessar [AQUI](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb){:target="_blank"} com o Chrome.

O código de exemplo pode ser encontrado no [GitHub](https://github.com/murilo-ramos/murilo-tech/tree/master/fruits-web-server){:target="_blank"}.