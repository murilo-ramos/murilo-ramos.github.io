---
title: Concatenação de strings e performance
author: Murilo Costa
date: 2021-12-20 12:00:00 -0300
categories: [Blogging]
tags: [string, java, big-o, performance, concat, stringbuilder, stringbuffer, concatenacao]
---

Eu acredito que a maioria das pessoas com alguma vivência na programação e que usa ou já usou Java, com certeza já ouviu falar que devemos evitar o uso de concatenação de strings na linguagem e devemos usar formas melhores como o uso do `StringBuilder` para casos em que será necessário concatenar grandes partes de texto, mas já parou alguma vez para entender o porquê disso?
{: style="text-align: justify;"}

Esse é um problema que está ligado diretamente a análise de algoritmos e suas ordens, o famoso [Big O](https://medium.com/linkapi-solutions/o-que-%C3%A9-big-o-notation-32f171e4a045){:target="_blank"}, e vamos entender um pouco sobre isso no post de hoje.
{: style="text-align: justify;"}

Vou usar o Java aqui nesse exemplo, porém o problema também existe em outras linguagens, onde cada uma provê sua própria solução.
{: style="text-align: justify;"}

## Vamos ao teste

Apesar de já ter ouvido sobre essa questão diversas vezes, será que ela realmente é válida? Será que faz alguma diferença mesmo usar `StringBuilder` no lugar de concatenação de strings?
{: style="text-align: justify;"}

Vamos então fazer alguns testes para verificar esse comportamento.
{: style="text-align: justify;"}

Para testar vamos fazer um gerador de textos [Lorem Ipsum](https://pt.wikipedia.org/wiki/Lorem_ipsum){:target="_blank"} aleatório, que irá gerar textos com uma quantidade definida de parágrafos, onde vamos comparar os tempos de execução em minha máquina, um i5 1.3GHz, 4GB de RAM e 128 SSD.
{: style="text-align: justify;"}

Primeiramente vamos definir uma classe de constantes com os parágrafos Lorem Ipsum.
{: style="text-align: justify;"}

```java
public class LoremIpsumText {

    public static String LOREM_TEXT_01 = "Lorem ipsum dolor sit amet, consectetur ...\n";
    public static String LOREM_TEXT_02 = "Donec a rutrum velit, eu sodales libero ...\n";
    public static String LOREM_TEXT_03 = "Duis fringilla porttitor elit, ac conse ...\n";
    public static String LOREM_TEXT_04 = "Fusce a euismod ante, ullamcorper variu ...\n";
    public static String LOREM_TEXT_05 = "Lorem ipsum dolor sit amet, consectetur ...\n";

}
```

Os textos foram cortados aqui no post para não ficar muito grande. Repare que no final de cada texto existe uma quebra de linha representada por `\n`.
{: style="text-align: justify;"}

A geração de parágrafos aleatórios será feita diretamente em uma `enum` representando os parágrafos disponíveis.
{: style="text-align: justify;"}

```java
public enum LoremIpsum {
	
    LOREM_01(LoremIpsumText.LOREM_TEXT_01),
    LOREM_02(LoremIpsumText.LOREM_TEXT_02),
    LOREM_03(LoremIpsumText.LOREM_TEXT_03),
    LOREM_04(LoremIpsumText.LOREM_TEXT_04),
    LOREM_05(LoremIpsumText.LOREM_TEXT_05);

    private String text;
	
    private LoremIpsum(String text) {
        this.text = text;
    }

    public String getText() {
        return text;
    }

    public static LoremIpsum random() {
        var index = new Random().ints(0, values().length - 1)
            .findFirst()
            .getAsInt();

        return values()[index];
    }
}
```

Como pode ser visto na `enum`, cada opção mapeia para um texto diferente e o método `random()` traz um texto de forma aleatória. Isso nos ajuda a ter uma diversidade melhor no processo, garantindo que não ocorra nenhum vício por parte de execução do software, como cache de [JVM](https://www.devmedia.com.br/introducao-ao-java-virtual-machine-jvm/27624){:target="_blank"}  por exemplo.
{: style="text-align: justify;"}

Afim de termos certeza que essa geração é rápida o suficiente e não vai afetar os testes, vamos criar um teste unitário para geração e verificar o tempo decorrido. O teste unitário consistirá em obter um texto de forma aleatória e verificar se ele está de acordo com os textos disponíveis.
{: style="text-align: justify;"}

```java
public class LoremIpsumTest {

    @Test
    public void loremIpsumAleatorio() {
        var lorems = Arrays.asList(
            LoremIpsum.LOREM_01,
            LoremIpsum.LOREM_02,
            LoremIpsum.LOREM_03,
            LoremIpsum.LOREM_04,
            LoremIpsum.LOREM_05
        );

        var lorem = LoremIpsum.random();

        assertTrue(lorems.contains(lorem));
    }
}
```

![unit-test-gerador](/assets/img/posts/concatenacao-de-strings-e-performance/unit-test-gerador.png)

Com esse resultado podemos ver que essa geração é extremamente rápida.
{: style="text-align: justify;"}

Agora vamos então criar a classe que irá gerar os textos, usando concatenação de strings ou `StringBuilder`.
{: style="text-align: justify;"}

```java
public class TextGenerator {

    public static String generateParagraphsTextWithStringConcat(int paragraphCount) {
        String text = "";

        for (int i = 0; i < paragraphCount; i++) {
            text += LoremIpsum.random().getText();
        }

        return text;
    }
	
    public static String generateParagraphsTextWithStringBuilder(int paragraphCount) {
        var text = new StringBuilder();

        for (int i = 0; i < paragraphCount; i++) {
            text.append(LoremIpsum.random().getText());
        }

        return text.toString();
    }
}
```

A classe acima possui somente dois métodos estáticos que fazem a mesma coisa: receber uma quantidade de parágrafos por parâmetro e então gerar textos aleatórios usando junções de parágrafos providos por `LoremIpsum.random()`, de acordo com a quantidade de parágrafos definida.
{: style="text-align: justify;"}

E agora vamos aos teste finais.
{: style="text-align: justify;"}

Os testes consistirão também de testes unitários, que irão gerar o texto com uma quantidade definida de parágrafos e depois testar a quantidade de quebras de linha no texto.
{: style="text-align: justify;"}

Inicialmente vamos usar uma quantidade de cem parágrafos e ver o resultado.
{: style="text-align: justify;"}

```java
public class TextGeneratorTest {

    @Test
    public void cemParagrafosDeTextoComStringConcat() {
        var text = TextGenerator.generateParagraphsTextWithStringConcat(100);
        assertEquals(100, text.chars().filter(ch -> ch == '\n').count());
    }

    @Test
    public void cemParagrafosDeTextoComStringBuilder() {
        var text = TextGenerator.generateParagraphsTextWithStringBuilder(100);
        assertEquals(100, text.chars().filter(ch -> ch == '\n').count());
    }

}
```

![unit-test-cem](/assets/img/posts/concatenacao-de-strings-e-performance/unit-test-cem.png)

Podemos ver na imagem que o tempo de execução foi o mesmo para as duas abordagens: 0,000 segundos.
{: style="text-align: justify;"}

Será que então realmente não têm diferença entre elas? Vamos utilizar mil parágrafos no próximo teste.
{: style="text-align: justify;"}

```java
public class TextGeneratorTest {
    @Test
    public void milParagrafosDeTextoComStringConcat() {
        var text = TextGenerator.generateParagraphsTextWithStringConcat(1000);
        assertEquals(1000, text.chars().filter(ch -> ch == '\n').count());
    }

    @Test
    public void milParagrafosDeTextoComStringBuilder() {
        var text = TextGenerator.generateParagraphsTextWithStringBuilder(1000);
        assertEquals(1000, text.chars().filter(ch -> ch == '\n').count());
    }
}
```

![unit-test-mil](/assets/img/posts/concatenacao-de-strings-e-performance/unit-test-mil.png)

Opa! Agora parece que já temos uma pequena diferença, onde a abordagem com o `StringBuilder` continua com 0,000 segundos, mas a concatenação já está levando 419 milissegundos (0,419 segundos) de execução.
{: style="text-align: justify;"}

Porém isso ainda não é algo que realmente faça tanta diferença assim para que seja uma recomendação levada a sério.
{: style="text-align: justify;"}

Vamos então subir um pouco mais a quantidade de parágrafos no nosso teste final: dez mil.
{: style="text-align: justify;"}

```java
public class TextGeneratorTest {
    @Test
    public void dezMilParagrafosDeTextoComStringConcat() {
        var text = TextGenerator.generateParagraphsTextWithStringConcat(10000);
        assertEquals(10000, text.chars().filter(ch -> ch == '\n').count());
    }

    @Test
    public void dezMilParagrafosDeTextoComStringBuilder() {
        var text = TextGenerator.generateParagraphsTextWithStringBuilder(10000);
        assertEquals(10000, text.chars().filter(ch -> ch == '\n').count());
    }
}
```

![unit-test-dez-mil](/assets/img/posts/concatenacao-de-strings-e-performance/unit-test-dez-mil.png)

E então finalmente temos nossa grande diferença. Usando o `StringBuilder` temos um tempo de execução de 0,007 segundos (7 milissegundos), enquanto que a concatenação levou longos 26,220 segundos, um tempo imensamente maior que o da primeira abordagem, o que comprova que realmente o `StringBuilder` é a melhor alternativa principalmente para textos longos.
{: style="text-align: justify;"}

Mas por quê isso acontece?
{: style="text-align: justify;"}

## Entendendo strings

Antes de falar sobre o problema vamos mostrar algumas coisas sobre as strings e como elas funcionam.
{: style="text-align: justify;"}

A maioria das linguagens (principalmente as compiladas) possuem diferentes tipos de atributos, geralmente chamados de tipos de variáveis, onde eles são separados em **primitivos** e **complexos**.
{: style="text-align: justify;"}

Os primitivos são os tipos mais básicos das linguagens, no caso do Java eles são a menor representação que um tipo pode ter, que são os tipos `int, short, long, byte, float, double, boolean, char` e etc, e os tipos complexos são definições criadas utilizando a linguagem e que agrupam e/ou manusei   am esses tipos primitivos, onde em Java as classes, enums e interfaces são utilizadas para esse propósito. Para exemplificar podemos ter os atributos primitivos `int codigo` e `double valor` para representar o código e valor de um produto, e podemos então criar um tipo complexo que é uma classe `Produto` contendo esses atributos primitivos, o que nos permite manusear os tipos primitivos de forma agrupada no tipo complexo.
{: style="text-align: justify;"}

```java
public class Produto {
    int codigo;
    double valor;
}

var produto = new Produto();
produto.codigo = 1;
produto.valor = 10.0;
```

As strings em Java funcionam dessa mesma forma, onde uma `String` é uma classe que representa um conjunto de letras, armazenadas no formato de um vetor de caracteres, ou seja um `char[] texto`.
{: style="text-align: justify;"}

Basicamente quando uma `String` é declarada, internamente é criado um vetor contendo os caracteres do texto e esse vetor nunca muda, pois as strings são imutáveis, ou seja não podem ser modificadas, e isso implica na fato de que sempre que duas strings são concatenadas é então produzida uma terceira string.
{: style="text-align: justify;"}

```java
String nome = "Murilo ";
String sobrenome = "Costa";
nome = nome + sobrenome;
```

Internamente aqui foram construídos três espaços de memória, o primeiro para representar a string `"Murilo "`, que foi atribuída a variável `nome`, o segundo para representar a string `"Costa"`, atribuída a variável `sobrenome` e então foi criado o último espaço de memória para armazenar os dois valores juntos, reatribuindo o novo valor na variável `nome`.
{: style="text-align: justify;"}

Existem outras coisas importantes ao se falar sobre strings em Java, como o fato delas serem `UTF-16`, o mecanismo de internalização que otimiza drasticamente o reúso de strings, mas esses assuntos não vem ao caso de hoje.
{: style="text-align: justify;"}

## O problema da concatenção

O grande vilão da concatenação de strings aqui está justamente no fato de strings serem imutáveis, o que acaba indiretamente gerando um processo duplicado a cada concatenação realizada. Como falei anteriormente, a concatenação de duas strings sempre produz uma terceira string, que é a junção das duas, e nessa junção basicamente se cria um novo vetor de caracteres com tamanho suficiente para as duas strings e se copia o valor das duas caractere a caractere para a nova string.
{: style="text-align: justify;"}

O código executado é semelhante ao código abaixo.
{: style="text-align: justify;"}

```java
public static String concatenate(String s1, String s2) {
    char[] result = new char[s1.length + s2.length];
    int pos = 0;
	
    for (int i = 0; i < s1.length; i++) {
        result[pos++] = s1.chartAt(i);
    }
    for (int i = 0; i < s2.length; i++) {
        result[pos++] = s2.chartAt(i);
    }

    return new String(result);
}
```

Por esse código percebemos que sempre vamos ter que percorrer as duas strings que são concatenadas por inteiro, e se verificarmos o código de concatenação utilizado nos testes anteriores vamos ver que esse processo se repete utilizando sempre o resultado da última concatenação, o que implica em novamente sempre percorrer a mesma string por inteiro em cada concatenação.
{: style="text-align: justify;"}

Para facilitar vamos reescrever o código do teste realizando a concatenação a partir do código acima.
{: style="text-align: justify;"}

```java
public static String generateParagraphsTextWithStringConcat(int paragraphCount) {
    String text = "";

    for (int i = 0; i < paragraphCount; i++) {
        String paragraph = LoremIpsum.random().getText(); //novo parágrafo

        char[] newText = new char[text.length + paragraph.length];
        int pos = 0;

        for (int i = 0; i < text.length; i++) { //percore toda a string text novamente a cada concatenação
            result[pos++] = text.chartAt(i);
        }
        for (int i = 0; i < paragraph.length; i++) {
            result[pos++] = paragraph.chartAt(i);
        }

        text = new String(newText);
    }

    return text;
}
```

Como pode ser visto no código anterior, para cada novo parágrafo adicionado o processo terá que percorrer toda a string `text` novamente, e é exatamente nesse ponto que a lentidão ocorre, pois quanto maior for a string `text` mais tempo essa cópia irá levar.
{: style="text-align: justify;"}

Agora ficou mais fácil de entender o que ocorre internamente, não é mesmo? E quando temos um processo desse com instruções de loop (for) aninhadas, ou seja uma dentro da outra, processando o mesmo valor, temos um algoritmo de ordem `Big O(n²)`, que é uma execução lenta comparada a outras performances. 
{: style="text-align: justify;"}

E por que isso não ocorre no `StringBuilder`?
{: style="text-align: justify;"}

O `StringBuilder` trabalha de uma forma diferente, onde ele armazena uma espécie de lista strings que não são concatenadas durante a adição, sendo que isso ocorre somente ao chamar algum método que utilize seu valor, como é o caso do `toString()`. E é por isso que esse problema de performance não ocorre, pois cada inserção no `StringBuilder` não necessita percorrer todas as outras strings.
{: style="text-align: justify;"}

A implementação por baixo dos panos com o `StringBuilder` seria mais ou menos assim.
{: style="text-align: justify;"}

```java
public static String generateParagraphsTextWithStringBuilder(int paragraphCount) {
    var list = new LinkedList<String>();
    var size = 0;

    for (int i = 0; i < paragraphCount; i++) {
        String paragraph = LoremIpsum.random().getText(); //novo parágrafo
        size += paragraph.length;
        list.add(paragraph);
    }

    char[] text = new char[size];

    var pos = 0;
    for (String s : list) {
        for (int i = 0; i < s.length; i++) { //só concatena no final
            text[pos++] = s.chartAt(i);
        }
    }

    return new String(text);
}
```

Podemos perceber que nessa implementação não precisamos percorrer toda a string a cada novo parágrafo adicionado, onde a criação da nova string só ocorre no final juntando todas as strings adicionadas na lista, o que configura um algoritmo de ordem `Big O(n)`, muito mais performático que o anterior.
{: style="text-align: justify;"}

*Obs: Existe uma explicação matemática mais precisa para configurar a ordem dos algoritmos citados acima, porém são explicações que envolvem teorias matemáticas que eu não quero trazer para cá, então caso você queira entender um pouco melhor isso, é possível encontrar essas explicações em buscas na internet, como essa [aqui](https://www.reddit.com/r/learnprogramming/comments/4uh2b6/comment/d5pogj8/?utm_source=share&utm_medium=web2x&context=3){:target="_blank"}.*
{: style="text-align: justify;"}

E é por isso que, pelo menos em Java, sempre devemos dar preferência ao `StringBuilder` para concatenar strings ao invés da concatenação tradicional.
{: style="text-align: justify;"}

Agora fica uma última pergunta: Se esse problema ocorre justamente pelo fato de strings serem imutáveis, por que ela são assim? Isso já é assunto para outro post :).


## Bônus: StringBuilder x StringBuffer em Java

Em Java, além do `StringBuilder` existe também uma outra classe chamada `StringBuffer` para realizar concatenações de strings de forma mais performática, onde as duas funcionam basicamente da mesma forma. A maior diferença entre elas é que o `StringBuffer` é mais antigo e ele possui uma serie de locks para uso com processo multi-thread, onde seu uso é desaconselhado em casos single-thread.
{: style="text-align: justify;"}

E é exatamente por essa característica que não é aconselhável utilizar o `StringBuffer` em processos que não tenham concorrência, pois esses locks podem prejudicar um pouco o desempenho do processo.
{: style="text-align: justify;"}

Vamos ver as implementações usando cada um e seus testes para compararmos os resultados.
{: style="text-align: justify;"}

```java
public static String generateParagraphsTextWithStringBuilder(int paragraphCount) {
    var text = new StringBuilder();

    for (int i = 0; i < paragraphCount; i++) {
        text.append(LoremIpsum.random().getText());
    }

    return text.toString();
}

public static String generateParagraphsTextWithStringBuffer(int paragraphCount) {
    var text = new StringBuffer();

    for (int i = 0; i < paragraphCount; i++) {
        text.append(LoremIpsum.random().getText());
    }

    return text.toString();
}
```

```java
@Test
public void dezMilParagrafosDeTextoComStringBuilder() {
    var text = TextGenerator.generateParagraphsTextWithStringBuilder(10000);
    assertEquals(10000, text.chars().filter(ch -> ch == '\n').count());
}

@Test
public void dezMilParagrafosDeTextoComStringBuffer() {
    var text = TextGenerator.generateParagraphsTextWithStringBuffer(10000);
    assertEquals(10000, text.chars().filter(ch -> ch == '\n').count());
}
```

![unit-test-stringbuffer](/assets/img/posts/concatenacao-de-strings-e-performance/unit-test-stringbuffer.png)

Como é possível ver, a diferença no tempo de execução é pouca, mas ela ainda existe, e dependendo da complexidade do seu código isso pode se tornar um problema.
{: style="text-align: justify;"}

E é isso aí pessoal, até a próxima.

Ah, os códigos podem ser obtidos [aqui](https://github.com/murilo-ramos/murilo-tech/tree/master/stringsconcatbuilder){:target="_blank"}.
{: style="text-align: justify;"}
