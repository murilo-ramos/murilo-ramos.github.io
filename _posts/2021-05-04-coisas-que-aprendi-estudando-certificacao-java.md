---
title: Coisas que aprendi estudando para a certificação Java
author: Murilo Costa
date: 2021-05-04 17:30:00 -0300
categories: [Blogging, Java]
tags: [java, certificacao, estudos, curiosidades]
---

Como alguns viram, no último mês de abril foi aniversário da linguagem de programação Java, a linguagem que mais venho utilizando desde meu início de carreira, que está completando 25 anos de idade em 2021.
{: style="text-align: justify;"}

E para comemorar essa data a [Oracle](https://www.oracle.com){:target="_blank"}, que atualmente é a detentora da plataforma Java, resolveu criar uma certificação promocional para aqueles que queiram fazer a prova de certificação, e eu resolvi entrar nessa, estudei bastante e consegui passar na prova, onde agora posso dizer que sou um **Oracle Certified Professional: Java SE 11 Developer**.
{: style="text-align: justify;"}

![certificado java](/assets/img/posts/coisas-que-aprendi-estudando-certificacao-java/certificado-java.png)

Esta prova de certificação é bem famosa por ser uma prova difícil, cheia de pegadinhas e coisas extremamente específicas da linguagem, e quem não estuda bastante os detalhes acaba não tendo êxito, e eu senti muito isso durante meus estudos e por isso vou compartilhar alguns pontos aqui sobre a linguagem que me deixaram bem surpreso.
{: style="text-align: justify;"}


## Atribuições retornam o valor atribuído

Atribuições são operações necessárias e comuns nas linguagens de programação pois obviamente é necessário preencher as variáveis utilizadas no código.
{: style="text-align: justify;"}


```java
int a;
double b;
String c;

a = 10;
b = 50.0;
c = "Hello World";
```

E eu sempre entendi essas operações como algo `void`, ou seja, algo que não retorna um valor, mas eu estava errado, no caso do Java essas atribuições sempre retornam o valor atribuído, que é o valor mais a esquerda, possibilitando fazer o código abaixo.
{: style="text-align: justify;"}

```java
System.out.println(a = 10);
System.out.println(b = 50.0);
System.out.println(c = "Hello World");
```

    Output:
    10
    50.0
    Hello World

Interessante, não? Inclusive podemos ter o seguinte tipo de atribuição, que funciona perfeitamente.
{: style="text-align: justify;"}

```java
int a, b, c, d;
a = b = c = d = 10;

System.out.println(a + b + c + d);
```

    Output:
    40

No código acima só precisamos nos atentar que ele só funciona devido ao fato de declaramos previamente as variáveis e os valores foram atribuídos da direita para esquerda, caso contrário poderíamos ter um erro de utilização de variável não inicializada, se for uma variável local.
{: style="text-align: justify;"}

```java
int a, b, c, d;
a = b = c = d; // erro de compilação: Variável d não foi inicializada
```

Apesar de ser um recurso interessante, utilizá-lo no dia-a-dia acredito não ser uma boa opção pois pode criar um código confuso e fora do padrão de código legível. Acredito que são poucos os lugares em que seu uso seja viável, e uma delas é na leitura de arquivos.
{: style="text-align: justify;"}

Perceba que no código a seguir a variável `line` recebe por atribuição o valor da instrução `br.readLine()` e, como esse valor é retornado pela atribuição, ele pode ser verificado se é nulo.
{: style="text-align: justify;"}

```java
try (BufferedReader br = new BufferedReader(new InputStreamReader(inputStream))) {
    String line;
    while ((line = br.readLine()) != null) {
        //código
    }
}
```

## ElseIf

Então, sabe o `elseif` que você usa todo dia enquanto programa em Java? Ele não existe! rsrs.
{: style="text-align: justify;"}

Java realmente não tem o `elseif`, mas vou explicar melhor. A linguagem possui a instrução condicional `if`, que pode vir acompanhada de um `else`.
{: style="text-align: justify;"}

```java
if (condition)
    foo();

if (condition)
    foo();
else
    bar();
```

Porém existe uma instrução a mais que outras linguagens possuem que é o `elseif`, que permite encadear instruções `if` e é exatamente essa instrução que não existe em Java.
{: style="text-align: justify;"}

Vejamos um exemplo de `elseif` em Perl, que usa o nome `elsif` para a instrução.
{: style="text-align: justify;"}

```perl
my $a = 1;

if (a == 1) {
   # codigo
} elsif (a == 2) {
   # codigo
} elsif (a == 3) {
   # codigo
} else {
   # codigo
}
```

Em Java o que fazemos é encadear instruções `if` usando o `else` + `if`, que nada mais é do que if aninhados.
{: style="text-align: justify;"}

```java
int a = 1;

if (a == 1) {
    //codigo
} else if (a == 2) {
    //codigo
} else if (a == 3) {
    //codigo
} else {
    //codigo
}
```

Isso na verdade é a mesma coisa que escrever da seguinte forma.
{: style="text-align: justify;"}

```java
if (a == 1) {
    //codigo
} else {
    if (a == 2) {
        //codigo
    } else {
        if (a == 3) {
            //codigo
        } else {
            //codigo
        }
    }
}
```

E no final acaba sendo tudo igual rs.
{: style="text-align: justify;"}

## Operações matemáticas sem ponto flutuante sempre resultam em um inteiro

Qualquer operação realizada envolvendo `short`, `byte`, `char` ou `int`, sempre vai retornar um `int`.
{: style="text-align: justify;"}

```java
short a = 1;
byte b  = 2;
char c  = 3;
int d   = 4;

var v1 = a + a;
var v2 = a + b;
var v3 = b + b;
var v4 = b + c;
var v5 = c + c;
var v6 = c + d;
var v7 = a + b + c + d;
```

No código acima, v1, v2, v3, v4, v5, v6 e v7 são do tipo `int`. É importante saber disso pois mesmo operações com um mesmo tipo podem precisar de casting para manter o tipo original. Isso parece acontecer na linguagem para evitar problemas de overflow em tipos menores que `int`.
{: style="text-align: justify;"}

```java
short a = 1;
short v1 = (short) (a + a);
```

Para operações com outros tipos de valores o resultado será o maior tipo envolvido.
{: style="text-align: justify;"}

```java
int a = 1;
long b = 2;
float c = 3.0f;
double d = 4.0;

var v1 = a + b; //v1 é long
var v2 = a + c; //v2 é float
var v3 = a + d; //v3 é double
var v4 = b + d; //v4 é double
```

## Existe divisão por zero

Sim, para valores do tipo ponto flutuante existe divisão por zero, e o resultado é um `Infinity`.
{: style="text-align: justify;"}

```java
float a = 1;
double b = 2;

var v1 = a / 0;
var v2 = b / 0;

System.out.println(v1 == Float.POSITIVE_INFINITY);
System.out.println(v2 == Double.POSITIVE_INFINITY);
```

    Output:
    true
    true

Porém para outros tipos de valores a regra matemática ainda persiste, onde a divisão por zero resulta em uma `ArithmeticException`.
{: style="text-align: justify;"}

```java
int a = 1;	
var v1 = a / 0;
```

    Output:
    Exception in thread "main" java.lang.ArithmeticException: / by zero

Se quiser entender um pouco melhor [aqui](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.17.2/){:target="_blank"} você pode ler a especificação.
{: style="text-align: justify;"}


## Declarações de valores

Durante meus estudos foi possível relembrar e descobrir alguns pontos importante sobre declarações de valores.
{: style="text-align: justify;"}

Para se declarar valores do tipo `float` é sempre necessário que o valor venha acompanhado da letra `f`, caso contrário será interpretado como `double`.
{: style="text-align: justify;"}

```java
float a = 1.0f;
float b = 2.0; //erro de compilação
```

Se um valor inteiro for declarado com um zero a esquerda ele será interpretado como octal.
{: style="text-align: justify;"}

```java
int a = 10;
int b = 010;

System.out.println(a);
System.out.println(b);
```

    Output:
    10
    8

Para facilitar a declaração de números grandes pode-se utilizar um underline `_` na declaração.
{: style="text-align: justify;"}

```java
final long UM_MILHAO = 1_000_000;

System.out.println(UM_MILHAO == 1000000);
```

    Output:
    true

Bacana, não é mesmo?

## Sobrescrita de métodos não precisa ocorrer ao pé da letra

O Java por ser uma linguagem a orientada a objetos possui um dos melhores recursos desse tipo de linguagem que é o polimorfismo, que nos permite trabalhar com abstrações de forma eficiente e fácil, e um dos recursos principais é a possibilidade de sobrescrita de métodos de interfaces, classes abstratas e até mesmo classes concretas, quando é permitido.
{: style="text-align: justify;"}

E ao sobrescrever esses métodos eu sempre enxerguei o método sobrescrito como um contrato, algo que não pode ser violado e deve ser idêntico quando sobrescrito, porém eu estive errado e é possível sim que a sobrescrita seja diferente, retornando valores e exceções diferenciadas, desde que aderentes ao método sobrescrito.
{: style="text-align: justify;"}

No caso de exceptions, ao sobrescrever um método que possui uma cláusula `throws` nós não precisamos lançar exatamente a mesma exception, podendo ser uma subclasse da mesma.
{: style="text-align: justify;"}

```java
public class SubClass implements SuperClass {
    @Override
    public void doSomething() throws FileNotFoundException {
        //codigo
    }
}

public interface SuperClass {
    void doSomething() throws IOException;
}
```

Repare que no exemplo acima a interface possui um método que lança `IOException`, mas a classe ao sobrescrever lança `FileNotFoundException` que é uma subclasse de `IOException`, e isso é permitido.
{: style="text-align: justify;"}


E o mais legal é que a classe que está sobrescrevendo pode inclusive optar por não lançar nenhuma exception, e isso também é permitido.
{: style="text-align: justify;"}

```java
public class SubClass implements SuperClass {
    @Override
    public void doSomething() {
        //codigo
    }
}

public interface SuperClass {
    void doSomething() throws IOException;
}
```

Porém o que não é permitido é lançar uma exception não compatível com a declarada no método sobrescrito, como no caso a seguir onde `SQLException` não é compatível com `IOException`.
{: style="text-align: justify;"}

```java
public class SubClass implements SuperClass {
    @Override
    public void doSomething() throws SQLException {
        //codigo
    }
}

public interface SuperClass {
    void doSomething() throws IOException;
}
```

E no caso de métodos que possuem um retorno nós temos a mesma regra, onde o retorno não precisa ser especificamente o mesmo do método sobrescrito, podendo ser também uma subclasse.
{: style="text-align: justify;"}

```java
public class SubClass implements SuperClass {
    @Override
    public ByteArrayOutputStream getStream() {
        return ...;
    }
}

public interface SuperClass {
    OutputStream getStream();
}
```

Apesar da interface definir um `OutputStream` como retorno do método `getStream()`, a sobrescrita está retornando um `ByteArrayOutputStream`.
{: style="text-align: justify;"}

## Interfaces podem ter constantes, métodos privados e métodos default

Esse assunto é relacionado as versões mais recentes da linguagem (da oito em diante) que ganhou vários novos recursos ao longo dos últimos anos, e as interfaces foram as que mais se modernizaram.
{: style="text-align: justify;"}

E sobre essas mudanças existem três itens que achei bem legal e não sabia que era possível, que é a definição de constantes nas interfaces, métodos privados e métodos default.
{: style="text-align: justify;"}

Para se declarar uma constante é só declarar uma variável comum.
{: style="text-align: justify;"}

```java
public interface MyInterface {
    String CAR = "Carro";
    String MOTORBIKE = "Motocicleta";
}

System.out.println(MyInterface.CAR)
```

Declarar uma constante em uma interface não necessita do `public static final` pois isso já é feito automaticamente pela linguagem.
{: style="text-align: justify;"}

Uma interface também pode ter métodos privados, que só podem ser acessados dentro da própria interface.
{: style="text-align: justify;"}

```java
public interface MyInterface {
    private void doSomethingPrivate() {
        //codigo
    }
}
```

Os métodos privados só podem ser acessados por métodos do tipo default, que são métodos com implementação que podem ser declarados dentro de uma interface, e são herdados por quem implementar a interface.
{: style="text-align: justify;"}

```java
public class MyClass implements MyInterface {
    @Override
    public void doIt() {
        doSomething();
    }
}

public interface MyInterface {
    void doIt();
    
    default void doSomething() {
        doSomethingPrivate();
    }
    
    private void doSomethingPrivate() {
        //codigo
    }
}
```

O uso de métodos privados e default em interfaces ajuda quando existe o caso de diferentes implementações possuírem processos em comum, evitando a duplicação de código ou a criação de uma 'baseclass' para reaproveitamento de código.
{: style="text-align: justify;"}


## Sobrescrita de métodos de uma enum

Em um [post](https://murilo.tech/posts/dart-versus-java/){:target="_blank"} anterior eu mostrei que enums em Java são bem poderosas e tem uma série de recursos que facilitam muito o desenvolvimento quando se precisa desse tipo de estrutura, onde comentei até que é possível implementar interfaces com uma enum.
{: style="text-align: justify;"}

```java
public enum MyEnum implements MyInterface {
    FIRST_VALUE {
        @Override
        public String getMessage() {
            return "Primeiro valor.";
        }
    },
    SECOND_VALUE {
        @Override
        public String getMessage() {
            return "Segundo valor.";
        }
    };
}

public interface MyInterface {
    String getMessage();
}
```

Mas o que eu não sabia era que é possível sobrescrever um método da própria enum, colocando um comportamento personalizado para cada item da enum, se necessário.
{: style="text-align: justify;"}

```java
public enum Animal{
    DOG("cachorro"),
    CAT("gato"),
    FISH("peixe") {
        @Override
        public void action() {
            System.out.println("O peixe nada.");
        }
    };
    
    private String name;
    
    Animal(String name) {
        this.name = name;
    }
    
    public void action() {
        System.out.println("O " + name + " anda.");
    }
}
```

Veja que o item `FISH` da enum sobrescreve seu próprio método `action()` para adicionar um comportamento específico.
{: style="text-align: justify;"}

Apesar de esquisito, é um recurso bem interessante.
{: style="text-align: justify;"}

E esses são os pontos mais interessantes que vi enquanto estudava para a certificação, e tenho certeza que se eu nunca tivesse feito essa prova ia acabar morrendo sem saber rsrs, fica assim a experiência.
{: style="text-align: justify;"}

Espero que tenham gostado, e até a próxima pessoal.
{: style="text-align: justify;"}