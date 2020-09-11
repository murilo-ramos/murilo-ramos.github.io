---
title: JCommander - CLI com parâmetros de forma fácil
author: Murilo Costa
date: 2018-12-26 13:37:00 -0300
categories: [Blogging, Java, CLI]
tags: [cli, java, jcommander, linha-de-comando, maven]
---

Aplicações em linha de comando (ou CLI - Command Line Interface) são um tipo de aplicação que nunca morrem, e sempre temos que fazer uma aplicação desse tipo para executar um script, processar um arquivo ou até mesmo subir um servidor, e uma dificuldade geralmente encontrada em aplicações desse tipo são os parâmetros a serem passados para a aplicação que acabam nos obrigado a criar uma estrutura para parsear e interpretar esses parâmetros, gerando um grande problema que as vezes pode levar um tempo para ser resolvido.
{: style="text-align: justify;"}

Como forma de resolver esse problema vou apresentar a biblioteca JCommander, que auxilia muito na utilização de parâmetros em aplicações linha de comando, onde é possível de forma fácil definir quais os parâmetros a aplicação irá possuir, seus tipos e se são obrigatórios ou não.
{: style="text-align: justify;"}

Para demonstração vamos criar um projeto Java com o Maven que será uma calculadora bem simples, então vamos ao código:
{: style="text-align: justify;"}

Vamos criar o projeto jcommander-calc-sample com o Maven e adicionar o JCommander ao `pom.xml` (estou usando o Eclipse como IDE neste exemplo).
{: style="text-align: justify;"}

```xml
<dependencies>
      <dependency>
          <groupId>com.beust</groupId>
          <artifactId>jcommander</artifactId>
          <version>1.72</version>
      </dependency>
</dependencies>
```

Neste exemplo vamos trabalhar com três parâmetros, sendo eles:

Primeiro valor: tipo `Double`

Segundo valor: tipo `Double`

Operação: tipo `Enum` contendo as operações de soma, subtração, multiplicação e divisão

Vamos criar então a classe contendo a enum de operações:

```java
package tech.murilo.jcommandercalcsample;

public enum Operation {
    ADD,
    SUBTRACT,
    MULTIPLY,
    DIVIDE;
}
```

Agora vamos criar uma classe que irá representar os parâmetros lidos na execução por linha de comando. O JCommander usará esta classe para saber quais os parâmetros devem ser lidos e suas configurações e esta classe também irá armazenar os valores inputados:
{: style="text-align: justify;"}

```java
package tech.murilo.jcommandercalcsample;

import com.beust.jcommander.Parameter;
import com.beust.jcommander.Parameters;

@Parameters(separators = "=")
public class CalcParameters {
    
    @Parameter(names = "primeiro_valor", description = "Primeiro valor da operação", required = true)
    private double firstValue;
    
    @Parameter(names = "segundo_valor", description = "Segundo valor da operação", required = true)
    private double secondValue;
    
    @Parameter(names = "operacao", description = "Tipo da operação", required = true)
    private Operation operation;
    
    public double getFirstValue() {
        return firstValue;
    }
    
    public double getSecondValue() {
        return secondValue;
    }
    
    public Operation getOperation() {
        return operation;
    }
}
```

É importante observar que é nesta classe que  definimos os nomes dos parâmetros, se são obrigatórios, o separador a ser utilizado e etc.
{: style="text-align: justify;"}

Com estas classes criadas agora podemos criar a clase `Main`, que será responsável por executar a aplicação:
{: style="text-align: justify;"}

```java
package tech.murilo.jcommandercalcsample;

import com.beust.jcommander.JCommander;

public class Main {
    
    public static void main(String[] args) {
        CalcParameters parameters = new CalcParameters();
        
        try {
            JCommander.newBuilder()
                      .addObject(parameters)
                      .build()
                      .parse(args);
            
            executeOperation(parameters);
        } catch (Exception ex) {
            System.out.println("Erro no processamento: " + ex.getMessage());
        }
    }
    
    private static void executeOperation(CalcParameters parameters) {
        double firstValue   = parameters.getFirstValue();
        double secondValue  = parameters.getSecondValue();
        Operation operation = parameters.getOperation();
        
        System.out.println("Primeiro valor: " + firstValue);
        System.out.println("Segundo valor: " + secondValue);
        System.out.println("Operação: " + operation.name());
        
        double result = 0;
        
        switch (operation) {
        case ADD:
            result = firstValue + secondValue;
            break;
        case SUBTRACT:
            result = firstValue - secondValue;
            break;
        case MULTIPLY:
            result = firstValue * secondValue;
            break;
        case DIVIDE:
            result = firstValue / secondValue;
            break;
        }
        
        System.out.println("Resultado: " + result);
    }
}
```

Como pode ser visto a utilização do JCommander é muito simples, bastando somente chamar seu builder com a instrução `JCommander.newBuilder()`, adicionar o objeto da classe de parâmetros com o `addObject()`, chamar o `build()` e finalizar com o `parse()`, passando o array de strings que contem os parâmetros de execução via linha de comando, onde o JCommander irá parseá-los de acordo com a classe de configuração.
{: style="text-align: justify;"}

Sendo assim, podemos gerar um jar executável e rodar um teste para ver o resultado.

Para gerar um jar executável, adicione a seguinte instrução no `pom.xml` e depois execute o comando `mvn package`:
{: style="text-align: justify;"}

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <configuration>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
                <finalName>jcommander-calc-sample</finalName>
                <appendAssemblyId>false</appendAssemblyId>
                <archive>
                    <manifest>
                        <mainClass>tech.murilo.jcommandercalcsample.Main</mainClass>
                    </manifest>
                </archive>

            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>single</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

    cd jcommander-calc-sample
    mvn package


Assim podemos executar a aplicação:


    $>java -jar target/jcommander-calc-sample.jar primeiro_valor=1 segundo_valor=2 operacao=ADD
    Primeiro valor: 1.0
    Segundo valor: 2.0
    Operação: ADD
    Resultado: 3.0


Acredito que você tenha percebido um problema na aplicação com relação a enum `Operation`, pois apesar do JCommander conseguir interpretar nativamente uma enum, não existe uma validação para verificar se é um valor válido  de enum, e ficou bem esquisito chamar a enum pelo nome nos parâmetros, como ADD, SUBTRACT e DIVIDE.
{: style="text-align: justify;"}

Para resolver esse problema o JCommander permite que criemos um mecanismo para converter um determinado parâmetro para um tipo específico  de objeto, facilitando assim a entrada do usuário.
Neste exemplo vamos criar esse  mecanismo para a enum de operações, melhorando sua utilização.
{: style="text-align: justify;"}

Para começar vamos incrementar a enum adicionando nomes melhores as operações (em português) e adicionando um método para se obter um valor de enum a partir do nome.
{: style="text-align: justify;"}

```java
package tech.murilo.jcommandercalcsample;

public enum Operation {
    
    ADD("Somar"),
    SUBTRACT("Subtrair"),
    MULTIPLY("Multiplicar"),
    DIVIDE("Dividir");
    
    private String name;
    
    private Operation(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    public static Operation getOperationByName(String name) {
        for (Operation operation : Operation.values()) {
            if (operation.getName().equals(name)) {
                return operation;
            }
        }
        
        throw new IllegalArgumentException("Nome não encontrado na lista de operações: " + name);
    }
}
```

Agora vamos criar uma classe `Converter` que será usada pelo JCommander para converter o parâmetro do input para nosso tipo específico. Neste caso o parâmetro será um string contendo o nome  da operação e o converter irá converter essa string para um valor da enum usando o método criado anteriormente. Para fazer isso criamos a classe implementando a interface `IStringConverter\<T\>`.
{: style="text-align: justify;"}

```java
package tech.murilo.jcommandercalcsample;

import com.beust.jcommander.IStringConverter;

public class OperationEnumConverter implements IStringConverter<Operation> {
    public Operation convert(String name) {
        return Operation.getOperationByName(name);
    }
}
```

E agora adicionamos esse converter na classe de parâmetros:

```java
@Parameter(names = "operacao", description = "Tipo da operação", required = true, converter = OperationEnumConverter.class)
private Operation operation;
```

Por fim alteramos a classe `Main` para exibir o nome da operação:

```java
System.out.println("Primeiro valor: " + firstValue);
System.out.println("Segundo valor: " + secondValue);
System.out.println("Operação: " + operation.getName());
```

E pronto, agora podemos gerar o jar novamente e testar usando os nomes definidos na enum.


    $>java -jar target/jcommander-calc-sample.jar primeiro_valor=1 segundo_valor=2 operacao=Somar
    Primeiro valor: 1.0
    Segundo valor: 2.0
    Operação: Somar
    Resultado: 3.0

Podemos agora também utilizar um nome de operação que não existe, onde de acordo com o tratamento feito no enum será exibida uma mensagem mais informativa ao usuário:
{: style="text-align: justify;"}


    $>java -jar target/jcommander-calc-sample.jar primeiro_valor=1 segundo_valor=2 operacao=RaizQuadrada
    Erro no processamento: Nome não encontrado na lista de operações: RaizQuadrada


Assim podemos ver como uma biblioteca simples pode trazer uma facilidade enorme na criação de aplicações CLI, evitando a escrita de várias linhas de código para tratamento de parâmetros e focando mais nas lógicas de negócio de sua aplicação.
{: style="text-align: justify;"}

Para saber mais sobre o JCommander o [site](http://jcommander.org/){:target="_blank"}.

Projeto completo está no [GitHub](https://github.com/murilo-ramos/murilo-tech/tree/master/jcommander-calc-sample){:target="_blank"}.