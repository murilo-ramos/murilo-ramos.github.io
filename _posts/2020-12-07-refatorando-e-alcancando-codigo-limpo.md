---
title: Refatorando e alcançando um código limpo
author: Murilo Costa
date: 2020-12-07 19:20:00 -0300
categories: [Blogging, C#]
tags: [refatorando, refatoracao, clean-code, codigo-limpo, c#, dotnet, mysql]
---

Acredito que muitos já devem ter observado que eu sou um praticante de [código e design de código limpos](https://www.hostgator.com.br/blog/clean-code-o-que-e/){:target="_blank"}, que são um conjunto de práticas e padrões para tornar o código mais legível e simples, melhorando seu entendimento e manutenção.
{: style="text-align: justify;"}

Existe bastante debate sobre esse assunto onde muitos se perguntam se realmente essas práticas agregam algum valor ao software desenvolvido, visto que quando seguimos metodologias de desenvolvimento é sempre o cliente/usuário que deve ser priorizado, e para eles o importante é o software estar funcionando.
{: style="text-align: justify;"}

![Meu codigo roda 9gag](/assets/img/posts/refatorando-e-alcancando-codigo-limpo/my-code-runs.png)

*Imagem disponibilizada gentilmente pelo [9gag](https://9gag.com/gag/an9LbjL){:target="_blank"}.*


Porém, baseado em minha experiência e em tudo que já vi por aí preciso dizer que o código simples, bem arquitetado e legível é de extrema importância no desenvolvimento de software, pois existe uma coisa que todos os softwares possuem em comum: **A mudança!**
{: style="text-align: justify;"}

São extremamente raros os softwares em que se desenvolve apenas uma vez e nunca mais ocorre uma manutenção ou atualização, e aqui fica explícito que em qualquer manutenção ou atualização vai ser necessário que alguém leia e altere o código existente, onde começa o problema pois se o software tiver sido mal escrito será bem difícil realizar a devida mudança nesse código, o que geralmente se traduz em prazos de desenvolvimento muito maiores do que o necessário.
{: style="text-align: justify;"}

Por isso resolvi compartilhar umas dicas de refatoração para melhorar a legibilidade e design de códigos 'não muito bem escritos' que podem transformá-los em algo que possa ser facilmente entendido por qualquer desenvolvedor.
{: style="text-align: justify;"}

## **O Exemplo**

Vamos usar como projeto de exemplo uma aplicação simples em C# (plataforma .Net Core) que tem o seguinte objetivo:
{: style="text-align: justify;"}

### 1) Ler dados contendo informações de funcionários de uma empresa, em formato texto

O arquivo será um csv simples separado por ponto e vírgula, possuindo os campos ID, Nome, Gênero, Telefone e Data de Contratação.
{: style="text-align: justify;"}

Exemplo:

    001301;AGENOR DE MIRANDA ARAUJO NETO;NI;2112345678;10/02/2019
    001302;PRISCILLA NOVAES LEONE;F;7111223344;22/04/2020
    001303;CASSIA REJANE ELLER;F;2111112222;30/07/2019
    001304;RENATO MANFREDINI JUNIOR;M;61135724685;13/08/2019

### 2) Validar os dados lidos

As regras de validação são somente duas: 1) O arquivo precisa ter todos os IDs começando com '0013'; 2) A data de contratação não pode ser menor que 01/01/2019.
{: style="text-align: justify;"}

Caso algum valor não esteja de acordo com a validação, toda a importação deve ser abortada.
{: style="text-align: justify;"}

### 3) Carregar em um banco de dados

A importação dos dados deve ser feita em um banco de dados MySQL chamado `company`, na tabela `employees`.
{: style="text-align: justify;"}

```sql
CREATE TABLE `employees` (
  `id`     varchar(100) NOT NULL,
  `name`   varchar(255) DEFAULT NULL,
  `gender` varchar(255) DEFAULT NULL,
  `phone`  varchar(255) DEFAULT NULL,
  `hire_date`  datetime DEFAULT NULL,
  `import_date` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

Coisa simples, não é? Parece ser meio difícil termos um código ruim em uma aplicação simples como essa.
{: style="text-align: justify;"}

Então vamos ao código.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using MySqlConnector;

namespace FileReader
{
    public class FileReader
    {
        public void execute(string f) {

            MySqlConnectionStringBuilder builder = new MySqlConnectionStringBuilder();
            builder.Server = "localhost";
             builder.Port = 3306;
             builder.Database = "company";
             builder.UserID = "company";
             builder.Password = "123456";
            

            using (var conn = new MySqlConnection(builder.ConnectionString)) {
                Console.WriteLine("Opening connection");
                conn.Open();

                    var trans = conn.BeginTransaction();

                using (var readFile = new StreamReader(f))
                {
                    string line;
                      while((line = readFile.ReadLine()) != null)  
                    {
                        if (string.IsNullOrEmpty(line))
                          continue;
                        

                        //lendo e validando
                        string[] campos = line.Split(";");
                        DateTime data;
                        string gender;

                        if (!campos[0].StartsWith("0013")) throw new Exception("ID não começa com 0013: " + campos[0]);
                        

                        switch (campos[2]) {
                            case "M":
                            gender = "Masculino";
                            break;
                            case "F":
                            gender = "Feminino";
                            break;
                            case "NI":
                            gender = "Não Informado";
                            break;
                            default:
                                throw new Exception("Gênero desconhecido: " + campos[2]);
                        }

                        data = DateTime.ParseExact(campos[4], "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture);

                        if (data < DateTime.ParseExact("01/01/2019", "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture))
                        throw new Exception("Data de importação não pode ser menor que 01/01/2019: " + campos[4]);


                        using (MySqlCommand command = conn.CreateCommand()) {   
                            command.Transaction = trans; 
                            command.CommandText = @"INSERT INTO employees (id, name, gender, phone, hire_date, import_date) VALUES (@id, @name, @gender, @phone, @hire_date, @import_date)";
                            command.Parameters.AddWithValue("@id", campos[0]);
                            command.Parameters.AddWithValue("@name", campos[1]);
                            command.Parameters.AddWithValue("@gender", gender);
                            command.Parameters.AddWithValue("@phone", campos[3]);
                            command.Parameters.AddWithValue("@hire_date", data);
                            command.Parameters.AddWithValue("@import_date", DateTime.Now);
                            command.ExecuteNonQuery();
                        }
                    }  
                }

                // connection will be closed by the 'using' block
                Console.WriteLine("Closing connection");
                trans.Commit();
            }
        }
    }
}
```

Nunca subestime a chance de um código ser mal escrito rs.
{: style="text-align: justify;"}

## **Refatorando**

A primeira coisa que podemos fazer nesse código é aplicar pequenas correções no estilo de código, como corrigir a identação (isso é básico, mas infelizmente muitos não ligam rs), deixar o abre/fecha chaves no padrão da linguagem e organizar o código em 'blocos de contexto', para facilitar a identificação de onde estão ocorrendo os processos 1,2 e 3 que especificamos no projeto.
{: style="text-align: justify;"}

Por enquanto vou colocar comentários no código para facilitar a visualização dos blocos.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using MySqlConnector;

namespace FileReader
{
    public class FileReader
    {
        public void execute(string f)
        {
            MySqlConnectionStringBuilder builder = new MySqlConnectionStringBuilder();
            builder.Server = "localhost";
            builder.Port = 3306;
            builder.Database = "company";
            builder.UserID = "company";
            builder.Password = "123456";
            
            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                conn.Open();
                var trans = conn.BeginTransaction();

                using (var readFile = new StreamReader(f))
                {
                    string line;
                    while((line = readFile.ReadLine()) != null)  
                    {
                        if (string.IsNullOrEmpty(line))
                          continue;

                        DateTime data;
                        string gender;

                        //1) Lendo dados
                        string[] campos = line.Split(";");

                        switch (campos[2])
                        {
                            case "M":
                                gender = "Masculino";
                                break;
                            case "F":
                                gender = "Feminino";
                                break;
                            case "NI":
                                gender = "Não Informado";
                                break;
                            default:
                                throw new Exception("Gênero desconhecido: " + campos[2]);
                        }

                        data = DateTime.ParseExact(campos[4], "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture);

                        //2) Validando dados
                        if (!campos[0].StartsWith("0013"))
                            throw new Exception("ID não começa com 0013: " + campos[0]);

                        if (data < DateTime.ParseExact("01/01/2019", "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture))
                            throw new Exception("Data de importação não pode ser menor que 01/01/2019: " + campos[4]);

                        
                        //3) Inserindo dados no banco
                        using (MySqlCommand command = conn.CreateCommand())
                        {   
                            command.Transaction = trans; 
                            command.CommandText = @"INSERT INTO employees (id, name, gender, phone, hire_date, import_date) VALUES (@id, @name, @gender, @phone, @hire_date, @import_date)";
                            command.Parameters.AddWithValue("@id", campos[0]);
                            command.Parameters.AddWithValue("@name", campos[1]);
                            command.Parameters.AddWithValue("@gender", gender);
                            command.Parameters.AddWithValue("@phone", campos[3]);
                            command.Parameters.AddWithValue("@hire_date", data);
                            command.Parameters.AddWithValue("@import_date", DateTime.Now);
                            command.ExecuteNonQuery();
                        }
                    }  
                }

                // connection will be closed by the 'using' block
                Console.WriteLine("Closing connection");
                trans.Commit();
            }
        }
    }
}
```

Só nessa mudança já podemos perceber que ficou bem mais fácil de entender o código, porém ainda temos muito a fazer.
{: style="text-align: justify;"}

Podemos agora melhorar os nomes de variáveis que estávamos usando, pois podemos perceber que o código não está usando nomes muito semânticos, como `f` para o arquivo de input e `data` para a data de contração de um funcionário, sem contar que temos uma mistura de nomes em inglês e português, o que também fica esquisito sendo o mais correto padronizar em uma língua só: tudo em inglês ou tudo em português.
{: style="text-align: justify;"}

Outra melhoria está também no nome do método da classe, que está muito genérico e pode ser melhorado. Aqui podemos também usar recursos da linguagem para simplificar o código, como a construção de objetos usando o padrão de `Properties` do C# e também a inferência por tipo usando o `var`, que diminui bastante a quantidade de código repetido. E por último vamos adicionar chaves também nos blocos `if` que possuem somente uma instrução, pois isso ajuda na legibilidade e facilita a separação de código.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using MySqlConnector;

namespace FileReader
{
    public class FileReader
    {
        public void read(string inputFile)
        {
            var databaseConnectionBuilder = new MySqlConnectionStringBuilder
            {
                Server   = "localhost",
                Port     = 3306,
                Database = "company",
                UserID   = "company",
                Password = "123456",
            };
            
            using (var databaseConnection = new MySqlConnection(databaseConnectionBuilder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                databaseConnection.Open();

                var connectionTransaction = databaseConnection.BeginTransaction();

                using (var fileReader = new StreamReader(inputFile))
                {
                    string fileLine;
                    while((fileLine = fileReader.ReadLine()) != null)  
                    {
                        if (string.IsNullOrEmpty(fileLine))
                        {
                          continue;
                        }

                        //1) Lendo dados
                        var fields = fileLine.Split(";");

                        var id       = fields[0];
                        var name     = fields[1];
                        var gender   = fields[2];
                        var phone    = fields[3];
                        var hireDate   = DateTime.ParseExact(fields[4], "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture);
                        var importDate = DateTime.Now;

                        switch (gender)
                        {
                            case "M":
                                gender = "Masculino";
                                break;
                            case "F":
                                gender = "Feminino";
                                break;
                            case "NI":
                                gender = "Não Informado";
                                break;
                            default:
                                throw new Exception("Gênero desconhecido: " + gender);
                        }

                        //2) Validando dados
                        if (!id.StartsWith("0013")) 
                        {
                            throw new Exception("ID não começa com 0013: " + id);
                        }

                        if (hireDate < DateTime.ParseExact("01/01/2019", "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture))
                        {
                            throw new Exception("Data de importação não pode ser menor que 01/01/2019: " + hireDate);
                        }
                        
                        //3) Inserindo dados no banco
                        using (var databaseCommand = databaseConnection.CreateCommand())
                        {   
                            databaseCommand.Transaction = connectionTransaction;
                            
                            databaseCommand.CommandText = @"INSERT INTO employees (id, name, gender, phone, hire_date, import_date) VALUES (@id, @name, @gender, @phone, @hire_date, @import_date)";
                            databaseCommand.Parameters.AddWithValue("@id", id);
                            databaseCommand.Parameters.AddWithValue("@name", name);
                            databaseCommand.Parameters.AddWithValue("@gender", gender);
                            databaseCommand.Parameters.AddWithValue("@phone", phone);
                            databaseCommand.Parameters.AddWithValue("@hire_date", hireDate);
                            databaseCommand.Parameters.AddWithValue("@import_date", importDate);

                            databaseCommand.ExecuteNonQuery();
                        }
                    }  
                }

                // connection will be closed by the 'using' block
                Console.WriteLine("Closing connection");
                connectionTransaction.Commit();
            }
        }
    }
}
```

Muito melhor, não é mesmo? Podemos observar que agora o código está muito mais legível com blocos organizados, cada variável com um nome que permite entender seu propósito e existe uma lógica na sequência de comandos que está de acordo com os processos que definimos anteriormente.
{: style="text-align: justify;"}

Mas ainda temos um problema nesse código: **Ele está muito grande!**
{: style="text-align: justify;"}

Temos um método de 85 linhas nesse código, e se olharmos os padrões do código limpo vamos ver que isso não é nada legal. Geralmente códigos com muitas linhas acabam fazendo muita coisa em um lugar só, e isso atrapalha muito em sua leitura pois não queremos ver o que um determinado código faz *linha-a-linha*, e sim queremos entender o `macro`, que no nosso caso é  'ler os dados', 'validar os dados' e 'inserir os dados', e somente se necessário olhamos mais a fundo.
{: style="text-align: justify;"}

E para alcançar esse resultado podemos usar aqui a famosa tática de 'dividir para conquistar', onde vamos separar esse nosso método em métodos menores.
{: style="text-align: justify;"}

Vamos então quebrá-lo em três métodos privados que serão chamados no principal. Repare que teremos que criar atributos a nível de classe para serem compartilhados.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using MySqlConnector;
using System.Collections.Generic;
using System.Dynamic;

namespace FileReader
{
    public class FileReader
    {
        private readonly List<dynamic> employees = new List<dynamic>();
        private string inputFile;

        public void read(string inputFile)
        {
            this.inputFile = inputFile;

            readInputFile();
            validateData();
            importToDatabase();
        }

        private void readInputFile()
        {
            using (var fileReader = new StreamReader(inputFile))
            {
                string fileLine;
                while((fileLine = fileReader.ReadLine()) != null)  
                {
                    if (string.IsNullOrEmpty(fileLine))
                    {
                        continue;
                    }
                    
                    var fields = fileLine.Split(";");

                    dynamic employee = new ExpandoObject();
                    employee.Id     = fields[0];
                    employee.Name   = fields[1];
                    employee.Gender = fields[2];
                    employee.Phone  = fields[3];
                    employee.HireDate   = DateTime.ParseExact(fields[4], "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture);
                    employee.ImportDate = DateTime.Now;

                    switch (employee.Gender)
                    {
                        case "M":
                            employee.Gender = "Masculino";
                            break;
                        case "F":
                            employee.Gender = "Feminino";
                            break;
                        case "NI":
                            employee.Gender = "Não Informado";
                            break;
                        default:
                            throw new Exception("Gênero desconhecido: " + employee.Gender);
                    }

                    employees.Add(employee);
                }  
            }
        }

        private void validateData()
        {
            foreach (var employee in employees)
            {
                //Funcionarios que nao tem seu Id comecando com 0013 pertencem a empresas terceirizadas
                if (!employee.Id.StartsWith("0013")) 
                {
                    throw new Exception("ID não começa com 0013: " + employee.Id);
                }

                //Funcionarios que foram contratados antes de 01/01/2019 nao devem ser importados a mando do RH
                if (employee.HireDate < DateTime.ParseExact("01/01/2019", "dd/MM/yyyy", System.Globalization.CultureInfo.InvariantCulture))
                {
                    throw new Exception("Data de importação não pode ser menor que 01/01/2019: " + employee.HireDate);
                }
            }
        }

        private void importToDatabase()
        {
            var databaseConnectionBuilder = new MySqlConnectionStringBuilder
            {
                Server   = "localhost",
                Port     = 3306,
                Database = "company",
                UserID   = "company",
                Password = "123456",
            };
            
            using (var databaseConnection = new MySqlConnection(databaseConnectionBuilder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
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
                
                Console.WriteLine("Closing connection");
                connectionTransaction.Commit();
            }
        }
    }
}
```

Agora sim! Com esta técnica minimizamos em muito o trabalho de quem está lendo e tentando entender nosso código, pois ao abrir a classe já é possível entender o que está acontecendo no método principal, que é a leitura de dados (`readInputFile()`), validação de dados (`validateData()`) e importação de dados no banco (`importToDatabase()`).
{: style="text-align: justify;"}

Repare também que que criei uma estrutura de dados do tipo `List` para guardar os valores lidos, que armazena valor do tipo `dynamic` do C#. Eu usei esse tipo aqui somente para simplificar o código, caso contrário teria que criar uma outra classe para representar o funcionário, porém em projetos reais eu aconselho fortemente que esse tipo de classe seja criada.
{: style="text-align: justify;"}

Uma coisa legal de se reparar também nesse código é na sequência de declaração dos métodos, que segue exatamente a sequência em que são chamados: primeiro `readInputFile`, segundo `validateData` e por último `importToDatabase`. Essa é uma boa prática do código limpo que diz que um código deve ser escrito como um livro, onde as páginas correntes tem sua sequência nas páginas posteriores, assim como os códigos correntes devem vir antes dos códigos que são invocados. Além disso, o código limpo também diz que a sequência dos métodos e atributos deve sempre ser 'públicos' primeiro e depois 'privados', pois alguém que está lendo o código sempre terá conhecimento primeiro da parte pública pois é ela quem é acessada por outros códigos, e por isso deve sempre estar no topo.
{: style="text-align: justify;"}

E por último podemos ver também que quando o código é bem estruturado torna-se praticamente nula a necessidade de comentários no código, onde eles devem ser usados somente para explicações de regras de negócio, como acontece em nossa validação.
{: style="text-align: justify;"}

E para finalizar podemos fazer mais alguns ajustes para reaproveitar melhor o código, como no caso do parseamento de datas, e também melhorar os logs, que são informações essenciais para se entender problemas que ocorrem quando o software já está em produção.
{: style="text-align: justify;"}

```csharp
using System;
using System.IO;
using MySqlConnector;
using System.Collections.Generic;
using System.Dynamic;

namespace FileReader
{
    public class FileReader
    {
        private static readonly string DateFormat = "dd/MM/yyyy";
        private readonly List<dynamic> employees = new List<dynamic>();
        private string inputFile;

        public void read(string inputFile)
        {
            this.inputFile = inputFile;

            readInputFile();
            validateData();
            importToDatabase();
        }

        private void readInputFile()
        {
            Console.WriteLine("Reading input file");
            
            using (var fileReader = new StreamReader(inputFile))
            {
                string fileLine;
                while((fileLine = fileReader.ReadLine()) != null)  
                {
                    if (string.IsNullOrEmpty(fileLine))
                    {
                        continue;
                    }
                    
                    var fields = fileLine.Split(";");

                    dynamic employee = new ExpandoObject();
                    employee.Id     = fields[0];
                    employee.Name   = fields[1];
                    employee.Gender = getGender(fields[2]);
                    employee.Phone  = fields[3];
                    employee.HireDate   = parseDate(fields[4]);
                    employee.ImportDate = DateTime.Now;

                    employees.Add(employee);
                }  
            }
        }

        private void validateData()
        {
            Console.WriteLine("Validating data");

            var matchId = "0013";
            var minimumHireDate = parseDate("01/01/2019");

            foreach (var employee in employees)
            {
                //Funcionarios que nao tem seu Id comecando com 0013 pertencem a empresas terceirizadas
                if (!employee.Id.StartsWith(matchId)) 
                {
                    throw new Exception("ID não começa com 0013: " + employee.Id);
                }

                //Funcionarios que foram contratados antes de 01/01/2019 nao devem ser importados a mando do RH
                if (employee.HireDate < minimumHireDate)
                {
                    throw new Exception("Data de importação não pode ser menor que 01/01/2019: " + employee.HireDate);
                }
            }
        }

        private void importToDatabase()
        {
            Console.WriteLine("Importing to database");

            using (var databaseConnection = createDatabaseConnection())
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

        private string getGender(string gender)
        {
            switch (gender)
            {
                case "M":
                    return "Masculino";
                case "F":
                    return "Feminino";
                case "NI":
                    return "Não Informado";
                default:
                    throw new Exception("Gênero desconhecido: " + gender);
            }
        }

        private MySqlConnection createDatabaseConnection()
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

        private DateTime parseDate(string date)
        {
            return DateTime.ParseExact(date, DateFormat, System.Globalization.CultureInfo.InvariantCulture);
        }
    }
}
```

E terminamos :)

## **Ponto negativo**

O único ponto negativo dessa refatoração seria o fato de que agora ao invés de uma iteração sobre os dados passamos a ter três, pois temos a primeira que lê os dados, a segunda que valida e a terceira que importa pro banco, sendo que no código inicial tudo era feito só em uma iteração. Porém se formos observar mais de perto e levar o famoso [Big O](https://pt.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/big-o-notation){:target="_blank"} na prática, vamos ver que a complexidade desse código é O(3n) (três iterações), que de acordo com as [regras práticas da complexidade de algoritmos](https://medium.com/@estevestoni/iniciando-com-a-nota%C3%A7%C3%A3o-big-o-be996fa3b47b){:target="_blank"} ainda é O(n) (uma iteração), sendo igual ao algoritmo inicial. E para aqueles que devem estar pensando em uma possível otimização eu já adianto: [Otimização prematura é a raiz de todo mal](http://www.vidageek.net/performance-java/otimizacao-prematura/){:target="_blank"}.
{: style="text-align: justify;"}

Os códigos desse projeto, tanto o original como o refatorado (incluindo o uso de classe externa) podem ser encontrados [aqui](https://github.com/murilo-ramos/murilo-tech/tree/master/FileReader){:target="_blank"}.
{: style="text-align: justify;"}

Até a próxima.
