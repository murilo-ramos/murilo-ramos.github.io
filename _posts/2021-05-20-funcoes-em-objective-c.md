---
title: Funções em Objective-C
author: Murilo Costa
date: 2021-05-20 17:30:00 -0300
categories: [Blogging]
tags: [objectivec, ios, simula, apple, c, funcoes]
---

A linguagem de programação Objective-C foi criada em meados de 1984 e veio com o objetivo de adicionar o paradigma de orientação a objetos a famosa linguagem C, que foi a precursora de muitas outras linguagens e ainda é muito utilizada como base de muitos softwares e plataformas que existem por aí. Apesar de conseguir cumprir com o seu objetivo, ela nunca foi amplamente utilizada pelos desenvolvedores que pareciam se interessar mais por seu primo C++, uma linguagem baseada em C que também é orientada a objetos.
{: style="text-align: justify;"}

Porém, podemos dizer que o jogo virou drasticamente quando a empresa [Apple](https://www.apple.com/){:target="_blank"} lançou seu primeiro smartphone (que praticamente foi o primeiro smartphone em geral) e disponibilizou um [SDK](https://pt.wikipedia.org/wiki/Kit_de_desenvolvimento_de_software){:target="_blank"} de desenvolvimento totalmente baseado em Objective-C, fazendo com que muitos desenvolvedores aprendessem a linguagem para conseguir criar apps para a plataforma [iOS](https://pt.wikipedia.org/wiki/IOS){:target="_blank"}, o que fez a linguagem ganhar uma grande quantidade de adeptos.

Apesar disso, muitos desenvolvedores não gostaram muito da escolha da Apple pôr o Objective-C ser uma linguagem não tão moderna como outras disponíveis na época, principalmente por utilizar muito acesso a ponteiros, que é algo complicado para iniciantes e pode facilmente causar problemas relacionados ao gerenciamento de memória, e por esse e outros motivos que a Apple resolveu criar uma nova linguagem para ser utilizada no lugar que é o [Swift](https://developer.apple.com/swift/){:target="_blank"}, atualmente a linguagem oficial de desenvolvimento para a plataforma Apple.
{: style="text-align: justify;"}

Durante algum tempo eu acabei trabalhando em um app para a plataforma iOS e senti na pele as dificuldades de uso dessa linguagem, principalmente em sua sintaxe que é diferente das linguagens que estou acostumado, porém preciso dizer que nem tudo é ruim como parece, e acabei descobrindo que o Objective-C resolve um problema de nomenclatura de métodos de uma forma bem legal, que vou mostrar para vocês.
{: style="text-align: justify;"}

## O problema

De acordo com os padrões de código limpo, os métodos que criamos devem ter nomes expressivos, que consigam em poucas palavras e verbos apresentar o que será executado, e essa missão muitas vezes se torna complicada pois fica difícil atender a este quesito quando um método possui mais de um parâmetro, onde acabamos com o exemplo de método abaixo, escrito em C#.
{: style="text-align: justify;"}

```csharp
public void ExecuteUpdateForUserUsingKey(User user, Key key)
{
 ...
}

User user;
Key key;

this.ExecuteUpdateForUserUsingKey(user, key);
```

Como precisamos escrever todo o nome do método primeiro, ele acaba se tornando essa 'tripa' de palavras enorme. E quando temos nomes mais específicos de entidades a coisa fica pior ainda.
{: style="text-align: justify;"}

```csharp
public void ExecuteUpdateForLoggedUserUsingMasterKey(LoggedUser user, MasterKey key)
{
 ...
}

this.ExecuteUpdateForLoggedUserUsingMasterKey(loggedUser, masterKey);
```

Para melhorar isso podemos até remover a nomenclatura de entidades do nome do método, porém isso acaba tendo como efeito o desenvolvedor tendo que adivinhar quem é quem no processamento.
{: style="text-align: justify;"}

```csharp
public void ExecuteUpdateFor(LoggedUser user, MasterKey key)
{
 ...
}

this.ExecuteUpdateFor(loggedUser, masterKey);
```

No caso o update é para a chave ou para o usuário?
{: style="text-align: justify;"}

E é esse tipo de situação que o Objective-C acaba resolvendo de uma forma bem elegante.
{: style="text-align: justify;"}


## A solução

A forma como o Objective-C resolve esse problema é fazendo com que os nomes dos parâmetros façam parte do nome do método, inclusive em sua chamada. Vejamos o exemplo a seguir.
{: style="text-align: justify;"}

```objective_c
+(void)doSomethingWithFirstParameter:(FirstParameter)firstParameter andWithSecondParameter:(SecondParameter)secondParameter {
...
}
```

O nome do método acima é composto de `doSomethingWithFirstParameter` com `andWithSecondParameter`, onde sua chamada é feita da seguinte forma.
{: style="text-align: justify;"}

```objective_c
FirstParameter *firstParameter;
SecondParameter *secondParameter;

[self doSomethingWithFirstParameter:firstParameter andWithSecondParameter:secondParameter];
```

Perceba que esse tipo de estrutura facilita e muito a leitura do método pois consegue trazer para a chamada o parâmetro e seu contexto.
{: style="text-align: justify;"}

**Nota:**

Para quem não conhece Objective-C, seguem algumas explicações para entender melhor a declaração de métodos:
{: style="text-align: justify;"}

- O `+` no início do método significa que é um método da classe em questão e não acessa nada da instância. Seria o equivalente ao `static` do C# e Java. Se fosse um método de instância ele deveria começar com `-`;
- Os tipos ficam sempre entre parênteses `()`, por isso temos o retorno `(void)` no início do método e os tipos dos parâmetros `(FirstParameter)` e `(SecondParameter)`;
- O nome do parâmetro é sempre declarado posterior ao tipo `(Tipo)nomeDoParametro`;
- A chamada de um método é sempre feita dentro de colchetes `[]`.


E assim para resolver o problema que foi mostrado no início do post, podemos escrever assim em Objective-C.
{: style="text-align: justify;"}

```objective_c
+(void)executeUpdateForUser:(User)user withKey:(Key)key {
...
}

[self executeUpdateForUser:user withKey:key];
```

Muito mais fácil de se ler, não é mesmo? E essa forma de se escrever ajuda muito também quando fazemos extensão de métodos*.
{: style="text-align: justify;"}

```objective_c
+(void)authenticateUser:(User)user {
...
}

+(void)authenticateUser:(User)user usingCertificate:(Certificate)certificate {
...
}

+(void)authenticateUser:(User)user usingCertificate:(Certificate)certificate andLanguage:(Language)language {
...
}

User *user;
Certificate *certificate;
Language *language;

[self authenticateUser:user];
[self authenticateUser:user usingCertificate:certificate];
[self authenticateUser:user usingCertificate:certificate andLanguage:language];
```

*Isso seria basicamente sobrecarregar os métodos, porém não podemos dizer que ocorre sobrecarga aqui pois o Objective-C não possui suporte a sobrecarga, onde no caso o que temos são métodos com nomes diferentes.

Para exemplificar, o código abaixo seria uma sobrecarga válida em outras linguagens (mesmo método com parâmetros diferentes), porém que causa erro de compilação em Objective-C.

```objective_c
-(void)doSomething:(float)floatValue {
    
}

-(void)doSomething:(int)intValue {
    
}
```

## Curiosidade

Uma curiosidade interessante sobre o Objective-C é que seu modelo de programação não segue o padrão [Simula](https://pt.wikipedia.org/wiki/Simula){:target="_blank"}, que basicamente é seguido pela maioria das linguagens de programação atuais.

Nesse padrão um método é ligado a uma seção de código de uma classe/objeto quase que de uma forma sólida, enquanto que no Objective-C essa construção não existe, e sim são enviadas mensagens à classe/objeto com o nome do método, que é resolvido em tempo de execução.
{: style="text-align: justify;"}

Você pode conferir uma explicação mais detalhada [aqui](https://pt.wikipedia.org/wiki/Objective-C#Mensagens){:target="_blank"}.
{: style="text-align: justify;"}

E é isso pessoal, espero que tenham gostado e até a próxima.
{: style="text-align: justify;"}