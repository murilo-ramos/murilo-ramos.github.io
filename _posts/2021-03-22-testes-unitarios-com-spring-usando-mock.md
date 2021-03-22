---
title: Testes unitários com Spring Framework usando mock
author: Murilo Costa
date: 2021-03-22 17:40:00 -0300
categories: [Blogging, Java, Testes, Dicas]
tags: [java, test, junit, mockito, easy-mock, spring, framework, mock]
---

Atualmente podemos dizer [Spring Framework](https://spring.io/){:target="_blank"} é a escolha de praticamente todos os desenvolvedores Java que estão iniciando em um novo projeto, e não é pra menos pois esse framework conseguiu reunir em um lugar só toda a facilidade para desenvolvimento de aplicações Web com sua criação de serviços REST, acesso a dados, gerenciamento de filas e acesso a outros serviços, tornando-se um padrão entre seus desenvolvedores.
{: style="text-align: justify;"}

E além de tudo que o Spring oferece, algo que existe no framework desde sua criação e que ajuda e muito os desenvolvedores é a sua facilidade de trabalhar com injeção de dependências, pois ele possui um container de inversão de controle que permite facilmente injetar dependências em uma classe usando a famosa anotação mágica `@Autowired`.
{: style="text-align: justify;"}

```java
public class MyClass {

    @Autowired
    private MyDependency myDependency;

    public void foo() {
        myDependency.bar();
    }
}
```

Porém, junto com essa facilidade vem um problema que trava muitos desenvolvedores no momento de criar testes unitários:
{: style="text-align: justify;"}

**Como criar um mock de uma classe injetada por @Autowired.**

Realmente, como é o próprio Spring quem faz a injeção, fica complicado de fazer esse mock, pois não é legal depender do Framework para fazer os testes unitários, já que em princípio um teste unitário deve depender somente da classe/arquivo que está sendo testado.
{: style="text-align: justify;"}

E para resolver esse problema é claro que temos alternativas, que vou apresentar a seguir usando como exemplo um projeto de teste encontrado por completo [aqui](https://github.com/murilo-ramos/murilo-tech/tree/master/spring-unit-test/birthdaychecker){:target="_blank"}.
{: style="text-align: justify;"}

Este projeto é uma aplicação web [SpringBoot](https://spring.io/projects/spring-boot){:target="_blank"} simples que a partir de seu nome e data de nascimento exibe uma mensagem dizendo quando será seu próximo aniversário. Nos testes iremos focar na classe `BirthdayService` que calcula quando será a próxima data de aniversário e constrói a mensagem, e na classe `LocalDateService` injetada pelo Spring e responsável por prover a data atual.
{: style="text-align: justify;"}

O projeto está usando o [JUnit 5](https://junit.org/junit5/){:target="_blank"} que já faz parte da configuração inicial do SpringBoot.
{: style="text-align: justify;"}

**Observação:** Se você não sabe o que é um mock aconselho e ler primeiro esse meu [post](https://murilo.tech/posts/estrategias-para-testes-unitarios/){:target="_blank"} .
{: style="text-align: justify;"}

## Injetando no construtor ou setter

Tudo bem, eu acredito que essa não seja a alternativa que você esteja procurando, mas ela funciona muito bem!
{: style="text-align: justify;"}

Eu sei que a maioria das pessoas usa o `@Autowired` direto na declaração da dependência para facilitar a escrita do código, como podemos ver a seguir.
{: style="text-align: justify;"}

```java
@Service
public class BirthdayService {
    
    public static final DateTimeFormatter DATE_FORMAT = DateTimeFormatter.ofPattern("dd/MM/yyyy");
    
    @Autowired
    private LocalDateService localDateService;
    
    public String getBirthdayMessage(String name, LocalDate bornDay) {
        var nextBirthDay = getNextBirthday(bornDay);
        
        var message = new StringBuilder()
                .append("Olá ")
                .append(name)
                .append(", seu próximo aniversário será no dia ")
                .append(DATE_FORMAT.format(nextBirthDay))
                .append("!");
        
        return message.toString();
    }

    private LocalDate getNextBirthday(LocalDate bornDay) {
        var currentDate = localDateService.getCurrentDate();
        var birthday = LocalDate.of(currentDate.getYear(), bornDay.getMonth(), bornDay.getDayOfMonth());

        if (currentDate.isAfter(birthday)) {
            birthday = birthday.plusYears(1);
        }

        return birthday;
    }
}
```

Mas é exatamente essa forma de escrita que causa o todo o problema de utilizar um mock nos testes unitários, o que pode ser resolvido passando a dependência via construtor ou via `setter`, como vemos abaixo.
{: style="text-align: justify;"}

```java
@Service
public class BirthdayService {
    
    public static final DateTimeFormatter DATE_FORMAT = DateTimeFormatter.ofPattern("dd/MM/yyyy");

    private LocalDateService localDateService;
    
    @Autowired
    public void setLocalDateService(LocalDateService localDateService) {
        this.localDateService = localDateService;
    }

    public String getBirthdayMessage(String name, LocalDate bornDay) {
        //...
    }
}
```

Usando o `@Autowired` no método de `setter` da dependência irá fazer com que o Spring consiga realizar a injeção normalmente, e nos permite escrever o teste passando o mock por parâmetro para a classe testada.
{: style="text-align: justify;"}

Neste exemplo vou usar o [Mockito](https://site.mockito.org/){:target="_blank"} como biblioteca de mock.
{: style="text-align: justify;"}

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;

import java.time.LocalDate;

import org.junit.jupiter.api.Test;

public class BirthdayServiceTest {

    @Test
    public void shouldReturnBirthdayMessage() {
        var name    = "Murilo";
        var bornDay = LocalDate.of(1989, 7, 10);

        LocalDateService localDateServiceMock = mock(LocalDateService.class);
        when(localDateServiceMock.getCurrentDate()).thenReturn(LocalDate.of(2021, 3, 15));

        BirthdayServiceAlternative birthdayService = new BirthdayServiceAlternative();
        birthdayService.setLocalDateService(localDateServiceMock); //atribuindo o mock

        var message = birthdayService.getBirthdayMessage(name, bornDay);

        assertEquals(message, "Olá Murilo, seu próximo aniversário será no dia 10/07/2021!");
    }
}
```

E assim resolvemos de maneira rápida e fácil o problema de mock.
{: style="text-align: justify;"}

## Usando o Mockito extension

Mas eu sei que a realidade de muitos projetos é bem diferente e a abordagem mais utilizada é adicionar o `@Autowired` diretamente no atributo, por isso temos que recorrer a outros meios para conseguir prover um mock.
{: style="text-align: justify;"}

```java
@Service
public class BirthdayService {
    
    public static final DateTimeFormatter DATE_FORMAT = DateTimeFormatter.ofPattern("dd/MM/yyyy");
    
    @Autowired
    private LocalDateService localDateService;
    
    public String getBirthdayMessage(String name, LocalDate bornDay) {
        //...
    }
}
```

Pensando nisso algumas bibliotecas de mock começaram a adicionar recursos extras permitindo que o teste fosse executado utilizando mais recursos dessas bibliotecas, geralmente usando a anotação `@RunWith`, fazendo com que essa injeção de mock acontecesse de forma transparente, e partir do JUnit 5 esse tipo de recurso foi incorporado ao framework usando o nome de [extensions](https://www.baeldung.com/junit-5-extensions){:target="_blank"}, que agora permitem fazer isso de forma super simples.
{: style="text-align: justify;"}

Aqui vamos usar a extension provida pelo Mockito 1.10.19, e vamos anotar o nosso mock com a anotação `@Mock` e nossa classe a ser testado com o `@InjectMocks`, que irá injetar o mock.
{: style="text-align: justify;"}

```java
@ExtendWith(MockitoExtension.class)
public class BirthdayServiceWithMockitoExtensionTest {
    
    @Mock
    private LocalDateService localDateServiceMock;
    
    @InjectMocks
    private BirthdayService birthdayService;

    @Test
    public void shouldReturnBirthdayMessage() {
        var name    = "Murilo";
        var bornDay = LocalDate.of(1989, 7, 10);

        when(localDateServiceMock.getCurrentDate()).thenReturn(LocalDate.of(2021, 3, 15));

        var message = birthdayService.getBirthdayMessage(name, bornDay);

        assertEquals("Olá Murilo, seu próximo aniversário será no dia 10/07/2021!", message);
    }
}
```

E pronto. Muito simples não é mesmo?
{: style="text-align: justify;"}

E não é somente o Mockito que provê uma extension.
{: style="text-align: justify;"}

## Usando o EasyMock extension

Além do Mockito, o [EasyMock](https://easymock.org/){:target="_blank"} é uma das libs de mock mais utilizadas em Java, e também provê o mecanismo de extension para injeção de mocks.
{: style="text-align: justify;"}

A forma de utilizar é praticamente idêntica ao Mockito, com a diferença que a classe a ser testada recebe a anotação `@TestSubject` para que sejam injetadas as dependências, e é claro que o EasyMock possui métodos diferentes para declaração dos comportamentos esperados.
{: style="text-align: justify;"}

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.easymock.EasyMock.expect;
import static org.easymock.EasyMock.replay;

import org.easymock.EasyMockExtension;
import org.easymock.Mock;
import org.easymock.TestSubject;

//... outros imports

@ExtendWith(EasyMockExtension.class)
public class BirthdayServiceWithEasyMockExtensionTest {
    
    @Mock
    private LocalDateService localDateServiceMock;
    
    @TestSubject
    private BirthdayService birthdayService;

    @Test
    public void shouldReturnBirthdayMessage() {
        var name    = "Murilo";
        var bornDay = LocalDate.of(1989, 7, 10);

        expect(localDateServiceMock.getCurrentDate()).andReturn(LocalDate.of(2021, 3, 15));
        replay(localDateServiceMock);

        var message = birthdayService.getBirthdayMessage(name, bornDay);

        assertEquals("Olá Murilo, seu próximo aniversário será no dia 10/07/2021!", message);
    }
}
```

Muito fácil não é mesmo?
{: style="text-align: justify;"}

Neste exemplo usamos a versão 4.2 do EasyMock.
{: style="text-align: justify;"}

## Usando o Spring Framework

Mas e se estivermos usando versões antigas do JUnit, Mockito ou EasyMock, onde essas possibilidades não existem, como poderemos usar esses mocks?
{: style="text-align: justify;"}

Talvez foi pensando nisso que o Spring passou a prover uma forma de também injetar os mocks em nossos testes unitários, que não é tão prática como as mostradas anteriormente, porém também funciona muito bem.
{: style="text-align: justify;"}

O Spring disponibiliza desde muito tempo a classe utilitária `ReflectionTestUtils` que permite injetarmos os mocks usando [reflections](https://www.devmedia.com.br/conhecendo-java-reflection/29148){:target="_blank"}, o que com um pouco mais de trabalho também resolve o problema.
{: style="text-align: justify;"}

Nesse caso nós criamos uma instância da classe a ser testada e injetamos os mocks utilizando o método `setField` da classe `ReflectionTestUtils`, passando como parâmetro o objeto onde será injetada as dependências, o nome do atributo dentro da classe testada e o mock.
{: style="text-align: justify;"}

Vou usar o Mockito novamente como lib de mock aqui.
{: style="text-align: justify;"}

```java
import org.springframework.test.util.ReflectionTestUtils;

public class BirthdayServiceWithReflectionTestUtilsTest {
    
    private LocalDateService localDateServiceMock;
    
    @BeforeEach
    public void setUp() {
        localDateServiceMock = mock(LocalDateService.class);
    }

    @Test
    public void shouldReturnBirthdayMessage() {
        var name    = "Murilo";
        var bornDay = LocalDate.of(1989, 7, 10);

        when(localDateServiceMock.getCurrentDate()).thenReturn(LocalDate.of(2021, 3, 15));

        BirthdayService birthdayService = buildBirthdayService();
        var message = birthdayService.getBirthdayMessage(name, bornDay);

        assertEquals("Olá Murilo, seu próximo aniversário será no dia 10/07/2021!", message);
    }
    
    private BirthdayService buildBirthdayService() {
        BirthdayService birthdayService = new BirthdayService();
        
        ReflectionTestUtils.setField(birthdayService, "localDateService", localDateServiceMock);
        
        return birthdayService;
    }
}
```

Perceba que foi necessário alguns códigos a mais para inicializar o objeto a ser testado com as dependências e também criar o mock toda vez que um teste for executado (`@BeforeEach`), mas assim conseguimos executar nosso teste unitário normalmente.
{: style="text-align: justify;"}

E com essa última dica eu encerro esse post na esperança que te ajude em suas próximas escritas de testes unitários, e caso tenha alguma dúvida ou sugestão por favor deixe nos comentários.
{: style="text-align: justify;"}

Projeto completo no [Github](https://github.com/murilo-ramos/murilo-tech/tree/master/spring-unit-test){:target="_blank"}.
{: style="text-align: justify;"}

Até mais.
{: style="text-align: justify;"}
