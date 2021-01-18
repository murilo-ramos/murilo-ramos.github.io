---
title: Estratégias para testes unitários
author: Murilo Costa
date: 2021-01-18 07:30:00 -0300
categories: [Blogging, C#]
tags: [unit-test, teste-unitario, solid, c#, xunit, moq, dotnet]
---

Nos dias atuais eu nem preciso explicar aqui a importância da escrita de testes automatizados pois a maioria dos desenvolvedores já entende que ter esses testes em seu software significa qualidade e também produtividade, visto que o tempo adicional que se gasta para escrever um teste automatizado é muito menor que o tempo gasto para se resolver problemas que não foram tratados pela falta de testes.
{: style="text-align: justify;"}

E dentre todos os tipos de testes automatizados o que mais é exercitado pelos desenvolvedores é o famoso teste unitário, que tem como objetivo testar as partes do seu software de forma indepedente, tendo como dependência somente o próprio código.
{: style="text-align: justify;"}

Porém ao escrever este tipo de teste nos deparamos com algumas dificuldades que tornam a escrita um pouco complicada, como, por exemplo, saber **o que** deve ser testado, **como** deve ser testado e principalmente como **escrever** um código que pode ser testado.
{: style="text-align: justify;"}

E por isso vou compartilhar aqui algumas dicas e estratégias para a escrita de testes unitários, que vão desde entender o que deve ser testado até as modificações que serão necessárias no código para que seja possível escrevê-los.
{: style="text-align: justify;"}

Para facilitar vamos usar o projeto de exemplo do último [post](https://murilo.tech/posts/refatorando-e-alcancando-codigo-limpo/){:target="_blank"}, onde iremos usar o [xUnit](https://xunit.net/){:target="_blank"} que é compatível com o .Net Core.
{: style="text-align: justify;"}

## Sabendo o que testar

Basicamente um teste unitário precisa testar todo o comportamento de uma unidade do sistema, o que pode ser alcançado seguindo os seguintes pontos.
{: style="text-align: justify;"}

- Os testes devem considerar todos os comportamentos de sucesso e também de erros, pois assim pode-se mapear os erros que podem acontecer;
- Deve-se também considerar as exceções, tanto as que são lançadas quanto as capturadas;
- As entradas dos testes devem ser variadas para garantir uma maior cobertura, como testar uma lista com somente um valor, dois ou mais valores e nenhum valor;
- Os testes com valores inválidos devem ser escritos quando aplicável, pois se sua arquitetura não permite chegar um valor inválido no código a ser testado então não é necessário.

## Fazendo uma lista

Agora que sabemos o que deve ser testado já podemos começar a criar os testes, porém uma dica que dou antes de iniciar é escrever uma lista com todos os testes que devem ser escritos, para que nenhum seja esquecido enquanto se escreve.
{: style="text-align: justify;"}

Essa lista pode ser feita em qualquer lugar, em um editor de texto a parte, no papel, porém eu prefiro já fazer direto no código de testes em formato de comentário, e como o exemplo é usando uma linguagem orientada a objetos faremos em uma classe de teste.
{: style="text-align: justify;"}

Eu prefiro sempre mapear primeiro os casos de erro e depois os casos de sucesso, sempre na ordem do que acontece no código de produção.
{: style="text-align: justify;"}

```csharp
public class FileReaderTest
{

    /*
    Ler dados de arquivo inexistente
    Ler dados com formato de data de contratação inválida
    Ler dados com gênero inválido
    Ler dados com ID que não inicia em 0013
    Ler dados com data de contratação menor que 01/01/2019
    Ler dados com somente um registro e inserir no banco
    Ler dados com mais de um registro e inserir no banco
    */
}
```

É isso mesmo, algo bem simples onde a ideia é que cada item dessa lista será um método de teste.
{: style="text-align: justify;"}

Neste exemplo vou estar considerando como premissa que quando o arquivo de entrada existe ele sempre terá pelo menos um registro já no formato correto.
{: style="text-align: justify;"}

## Escrevendo os testes

Vamos então começar a escrever os testes. Iremos colocar os arquivos de entrada dentro da pasta `Resources` do projeto de testes, que vou colocando aqui de acordo com cada teste.
{: style="text-align: justify;"}

A nomenclatura dos testes seguirá o padrão `Deve_ResultadoEsperado_Quando_AcaoExecutada`. Outros padrões podem ser encontrados [aqui](https://dzone.com/articles/7-popular-unit-test-naming){:target="_blank"}.
{: style="text-align: justify;"}

    
### Ler dados de arquivo inexistente

Esse teste irá tentar ler um arquivo que não existe, onde será lançada uma exceção do tipo `FileNotFoundException`.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_ThrownFileNotFound_When_ReadNonexistentFile()
{
    var fileReader = new FileReader();
    Assert.Throws<FileNotFoundException>(() => fileReader.Read("./Resources/NonexistentFile.txt"));
}
```

### Ler dados com data de contratação inválida

Esse teste irá ler o arquivo `InvalidHireDateFormatFile.txt` que possui a data de contratação com um formato inválido, onde será lançada a exceção `FormatException`.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_ThrownFormatException_When_ReadFileWithInvalidHireDateFormat()
{
    var fileReader = new FileReader();
    Assert.Throws<FormatException>(() => fileReader.Read("./Resources/InvalidHireDateFormatFile.txt"));
}
```

Arquivo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;32/02/2019

### Ler dados com gênero inválido

Esse teste irá ler o arquivo `InvalidGenderFile.txt` que possui um gênero inválido, onde será lançada uma exceção do tipo `Exception` com uma mensagem de erro personalizada, que deverá ser validada também no teste.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_ThrownException_When_ReadFileWithInvalidGender()
{
    var fileReader = new FileReader();

    var exception = Assert.Throws<Exception>(() => fileReader.Read("./Resources/InvalidGenderFile.txt"));

    Assert.Equal("Gênero desconhecido: A", exception.Message);
}
```

Arquivo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;A;2112345678;10/02/2019


### Ler dados com ID que não inicia em 0013

Esse teste irá ler o arquivo `InvalidIDFile.txt` que possui um ID inválido, onde também será lançada uma exceção com mensagem de erro personalizada.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_ThrownException_When_ReadFileWithInvalidID()
{
    var fileReader = new FileReader();

    var exception = Assert.Throws<Exception>(() => fileReader.Read("./Resources/InvalidIDFile.txt"));

    Assert.Equal("ID não começa com 0013: 001501", exception.Message);
}
```

Arquivo:

    001501;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;10/02/2019

### Ler dados com data de contratação menor que 01/01/2019

Esse teste irá ler o arquivo `InvalidHireDateFile.txt` que possui a data de contratação menor que 01/01/2019 sendo, portanto, inválida, onde também será lançada uma exceção com mensagem de erro personalizada.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_ThrownException_When_ReadFileWithInvalidHireDate()
{
    var fileReader = new FileReader();

    var exception = Assert.Throws<Exception>(() => fileReader.Read("./Resources/InvalidHireDateFile.txt"));

    Assert.Equal("Data de importação não pode ser menor que 01/01/2019: 01/01/2018 00:00:00", exception.Message);
}
```

Arquivo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;01/01/2018

### Ler dados com somente um registro e inserir no banco

Agora chegamos nos casos de sucesso, onde o teste irá conseguir percorrer o código até o final, lendo os dados do arquivo, validando e **inserindo no banco de dados**.
{: style="text-align: justify;"}

Porém aqui verificamos que temos um problema grande, pois como falamos anteriormente o teste unitário precisa depender somente do código criado e não possuir dependências externas, como é o caso do banco de dados, o que impossibilita criarmos o teste.
{: style="text-align: justify;"}

E como resolvemos isso? Será que existe uma forma de usar um mecanismo que substitua o acesso ao banco de dados e que possamos usá-lo para testar o comportamento por completo do código?
{: style="text-align: justify;"}

A resposta é sim!
{: style="text-align: justify;"}

## Refatorando dependências

Um dos grandes desafios de se escrever testes unitários sempre está na forma como lidamos com as dependências do nosso código, principalmente as dependências externas, que precisam ser estruturadas de uma forma que possam ser substituídas por mecanismos de [Mock](https://pt.wikipedia.org/wiki/Objeto_mock){:target="_blank"} nos testes unitários, que são mecanismos que podem imitar o comportamento de dependências no código.
{: style="text-align: justify;"}

Assim, teremos que fazer uma refatoração em nosso código de produção para tratar a dependência direta do banco de dados, onde a melhor forma de fazer isso aqui será criar uma interface `IEmployeeRepository` representando um repositório de dados com um método para adicionar os registros.
{: style="text-align: justify;"}

```csharp
using System.Collections.Generic;

namespace FileReader
{
    public interface IEmployeeRepository
    {
        void AddAll(List<Employee> employees);
    }
}
```

E criamos também a sua implementação `MySqlEmployeeRepository`, mas que não será utilizada nos testes.
{: style="text-align: justify;"}

```csharp
using MySqlConnector;
using System.Collections.Generic;

namespace FileReader
{
    public class MySqlEmployeeRepository : IEmployeeRepository
    {
        public void AddAll(List<Employee> employees)
        {
            using (var databaseConnection = CreateDatabaseConnection())
            {
                databaseConnection.Open();

                var connectionTransaction = databaseConnection.BeginTransaction();

                foreach (var employee in employees)
                {
                    using (var databaseCommand = databaseConnection.CreateCommand())
                    {   
                        databaseCommand.Transaction = connectionTransaction;
                        
                        databaseCommand.CommandText = @"INSERT INTO employees (id, name, gender, phone, hire_date, import_date) VALUES (@id, @name, @gender, @phone, @hire_date, @import_date)";
                        databaseCommand.Parameters.AddWithValue("@id", employee.Id);
                        databaseCommand.Parameters.AddWithValue("@name", employee.Name);
                        databaseCommand.Parameters.AddWithValue("@gender", employee.Gender);
                        databaseCommand.Parameters.AddWithValue("@phone", employee.Phone);
                        databaseCommand.Parameters.AddWithValue("@hire_date", employee.HireDate);
                        databaseCommand.Parameters.AddWithValue("@import_date", employee.ImportDate);

                        databaseCommand.ExecuteNonQuery();
                    }
                }
                
                connectionTransaction.Commit();
            }
        }

        private MySqlConnection CreateDatabaseConnection()
        {
            var databaseConnectionBuilder = new MySqlConnectionStringBuilder
            {
                Server   = "localhost",
                Port     = 3306,
                Database = "company",
                UserID   = "company",
                Password = "123456",
            };

            return new MySqlConnection(databaseConnectionBuilder.ConnectionString);
        }
    }
}
```

E assim adicionamos a interface como dependência ao nosso `FileReader`.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using System.Collections.Generic;

namespace FileReader
{
    public class FileReader
    {
        private static readonly string DateFormat = "dd/MM/yyyy";
        private readonly List<Employee> employees = new List<Employee>();
        private readonly IEmployeeRepository employeeRepository;
        private string inputFile;

        public FileReader(IEmployeeRepository employeeRepository) {
            this.employeeRepository = employeeRepository;
        }

        public void Read(string inputFile)
        {
            this.inputFile = inputFile;

            ReadInputFile();
            ValidateData();
            ImportToDatabase();
        }

        //...

        private void ImportToDatabase()
        {
            Console.WriteLine("Importing to database");

            employeeRepository.AddAll(employees);
        }

        //...
    }
}
```

E agora com essa modificação temos nossa dependência de banco de dados isolada do nosso código principal, permitindo ser substituída por um `Mock` e testada com eficiência.
{: style="text-align: justify;"}

Outra coisa legal dessa refatoração é que essa adequação acaba atendendo também a dois princípios do [SOLID](https://pt.wikipedia.org/wiki/SOLID){:target="_blank"}, que é o **SRP** (princípio da responsabilidade única) que diz que uma classe deve ter somente uma responsabilidade, e o **DIP** (dependa de abstrações) o qual diz que uma classe deve depender sempre de abstrações, pois assim é possível trocar implementações sem danos ao código, provendo a famosa inversão de controle.
{: style="text-align: justify;"}

Podemos agora finalizar a escrita de testes.
{: style="text-align: justify;"}

## Finalizando os testes

Para escrever os últimos testes iremos usar a lib [Moq](https://github.com/Moq/moq4/wiki/Quickstart){:target="_blank"} que é um dos mecanismos disponíveis para `Mock` no C#, e usaremos um `Mock` para substituir o acesso ao banco de dados por meio da interface `IEmployeeRepository`, e o teste será verificar chamadas ao método `AddAll` da interface.
{: style="text-align: justify;"}

### Ler dados com somente um registro e inserir no banco

Esse teste irá ler o arquivo `OneItemFile.txt`, validar as informações e inserir no repositório de dados, onde iremos verificar se o método `AddAll` da interface foi chamado somente uma vez com uma lista contendo somente um item.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_AddOneItemToDatabase_When_ReadFileWithOneItem()
{
    var employeeRepositoryMock = new Mock<IEmployeeRepository>();
    var fileReader = new FileReader(employeeRepositoryMock.Object);

    fileReader.Read("./Resources/OneItemFile.txt");

    employeeRepositoryMock.Verify(c => c.AddAll(It.Is<List<Employee>>(l => l.Count == 1)), Times.Once());
}
```

Arquivo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;10/02/2019

### Ler dados com mais de um registro e inserir no banco

Esse teste irá ler o arquivo `MultipleItemsFile.txt`, validar as informações e inserir no repositório de dados, onde iremos verificar se o método `AddAll` da interface foi chamado somente uma vez com uma lista contendo quatro itens.
{: style="text-align: justify;"}

```csharp
[Fact]
public void Should_AddFourItemsToDatabase_When_ReadFileWithFourItems()
{
    var employeeRepositoryMock = new Mock<IEmployeeRepository>();
    var fileReader = new FileReader(employeeRepositoryMock.Object);

    fileReader.Read("./Resources/MultipleItemsFile.txt");

    employeeRepositoryMock.Verify(c => c.AddAll(It.Is<List<Employee>>(l => l.Count == 4)), Times.Once());
}
```

Arquivo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;10/02/2019
    001302;PRISCILLA NOVAES LEONE;F;7111223344;22/04/2020
    001303;CASSIA REJANE ELLER;F;2111112222;30/07/2019
    001304;RENATO MANFREDINI JUNIOR;M;61135724685;13/08/2019

E assim conseguimos terminar todos nossos testes unitários.
{: style="text-align: justify;"}

## Indo além

Com os testes finalizados podemos ver que existem outras melhorias que podem ser adicionadas também na aplicação, onde um exemplo é a leitura de dados que poderia ser separada em uma outra classe e utilizada como dependência do mesmo jeito que fizemos com o acesso ao banco de dados, o que ficaria mais aderente ao princípio **SRP** comentado anteriormente. Outro ponto também seriam os testes da classe `MySqlEmployeeRepository` que não foram escritos pois é impossível criar um teste unitário para ela, já que está altamente acoplada ao banco de dados, porém isso não significa que não é possível ter outro tipo de teste aqui, onde podemos criar um teste de integração para manipulação do banco.
{: style="text-align: justify;"}

Porém não vou abordar esses assuntos aqui para que o post não fique maior do que já ficou rs, então deixo esses pontos para vocês tentarem.
{: style="text-align: justify;"}

O código completo pode ser encontrado [aqui](https://github.com/murilo-ramos/murilo-tech/tree/master/FileReaderTest){:target="_blank"}, e nos vemos na próxima!
{: style="text-align: justify;"}
