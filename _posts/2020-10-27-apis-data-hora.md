---
title: APIs de Data e Hora
author: Murilo Costa
date: 2020-10-26 07:00:00 -0300
categories: [Blogging]
tags: [java, javascript, go, golang, programacao, date, time, api]
---

Algo que sempre precisa ser manipulado e frequentemente gera dúvidas em sites como o [StackOverflow](https://pt.stackoverflow.com/){:target="_blank"} é a manipulação de datas pelas linguagens de programação, pois existem muitos processos que são realizados baseados em datas, como relatórios por período, tempo de disponibilidade de informações, timeouts e etc.
{: style="text-align: justify;"}

E uma coisa que eu percebi é que não parece existir uma padrão entre como tratar de datas, onde cada linguagem parece seguir um caminho diferente, impondo ao desenvolvedor a tarefa de sempre consultar as documentações para não assumir que algo funcione de um jeito, mas acabe sendo de outro, principalmente pelo fato de que algumas linguagens tem APIs bem diferentes, e algumas vezes até com coisas que não fazem muito sentido.
{: style="text-align: justify;"}

E para trazer alguns exemplos resolvi fazer esse post mostrando alguns pontos intrigantes (pra não dizer bizarros rs) das APIs de data que já trabalhei.
{: style="text-align: justify;"}

## Java

Com o lançamento do `Java 8` em 2014 eu preciso dizer que a manipulação de datas em Java foi em [muito melhorada](https://www.devmedia.com.br/java-8-e-sua-nova-api-para-datas/31462){:target="_blank"}, porém antes disso algumas coisas não faziam muito sentido em minha opinião.
{: style="text-align: justify;"}

Começando pela classe `Date` que é utilizada para representar uma data no sistema, a primeira coisa que podemos ver é que semanticamente ela já causa um pouco de confusão pois ela não armazena somente a data no sentido de Dia, Mês e Ano, armazenando também o horário.
{: style="text-align: justify;"}

Isso pode até parecer bobeira, mas é algo que pode causar confusão. Vamos ver o exemplo abaixo considerando que hoje seja 21/10/2020.
{: style="text-align: justify;"}

```java
Calendar calendar = Calendar.getInstance();
calendar.set(2020, 9, 21);
Date date = calendar.getTime();

Date currentDate = new Date();

boolean areEquals = currentDate.equals(date); // falso

```

Aqui vemos que estamos criando duas instâncias de `Date` que teoricamente seriam equivalentes, pois a primeira está sendo construída com a data de `21/10/2020` e a segunda é criada com base na data atual, que também seria `21/10/2020`, então são iguais. Porém se esse código for executado voê verá que o atributo `areEquals` será `false`, pois o que ocorre é que o `date` não foi criado especificando o horário, então ele acaba ficando com o horário do momento enquanto o `currentDate` será criado com o horário do momento seguinte em que foi executado, gerando a divergência mesmo que por milisegundos.
{: style="text-align: justify;"}

A forma que outras linguagens usaram pra resolver esse problema diverge, como o `C#` em que a classe se chama [DateTime](https://docs.microsoft.com/pt-br/dotnet/api/system.datetime?view=netcore-3.1){:target="_blank"} para essa representação, e o `Perl` que usa uma função [localtime](https://perldoc.perl.org/functions/localtime){:target="_blank"} representando o 'tempo' no sentido de tempo universal, de onde podem ser obtidas as informações necessárias como data atual, horario, timezone, etc.
{: style="text-align: justify;"}

Outra coisa em `Java` que é esquisito é sobre a classe `Calendar` usada no último código, que para se obter uma instância devemos utilizar o método `getInstance()`.
{: style="text-align: justify;"}

Todos sabemos que em Java para se criar um objeto devemos usar o operador `new` ou então algum outro meio de construção usando uma `Factory`, `Builder` ou um método estático inicializador, que é o caso dessa classe, porém o estranho aqui é que o método usa o nome `getInstance`, que geralmente é utilizado em construções do tipo `Singleton` (design patterns), indicando que a classe só possui uma instância única e o método estático sempre retorna essa mesma instância, o que causa confusão ao usar essa classe pois fica a dúvida se você está realmente criando um novo objeto ou está trabalhando com um objeto reutilizável por trás, por meio do `Singleton`.
{: style="text-align: justify;"}

E aqui no caso do `Java` não é um `Singleton`, e sim é criada uma nova instância mesmo.
{: style="text-align: justify;"}

```java
Calendar calendar01 = Calendar.getInstance();
Calendar calendar02 = Calendar.getInstance();

boolean areEquals = calendar01 == callendar02; // Falso
```

## JavaScript

Em `JavaScript` temos o mesmo problema semântico do `Java`, tendo uma classe (ou função se preferir) pra representar a data chamada `Date` mas que também armazena data e hora.
{: style="text-align: justify;"}

```javascript
Date date = new Date();

console.log(date); // Wed Oct 21 2020 19:23:26 GMT-0300 (Brasilia Standard Time)
```

Isso gera os mesmos problemas mencionados para `Java`, e em `JavaScript` também temos um problema de semântica em dois métodos, vejamos o seguinte.
{: style="text-align: justify;"}

```javascript
Date date = new Date();

date.getHours(); 
date.getMinutes()
date.getSeconds();
date.getFullYear();
date.getMonth();
date.getDate();
date.getDay();
```

Os métodos do objeto `date` parecem normais até o `getDate()` e `getDay()` certo? E são exatamente esses dois que tem o problema, pois no caso o `getDay()` retorna o dia da semana (representado por um inteiro de 0 a 6) e `getDate()` retorna (pasmem) o dia do mês entre 1 e 31.
{: style="text-align: justify;"}

Ai vocês devem perceber que o `getDay()` realmente poderia ter um nome melhor como `getDayOfWeek()`, mas o maior problema aqui é ter um método `getDate()` que retorna o dia do mês, isso parece não ter sentido algum e na minha opinião continua não tendo. Porém, afim de tentar entender isso melhor, eu conversei com amigos que possuem uma vivência maior no inglês e me disseram que em alguns casos se utiiza o termo `date` para se referenciar um dia, seja ele dia do mês ou dia do ano, e talvez no momento da construção da API tenham levado isso em conta.
{: style="text-align: justify;"}

Mal sabiam eles que uma simples utilização de `getDayOfWeek()` e `getDayOfMonth()` teria prevenido várias confusões e intepretações erradas.
{: style="text-align: justify;"}

## Golang

E para finalizar vamos mostrar a última coisa esquisita sobre APIs de data e hora, que é a formatação de datas em `Go`.
{: style="text-align: justify;"}

Independente da linguagem, a formatação de datas sempre segue um padrão mais ou menos parecido, usando sempre letras para a formatação.
{: style="text-align: justify;"}

Vejamos exemplos em diferentes linguagens para o formato `21/10/2020 20:45:04`.
{: style="text-align: justify;"}

```java
// Java
SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy HH:mm:ss");
Date date = new Date();
String fmtDate = dateFormat.format(date);
```

```c#
// C#
DateTime date = DateTime.Now;
string fmtDate = date.ToString("dd/MM/yyyy HH:mm:ss");
```

```perl
# Perl
my $fmt_date = strftime "%d/%m/%Y %H:%M:%S", localtime;
```

Nada diferente do usual, certo?

E vamos ao exemplo em `Go`.

```go
// Golang
date := time.Now()
fmtDate := date.Format("02/01/2006 15:04:05")
```

Não, isso não está errado!

Em Golang são utilizados números e nomes para representar formatos de data e hora, o que é totalmente diferente de qualquer outra linguagem que eu conheço.
{: style="text-align: justify;"}

    Formato:                    Identificador: 

    Dia                                      2
    Dia (com zero a esquerda)               02
    Dia da semana  (numero inteiro)      Monday
    Mês com número                           1
    Mês com número (com zero a esquerda)    01
    Ano (completo)                        2006
    Ano (abreviado dois digitos)            06
    Hora                                     3
    Hora (com zero a esquerda)              03
    Hora (24 horas)	                        15
    Minutos                                  4
    Minutos (com zero a esquerda)           04
    Segundos                                 5
    Segundos (com zero a esquerda)          05

Bizarro não é?

Eu procurei saber porquê foi escolhida essa abordagem de números e nomes ao invés da tradicional e me parece que isso foi uma tentativa de [facilitar a memorização das representações](https://medium.com/@simplyianm/how-go-solves-date-and-time-formatting-8a932117c41c){:target="_blank"}, que geralmente nenhum desenvolvedor sabe de cabeça (nisso eu concordo), porém para mim ainda continua sendo bem esquisita essa formatação, e quando vejo isso em um código demoro um tempo para assimilar rs, mas talvez isso também seja devido ao fato de não trabalhar muito com a linguagem.
{: style="text-align: justify;"}

E você, já se deparou com alguma coisa esquisita usando as APIs de data e hora da sua linguagem favorita? Ou tem uma explicação mais elaborada para os itens que coloquei aqui?
{: style="text-align: justify;"}

Compartilhe comigo :)

Até a próxima.



