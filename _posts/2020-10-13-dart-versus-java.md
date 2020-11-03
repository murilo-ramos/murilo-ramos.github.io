---
title: Dart x Java
author: Murilo Costa
date: 2020-10-13 19:00:00 -0300
categories: [Blogging, Java, Dart]
tags: [java, dart, flutter, programacao, linguagem, orientacao-a-objetos]
---

Esse ano comecei a fazer um curso de [Flutter](https://flutter.dev/){:target="_blank"}, que é uma plataforma de desenvolvimento mobile (e no futuro web também) e estou gostando bastante de fazer apps com ela, pois seus recursos de criação de tela via [Widgets](https://flutter.dev/docs/reference/widgets){:target="_blank"}, uso padrão de [Material Design](https://material.io/design){:target="_blank"} e suporte nativo a multiplataforma tornam o desenvolvimento bem interessante.
{: style="text-align: justify;"}

Porém além desses pontos, o que mais tem me chamado atenção nessa plataforma é o [Dart](https://dart.dev/){:target="_blank"} que é a linguagem utilizada para desenvolvimento, e devido a sua semelhança fica impossível de não compará-la com uma das linguagens que mais utilizo atualmente, que é o Java, onde me parece que o Dart é um Java mais sofisticado contendo melhorias nativas que tornam a escrita de código bem mais interessante.
{: style="text-align: justify;"}

Por isso resolvi fazer esse post apontando umas diferenças entre essas duas linguagens, colocando os pontos onde o Dart apresenta melhorias significativas perante o Java e também outros pontos onde, na minha opinião, o Java ainda é superior.
{: style="text-align: justify;"}

Os comentários e exemplos desse post vão levar em consideração o Dart 2 e o Java 11.
{: style="text-align: justify;"}

## Instanciando objetos e criando lambdas

A primeira diferença que se nota de quem vem do Java está na forma de instanciar objetos, onde não é preciso mais utilizar o operador `new`, sendo algo opcional.
{: style="text-align: justify;"}

```dart
// Dart
void main() {
  Foo foo = Foo();
  foo.bar();
}

class Foo {
  void bar() {
    print("Hello World!");
  }
}
```

```java
// Java
public void run() {
    Foo foo = new Foo();
    foo.bar();
}

public class Foo {
    public void bar() {
        System.out.println("Hello World!");
    }
}
```

O operador `new` é algo com o qual você se acostuma depois de um bom tempo trabalhando com a linguagem, porém se reparamos bem podemos perceber que ele não faz muita diferença no código, e por isso eu acho que o Dart  acertou em não obrigar a utilizá-lo.
{: style="text-align: justify;"}

E a criação de lambdas tem essa mesma redução, não sendo mais necessário em Dart usar algo como `=>`.
{: style="text-align: justify;"}

```dart
// Dart
doSomething(() {
  print("Hello World"));
});
```

```java
//Java
doSomething(() ->  {
    System.out.println("Hello World"))
};
```

## Inferência por tipo

Inferência por tipo é algo que o Java demorou bastante para ter na linguagem, mas desde sua versão 10 já é possível utilizar a palavra reservada `var` para declaração de atributos.
{: style="text-align: justify;"}

```java
// Java
public void printText() {
    String firstWord = "Hello";
    var secondWord = "World";
    System.out.println(firstWord + " " + secondWord);
}
```

E em Dart também temos essa mesma possibilidade mas de forma expandida, onde podemos utilizar a inferência por tipo em atributos de classe, constantes e outros, o que não é possível de se fazer em Java.
{: style="text-align: justify;"}

```dart
// Dart
final firstWord = "Hello";

class HelloWorld {
  var secondWord = "World";
  
  void printMessage() {
    var message = firstWord + " " + secondWord + "!";
    print(message);
  }
}
```

## Concatenação de strings

O Java ainda é umas das poucas linguagens que mesmo nas versões mais novas não incorporou nenhum mecanismo mais moderno para concatenar strings.
{: style="text-align: justify;"}

```java
// Java
public void printText() {
    String word = "World";
    System.out.println("Hello " + word + "!");
}
```

Já no Dart temos a forma tradicional e também podemos usar o `String interpolation` para concatenar strings.
{: style="text-align: justify;"}

```dart
// Dart
void printText() {
    var word = "World";
    print("Hello " + word + "!");
    print("Hello ${word}!"); // String interpolation
}
```

## Callbacks

A linguagem Dart considera todo tipo de atributo como objeto, incluindo funcões, o que permite que elas sejam passadas por parâmetros para outros métodos, provendo o famoso `callback`. Veja o exemplo abaixo onde é passada uma funcão `lambda` como parâmetro para a função `execute`.
{: style="text-align: justify;"}

```dart
// Dart
void main() {
  execute((name) {
    print("Seu nome é ${name}.");
  });
}

void execute(Function printName) {
  printName("Murilo");
}
```

Em Java não existe suporte para este tipo de abordagem, onde geralmente se usam interfaces com só um método para tentar prover o mesmo resultado.
{: style="text-align: justify;"}

```java
// Java
public class Main {
    public static void main(String[] args) {
        execute((name) -> System.out.println("Seu nome é " + name + "."));
    }

    private static void execute(MyFunction function) {
        function.printName("Murilo");
    }

    public interface MyFunction {
        void printName(String name);
    }
}
```

## Parâmetros Opcionais

Uma outra feature sensacional do Dart é o uso de parâmetros opcionais, que podem ser passados para funções, construtores de classes e aparentemente qualquer lugar que possa receber parâmetros, devendo serem declarados dentro de chaves `{}`.
{: style="text-align: justify;"}

Para usar os parâmetros opcionais eles devem sempre serem passados usando `nomeDoParametro: valor` e quando um parâmetro opcional não é preenchido, seu valor se torna nulo.
{: style="text-align: justify;"}

```dart
// Dart
void main() {
  printInfo("Murilo");
  printInfo("Murilo", lastName: "Costa");
  printInfo("Murilo", age: 31);
  printInfo("Murilo", lastName: "Costa", age: 31);
}

void printInfo(String name, {String lastName, int age}) {
  var text = new StringBuffer();

  text.write("Olá, meu nome é ${name}");
  
  if (lastName != null) {
    text.write(" ${lastName}");
  }
  
  if (age != null) {
    text.write(" e eu tenho ${age} anos");
  }
  
  text.write(".");
  
  print(text.toString());
}
```

Em Java uma forma de se fazer isso é usando sobrecarga de métodos, sendo também uma forma eficiente e aderente às boas práticas de programação, porém muito mais verbosa.
{: style="text-align: justify;"}

```java
// Java
public void run() {
    printInfo("Murilo");
    printInfo("Murilo","Costa");
    printInfo("Murilo", 31);
    printInfo("Murilo", "Costa", 31);
}

private void printInfo(String name) {
    printInfo(name, null, -1);
}

private void printInfo(String name, String lastName) {
    printInfo(name, lastName, -1);
}
private void printInfo(String name, int age) {
    printInfo(name, null, age);
}

private void printInfo(String name, String lastName, int age) {
    var text = new StringBuilder(); // Usando inferência por tipo do Java 10 :)

    text.append("Olá, meu nome é " + name);

    if (lastName != null) {
        text.append(" " + lastName);
    }

    if (age != -1) {
        text.append(" e eu tenho " + age + " anos");
    }

    text.append(".");

    System.out.println(text.toString());
}
```

## Uso de Getters

Outra coisa legal no Dart é o uso de `getters` personalizados, que criam atributos em classes podendo retornar valores de atributos da classe, valores customizados e/ou calculados.
{: style="text-align: justify;"}

```dart
// Dart
void main() {
  var murilo = Murilo();
  print("Primeiro Nome: ${murilo.firstName}");
  print("Nome Completo: ${murilo.fullName}");
}

class Murilo {
  String name = "Murilo";
  String lastName = "Costa";
  
  String get firstName => name;
  
  String get fullName {
    return "${name} ${lastName}";
  }
}
```

Em Java ainda não saímos dos tradicionais `setters and getters`.
{: style="text-align: justify;"}

```java
// Java
public void run() {
    Murilo murilo = new Murilo();
    System.out.println("Primeiro Nome: " + murilo.getFirstName());
    System.out.println("Nome Completo: " + murilo.getFullName());
}

public class Murilo {
    private String name = "Murilo";
    private String lastName = "Costa";
    
    public String getFirstName() {
        return name;
    }
    
    public String getFullName() {
        return name + " " + lastName;
    }
}
```

Apesar dos getters em Dart serem bacanas eles ainda não superam o uso de [Properties em C#](https://docs.microsoft.com/pt-br/dotnet/csharp/programming-guide/classes-and-structs/properties){:target="_blank"}, que na minha opinião ainda são muito melhores.
{: style="text-align: justify;"}

## Construtores

Essa é a parte que eu percebi que Dart mais inovou, trazendo várias melhorias para criação de objetos e utilização de parâmetros.
{: style="text-align: justify;"}

Em Java, ao passar parâmetros no construtor não é possível fugir da verbosidade de reatribuir os parâmetros do construtor para parâmetros internos.
{: style="text-align: justify;"}

```java
// Java
public class Person {
  final String name;
  final String lastName;

  public Person(String name, String lastName) {
    this.name = name;
    this.lastName = lastName;
  }

  public String getFullName() {
    return name + " " + lastName;
  }
}
```

Já em Dart isso pode ser feito utilizando somente a palavra reservada `this` e o nome dos parâmetros, contanto que sejam do mesmo nome (que é o padrão em orientação a objetos).
{: style="text-align: justify;"}

```dart
// Dart
class Person {
  final String name;
  final String lastName;

  Person(this.name, this.lastName);

  String get fullName {
    return "${name} ${lastName}";
  }
}
```

E como falei no tópico de parâmetros opcionais eles também podem ser utilizados nos construtores, onde inclusive podemos ter um construtor com somente parâmetros opcionais.
{: style="text-align: justify;"}

```dart
// Dart
class Person {
  final String name;
  String lastName;
  
  Person(this.name, {this.lastName}); // Parâmetro obrigatório e opcional
}
```

```dart
// Dart
class Person {
  String name;
  String lastName;
  
  Person({this.name, this.lastName}); // Somente parâmetros opcionais.
}
```

E é claro que os parâmetros do construtor não precisam ser da classe, pois podem ser parâmetros comuns.
{: style="text-align: justify;"}

```dart
// Dart
class MessagePrinter {

  MessagePrinter(String message01, {String message02}) {
    print(message01);

    if (message02 != null) {
      print(message02);
    }
  }
}
```

Lembrando que os parâmetros opcionais devem ser passados usando `nomeDoParametro: valor`.
{: style="text-align: justify;"}

```dart
// Dart
void main () {
  MessagePrinter messagePrinter;
  messagePrinter = MessagePrinter();
  messagePrinter = MessagePrinter(message01: "Message 01");
  messagePrinter = MessagePrinter(message02: "Message 02");
  messagePrinter = MessagePrinter(message01: "Message 01", message02: "Message 02");
}

class MessagePrinter {

  MessagePrinter({String message01, String message02}) {
    if (message01 != null) {
      print(message01);
    }

    if (message02 != null) {
      print(message02);
    }
  }
}
```

Eu nem preciso dar exemplos de como fazer em Java, pois você já deve ter percebido que precisaríamos de várias sobrecargas de construtor para atingir o mesmo resultado.
{: style="text-align: justify;"}

E por último na parte de construtores temos os `Named Constructors`, que permitem ter um construtor para a classe com parâmetros diferenciados e um com um nome específico, dando uma semântica bem legal a este tipo de construção.
{: style="text-align: justify;"}

```dart
// Dart
void main() {
  var personWithName = Person("Murilo");
  var personWithFullInfo = Person.fullInfo("Murilo", "Costa", 31);
}

class Person {
  final String name;
  String lastName;
  int age;
  
  Person(this.name);
  
  Person.fullInfo(this.name, this.lastName, this.age);
}
```

Inclusive em Dart só podemos ter um construtor com o nome da classe. Se for necessário ter qualquer outro construtor ele deverá ser um `Named Constructor`.
{: style="text-align: justify;"}

Em Java não existe essa possibilidade, onde o modo mais comum de ser ter este tipo de semântica é utilizando inicializadores estáticos.
{: style="text-align: justify;"}

```java
// Java
public void run() {
    Person person = Person.personWithFullInfo("Murilo", "Costa", 31);
}

public class Person {
    private final String name;
    private String lastName;
    private int age;

    public Person (String name) {
        this.name = name;
    }

    public static Person personWithFullInfo(String name, String lastName, int age) {
        Person person = new Person(name);
        person.setLastName(lastName);
        person.setAge(age);

        return person;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## Async/Await

Outra coisa moderna que o Dart traz é o uso de código assíncrono na linguagem, tendo isso de forma nativa em sua execução.
{: style="text-align: justify;"}

No código abaixo podemos ver uma função que chama outras duas funções assícronas, onde uma ocorre de forma assíncrona e a outra é aguardada (await).
{: style="text-align: justify;"}

```dart
// Dart
void main() async {
  print("Teste 01");
  
  printMessage01(); // Será executada em modo async
  
  await printMessage02(); // É async, mas processo principal irá aguardar a execução
  
  print("Teste 04");
}

void printMessage01() async {
  await new Future.delayed(const Duration(milliseconds : 100)); // timer de espera de 100 milisegundos
  print("Teste 02");
}

void printMessage02() async {
  print("Teste 03");
}

```

Eu ainda não usei o Dart fora do Flutter, porém para essa plataforma eu posso dizer que o uso de `async/await` é extremamente eficaz em processos que tem sua natureza assíncrona, como obter dados do banco de dados, realizar uma requisição web, dentre outros.
{: style="text-align: justify;"}

Em Java não existe esse suporte de forma nativa, onde o mais próximo que se pode chegar disso é usando libs adicionais como o [Spring que habilitam suporte async ao código](https://www.baeldung.com/spring-async){:target="_blank"}, mas isso ainda está longe do `async/await` que existe em Dart e em outras linguagens como [C#](https://docs.microsoft.com/pt-br/dotnet/csharp/programming-guide/concepts/async/){:target="_blank"} e [ECMAScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/funcoes_assincronas){:target="_blank"}.
{: style="text-align: justify;"}

## Enums

Porém nem tudo são flores e existem coisas do Dart que realmente poderiam ser melhores, onde vou destacar dois pontos aqui.
{: style="text-align: justify;"}

O primeiro é sobre `enums`, que infelizmente em Dart são extremamente simples e não adicionam capacidades extras como é possível no Java.
{: style="text-align: justify;"}

Essa é uma utilização de `enum` em Dart, que não passa muito disso.
{: style="text-align: justify;"}

```dart
// Dart
import 'dart:math';

void main() async {
  var fruit = randomFruit();
  
  switch(fruit) {
    case Fruit.Apple:
      print("Maça");
      break;
    case Fruit.Banana:
      print("Banana");
      break;
    case Fruit.Grape:
      print("Uva");
      break;
    case Fruit.Orange:
      print("Laranja");
      break;
    default:
      print("Não é fruta.");
  }
}

Fruit randomFruit() {
  var rnd = new Random();
  var index = rnd.nextInt(Fruit.values.length);
  return Fruit.values[index];
}

enum Fruit {
  Apple,
  Banana,
  Grape,
  Orange
}
```

Como pode ser visto, `enums` em Dart são simples lista enumeradas com nomes específicos, mas que não podem receber funcões mais avançadas como é possível em Java.
{: style="text-align: justify;"}

```java
// Java
public class Main {
    public static void main(String[] args) {
        var fruit = Fruit.randomFruit();
        System.out.println(fruit.getName());
    }
}

import java.util.Random;

public enum Fruit {
    APPLE("Maça"),
    BANANA("Banana"),
    GRAPE("Uva"),
    ORANGE("Laranja");

    private final String name;

    private Fruit(String name) {
        this.name = name;
    }

    public static Fruit randomFruit() {
        var random = new Random();
        var fruit = random.ints(0, Fruit.values().length).findFirst().getAsInt();

        return Fruit.values()[fruit];
    }

    public String getName() {
        return name;
    }
}
```

Quam já trabalhou com Java sabe o quão poderosos podem ser seus `enums`, facilitando muita coisa na escrita de código.
{: style="text-align: justify;"}

Até hoje considerando todas as linguagens que já trabalhei, somente o [Swift](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html){:target="_blank"} apresenta algo parecido com Java quando se fala de enums.
{: style="text-align: justify;"}

## Público e Privado

O segundo e último item que poderia ser melhor no Dart são os modificadores de acesso, que na minha opinião poderiam ter sido pensados de uma forma melhor. Aqui no caso é muito mais uma questão de gosto mesmo, pois o meu estranhmento tem muito mais a ver com a semântica de escrita do código do que como o código é realmente escrito.
{: style="text-align: justify;"}

Em todos os exemplos usando Dart que coloquei aqui você deve ter reparado na ausência das palavras `private`, `public`  e `protected` certo? E é isso mesmo, essas palavras não são usadas em Dart. Os métodos e atributos são sempre públicos por padrão (`protected` parece que nem existe), e se quiser torná-los privado é necessário declarar um underline `_` no início do método ou atributo, ex: `_myMethod()`.
{: style="text-align: justify;"}

```dart
// Dart
void thisIsAPublicMethod() {
  print("Método público");
}

class Public {
  String thisIsAPublicAttribute = "Atributo público";
}

void _thisIsAPrivateMethod() {
  print("Método privado");
}

class Private {
  String _thisIsAPrivateAttribute = "Atributo privado";
}
```

Apesar de ser só uma forma de escrever, eu acredito que a utilização das palavras `public` e `private` apresentam mais clareza na hora de identificar métodos e atributos, principalmente para quem está começando na linguagem, e já elimina qualquer interpretação errada, por isso acho que isso poderia ter sido melhorado. Inclusive no meu caso eu precisei buscar na documentação oficial o que significavam os métodos e atributos que começavam com underline para então descobrir que era privados, e tenho certeza que não deve ter sido somente eu.
{: style="text-align: justify;"}

E não paramos por aí. Para quem vem do Java e C# que são linguagens que apresentam grande semelhança com o Dart devem estar pensando que atributos e métodos privados são referentes a classe em que estão, correto?
{: style="text-align: justify;"}

Errado!!!

Em Dart atributos e métodos privados são privados somente em relação a outros pacotes, e por isso acabam sendo públicos por padrão se estiverem dentro do mesmo pacote.
{: style="text-align: justify;"}

Ficou difícil de entender? Vamos ao exemplo.
{: style="text-align: justify;"}

Abaixo temos duas classes dentro do mesmo pacote, onde em uma classe é criada um objeto da outra.
{: style="text-align: justify;"}

```dart
// Dart

// package A

void main()  {
  ClassA instanceOfA = ClassA();
  instanceOfA.doSomething();
}

class ClassA {
  void doSomething() {
    ClassB instanceOfB = ClassB();
    
    print(instanceOfB._value);
    print(instanceOfB.getValue());
  }
}

class ClassB {
  String _value = "value";

  String getValue() {
    return this._value;
  }
}
```

Repare que em `ClassA` o método `doSomething()` cria uma instância de `ClassB` e consegue acessar o atributo privado `_value` tanto usando o método público `getValue()` como também consegue acessar o atributo diretamente pelo seu nome `_value`, e isso é possível somente porque as duas classes estão no mesmo `package A`.
{: style="text-align: justify;"}

Agora imagine que temos essas classes em pacotes diferentes.
{: style="text-align: justify;"}

```dart
// Dart

// package A

import 'package:project/B.dart';

void main()  {
  ClassA instanceOfA = ClassA();
  instanceOfA.doSomething();
}

class ClassA {
  void doSomething() {
    ClassB instanceOfB = ClassB();
    
    print(instanceOfB.getValue());
    print(instanceOfB._value); --> ERRO DE COMPILAÇÃO
  }
}

// package B

class ClassB {
  String _value = "value";

  String getValue() {
    return this._value;
  }
}
```

Neste caso por as classes estarem em pacotes separados, não será possível acessar os métodos e atributos privados de `ClassB` dentro de `ClassA`.
{: style="text-align: justify;"}

Por uma questão de organização de código e prática do Clean Code eu acredito que se métodos e atributos fossem privados a classe que estão seria algo melhor, pois assim é possível quebrar os processos internos da classe em sub-métodos, o que facilita em muito a organização e até mesmo reutilização do código, porém me parece que o Dart não foi pensado assim, onde ele foi mais pensado em manter o que é privado ao pacote e não as classes dentro de um pacote, o que também acaba sendo uma alternativa válida.
{: style="text-align: justify;"}

E isso pode ser visto em alguns pontos do Flutter, como no caso de `Widgets Stateful`, onde é criado um pacote que possui uma classe pública que é usada nos outros pacotes, e uma classe privada contendo toda a lógica do Widget.
{: style="text-align: justify;"}

```dart
// Dart
class MyWidget extends StatefulWidget { // Classe pública
  @override
  _MyWidgetState createState() => _MyWidgetState(); // Atributo privado da classe privada
}

class _MyWidgetState extends State<MyWidget> { // Classe privada
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

E é isso pessoal, acredito que eu tenha conseguido mostrar os pontos mais interessantes dessa linguagem quando a comparamos com o Java, e espero que talvez num futuro próxima as duas possam se espelhar uma na outra para melhorarem e se modernizarem ainda mais.
{: style="text-align: justify;"}

Uma outra linguagem que já faz algum tempo que muitas pessoas elogiam e tem alguma semelhança com o Java é o [Kotlin](https://kotlinlang.org/){:target="_blank"}, que espero estar colocando minhas mãos em breve. Quem sabe até sai um post aqui também :)
{: style="text-align: justify;"}

Os exemplos em Dart podem ser executados no [DartPad](https://dartpad.dev/){:target="_blank"} enquanto que os exemplos em Java é mais fácil executar em sua máquina, pois não achei nada online que seja legal de usar rs.

Valeu e até mais.