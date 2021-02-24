---
title: Criando um JAR com dependências usando o Maven
author: Murilo Costa
date: 2021-02-24 12:30:00 -0300
categories: [Blogging, Java]
tags: [java, maven, build, dependencia, dependency, pom]
---

É incrível como o [Maven](https://maven.apache.org/){:target="_blank"}, o gerenciador automático de dependências e build, lançado a dezesseis anos atrás ainda se mantém como uma das principais ferramentas do ecossistema Java, mesmo após o lançamento do [Gradle](https://gradle.org/){:target="_blank"}, seu primo mais novo que veio com a promessa de solucionar problemas que o Maven falhou em resolver.
{: style="text-align: justify;"}

Mas também não é pra menos, pois o Maven é uma das ferramentas mais intuitivas e fáceis de se usar que conheço, que por utilizar o modelo 'convenção sobre configuração' facilita muito o trabalho do dia a dia diminuindo as escolhas que o desenvolvedor precisa fazer, proporcionando um ganho de tempo importante.
{: style="text-align: justify;"}

E algo que os usuários de Maven já tiveram que procurar na internet pelo menos uma vez na vida é como gerar um pacote jar com dependências, pois esse é um ponto que o Maven é falho e não oferece de forma simples.
{: style="text-align: justify;"}

Por isso resolvi trazer alguns métodos e exemplos de como atingir esse objetivo usando o Maven, através de plugins.
{: style="text-align: justify;"}

## Criando um JAR

Como exemplo vamos um projeto bem simples, que somente lista os repositórios no [Github de um determinado usuário](https://docs.github.com/en/rest/reference/repos#list-repositories-for-a-user){:target="_blank"}.
{: style="text-align: justify;"}

O diferencial do projeto serão as dependências que ele utiliza (libs do [Retrofit](https://square.github.io/retrofit/){:target="_blank"}, usada para realizar requisições REST de forma fácil), que iremos colocar junto no build do Maven.
{: style="text-align: justify;"}

O projeto pode ser encontrado [aqui](https://github.com/murilo-ramos/murilo-tech/tree/master/maven-dependency/github-user-repositories){:target="_blank"}, e sua execução é feita com o comando `java -jar pacotejar.jar usuario-github`.
{: style="text-align: justify;"}

E este é o arquivo pom.xml.
{: style="text-align: justify;"}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>tech.murilo</groupId>
    <artifactId>github-user-repositories</artifactId>
    <version>1.0.0</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.squareup.retrofit2</groupId>
            <artifactId>retrofit</artifactId>
            <version>2.7.2</version>
        </dependency>
        <dependency>
            <groupId>com.squareup.retrofit2</groupId>
            <artifactId>converter-jackson</artifactId>
            <version>2.7.2</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
                        </manifest>
                    </archive>
                    <finalName>github-user-repositories</finalName>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

Vamos explicar alguns pontos importantes desse arquivo.
{: style="text-align: justify;"}

```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

Essas são as propriedades utilizadas pelo Maven no projeto, que especificam que a versão do Java utilizada é a 11 tanto a nível de compilação (`source`) quanto de execução (`target`), e os arquivos serão interpretados com o encoding UTF-8 (`sourceEncoding`), algo que é importante se existem textos no seu código com caracteres especiais.
{: style="text-align: justify;"}

```xml
<dependencies>
    <dependency>
        <groupId>com.squareup.retrofit2</groupId>
        <artifactId>retrofit</artifactId>
        <version>2.7.2</version>
    </dependency>
    <dependency>
        <groupId>com.squareup.retrofit2</groupId>
        <artifactId>converter-jackson</artifactId>
        <version>2.7.2</version>
    </dependency>
</dependencies>
```

Essas são as dependências do projeto considerando também as subdependências, que se traduzem em sete arquivos jar.
{: style="text-align: justify;"}

- retrofit-2.7.2.jar
- okhttp-3.14.7.jar
- okio-1.17.2.jar
- converter-jackson-2.7.2.jar
- jackson-annotations-2.10.1.jar
- jackson-core-2.10.1.jar
- jackson-databind-2.10.1.jar

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
            </manifest>
        </archive>
        <finalName>github-user-repositories</finalName>
    </configuration>
</plugin>
```

Aqui é a configuração de build que é o que importa no assunto do post. As principais informações nessa configuração são o plugin `maven-jar-plugin`, que auxilia na criação do pacote jar, as tags `mainClass` que define a classe que será executada ao se executar o comando `java -jar meuarquivo.jar` e a `finalName` que indica o nome do arquivo que será gerado, pois caso contrário o Maven por padrão coloca o nome do artefato + versão, o que no nosso caso ficaria algo como `github-user-repositories-1.0.0.jar`, interessante somente para bibliotecas e não para arquivos executáveis (runnable jar).
{: style="text-align: justify;"}

Ao rodar o comando `mvn clean package` o maven compila e gera o pacote na pasta `target`, tendo o seguinte resultado.
{: style="text-align: justify;"}

![target maven-jar-plugin](/assets/img/posts/criando-jar-com-dependencias-usando-maven/maven-jar-plugin.png)

E agora finalmente podemos executar a aplicação com o comando `java -jar target/github-user-repositories.jar murilo-ramos`.
{: style="text-align: justify;"}

    $ java -jar target/github-user-repositories.jar murilo-ramos
Exception in thread "main" java.lang.NoClassDefFoundError: retrofit2/Converter$Factory
	at tech.murilo.githubuserrepositories.main.Main.main(Main.java:17)
Caused by: java.lang.ClassNotFoundException: retrofit2.Converter$Factory
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:581)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
	... 1 more

Assim podemos ver que a aplicação falhou apresentando um `NoClassDefFoundError`, erro clássico indicando que alguma classe está faltando no sistema, que no nosso caso são as dependências.
{: style="text-align: justify;"}

## JAR com dependências em diretório externo (maven-dependency-plugin)

A primeira opção que vou apresentar aqui é gerar o pacote com as dependências em um diretório (ou pasta, se preferir chamar assim) adicional, que pode ser gerado automaticamente pelo Maven.
{: style="text-align: justify;"}

Essa opção é bem interessante para aqueles que queiram distribuir a aplicação em partes, podendo prover somente o pacote principal e as dependências que foram alteradas, não sendo necessário prover um pacote completo toda vez que uma alteração é realizada na aplicação.
{: style="text-align: justify;"}

Aqui podemos usar um plugin do maven chamado `maven-dependency-plugin` que em conjunto com o `maven-jar-plugin` consegue gerar o pacote com as dependências.
{: style="text-align: justify;"}

```xml
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>3.1.2</version>
        <executions>
            <execution>
                <id>copy-dependencies</id>
                <phase>prepare-package</phase>
                <goals>
                    <goal>copy-dependencies</goal>
                </goals>
                <configuration>
                    <outputDirectory>${project.build.directory}/lib</outputDirectory>
                </configuration>
            </execution>
        </executions>
    </plugin>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.2.0</version>
        <configuration>
            <archive>
                <manifest>
                    <addClasspath>true</addClasspath>
                    <classpathPrefix>lib</classpathPrefix>
                    <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
                </manifest>
            </archive>
            <outputDirectory>${project.build.directory}</outputDirectory>
            <finalName>github-user-repositories</finalName>
        </configuration>
    </plugin>
</plugins>
```

As principais informações nessa configuração são a tag de `configuration/outputDirectory` no plugin `maven-dependency-plugin` que direciona as dependências para uma pasta chamada `lib`, e as configurações de `manifest` presentes no plugin `maven-jar-plugin` que dizem ao pacote que nossas dependências estarão na pasta `lib` através de uma configuração de `classpath`.
{: style="text-align: justify;"}

Agora ao rodar o comando `mvn clean package` veremos nosso pacote na raiz da pasta `target` e as dependências na pasta `lib`.
{: style="text-align: justify;"}

![target maven-dependency-plugin](/assets/img/posts/criando-jar-com-dependencias-usando-maven/maven-dependency-plugin-01.png)

Porém desta forma eu acredito que fica um pouco 'bagunçado' pois os arquivos são colocados junto aos outros arquivos produzidos pelo Maven, o que dificulta a visualização, por isso prefiro direcionar a saída para uma subpasta, algo que é super simples de se fazer bastando somente adicionar a subpasta na configuração de `outputDirectory`. Vamos chamar essa subpasta de `dist`.
{: style="text-align: justify;"}

```xml
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>3.1.2</version>
        <executions>
            <execution>
                <id>copy-dependencies</id>
                <phase>prepare-package</phase>
                <goals>
                    <goal>copy-dependencies</goal>
                </goals>
                <configuration>
                    <outputDirectory>${project.build.directory}/dist/lib</outputDirectory>
                </configuration>
            </execution>
        </executions>
    </plugin>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.2.0</version>
        <configuration>
            <archive>
                <manifest>
                    <addClasspath>true</addClasspath>
                    <classpathPrefix>lib</classpathPrefix>
                    <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
                </manifest>
            </archive>
            <outputDirectory>${project.build.directory}/dist</outputDirectory>
            <finalName>github-user-repositories</finalName>
        </configuration>
    </plugin>
</plugins>
```

![target maven-dependency-plugin dist](/assets/img/posts/criando-jar-com-dependencias-usando-maven/maven-dependency-plugin-02.png)

E finalmente conseguimos executar a aplicação com sucesso.
{: style="text-align: justify;"}

    $ java -jar target/dist/github-user-repositories.jar murilo-ramos
    Repositórios de murilo-ramos:

    Nome: alura-imersao-react, Forks: 0, Estrelas: 0
    Nome: alura-jquery, Forks: 0, Estrelas: 0
    Nome: alura-springmvc, Forks: 0, Estrelas: 0
    Nome: byte-string-format, Forks: 0, Estrelas: 0
    Nome: CameraAndroid, Forks: 0, Estrelas: 0
    Nome: curso-go, Forks: 0, Estrelas: 0
    ...

## JAR com dependências em um único pacote (onejar-maven-plugin)

Se você preferir gerar um pacote contendo todas as dependências isso também é possível através do plugin `onejar-maven-plugin`, que gera um pacote executável contendo o jar de nossa aplicação e as dependências em uma estrutura própria de execução.
{: style="text-align: justify;"}

```xml
<plugin>
    <groupId>com.jolira</groupId>
    <artifactId>onejar-maven-plugin</artifactId>
    <version>1.4.4</version>
    <executions>
        <execution>
            <configuration>
                <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
                <filename>github-user-repositories.jar</filename>
            </configuration>
            <goals>
                <goal>one-jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

No caso desse plugin a única configuração diferenciada é a definição do nome do arquivo, que nesse caso é a tag `filename` devendo receber também a extensão do arquivo a ser produzido.
{: style="text-align: justify;"}

Executando o `mvn clean package` temos o resultado.
{: style="text-align: justify;"}

![target onejar-maven-plugin](/assets/img/posts/criando-jar-com-dependencias-usando-maven/onejar-maven-plugin.png)

Observe que a geração do pacote por esse plugin não remove o arquivo original `github-user-repositories-1.0.0.jar`, mas não devemos nos preocupar com isso e podemos desconsiderar esse arquivo. Isso irá acontecer também nas estratégias posteriores.
{: style="text-align: justify;"}

    $ java -jar target/github-user-repositories.jar murilo-ramos
    JarClassLoader: Warning: module-info.class in lib/jackson-annotations-2.10.1.jar is hidden by lib/jackson-databind-2.10.1.jar (with different bytecode)
    JarClassLoader: Warning: module-info.class in lib/jackson-core-2.10.1.jar is hidden by lib/jackson-databind-2.10.1.jar (with different bytecode)
    Repositórios de murilo-ramos:

    Nome: alura-imersao-react, Forks: 0, Estrelas: 0
    Nome: alura-jquery, Forks: 0, Estrelas: 0
    Nome: alura-springmvc, Forks: 0, Estrelas: 0
    Nome: byte-string-format, Forks: 0, Estrelas: 0
    Nome: CameraAndroid, Forks: 0, Estrelas: 0
    Nome: curso-go, Forks: 0, Estrelas: 0
    ...

A maior vantagem desse plugin em relação aos outros que veremos adiante é que ele mantém de forma íntegra as dependências utilizadas e não sobrescreve nenhum arquivo no processo de construção do pacote.
{: style="text-align: justify;"}

Infelizmente sua desvantagem é que parece ter sido abandonado pela equipe de desenvolvimento, pois não recebe mais atualizações [desde o final de 2011](https://mvnrepository.com/artifact/com.jolira/onejar-maven-plugin/1.4.4){:target="_blank"}.
{: style="text-align: justify;"}

## JAR com dependências em um único pacote (maven-assembly-plugin)

A segunda alternativa para gerar um único pacote é o famoso `maven-assembly-plugin`.
{: style="text-align: justify;"}

Este plugin consegue também gerar um pacote de nossa aplicação incluindo as dependências, porém com a diferença que as dependências são descompactadas e agregadas ao pacote, fazendo com que a aplicação se torne uma coisa só.
{: style="text-align: justify;"}

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>3.3.0</version>
    <configuration>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
        <finalName>github-user-repositories</finalName>
        <appendAssemblyId>false</appendAssemblyId>
        <archive>
            <manifest>
                <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
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
```

As configurações mais importantes aqui são a tag `descriptorRefs/descriptorRef` que indica qual mecanismo será utilizado para criar o pacote, onde estamos usando o padrão e pré-definido `jar-with-dependencies` para gerar o pacote com as dependências, e a tag `appendAssemblyId` que setamos para `false` pois caso contrário o pacote gerado ficará com o nome padrão, incluindo a versão no nome do arquivo.
{: style="text-align: justify;"}

Executando o `mvn clean package` obtemos o arquivo.
{: style="text-align: justify;"}

![target maven-assembly-plugin](/assets/img/posts/criando-jar-com-dependencias-usando-maven/maven-assembly-plugin.png)

    $ java -jar target/github-user-repositories.jar murilo-ramos
    Repositórios de murilo-ramos:

    Nome: alura-imersao-react, Forks: 0, Estrelas: 0
    Nome: alura-jquery, Forks: 0, Estrelas: 0
    Nome: alura-springmvc, Forks: 0, Estrelas: 0
    Nome: byte-string-format, Forks: 0, Estrelas: 0
    Nome: CameraAndroid, Forks: 0, Estrelas: 0
    Nome: curso-go, Forks: 0, Estrelas: 0
    ...

Esse plugin, no entanto, é recomendável somente para projetos médios e pequenos, pois ele pode provocar conflitos no caso de classes com o mesmo nome em dependências distintas, e também ocorre sobrescrita de arquivos de mesmo nome, onde configurações podem ser perdidas se isso ocorrer dentro da aplicação e dependências (principalmente com arquivos properties).
{: style="text-align: justify;"}

Para resolver esse problema temos o shade, o próximo da lista.
{: style="text-align: justify;"}

## JAR com dependências em um único pacote (maven-shade-plugin)

E para finalizar, o último método e plugin utilizado será o `maven-shade-plugin`, que basicamente produz um resultado bem parecido com o plugin anterior, mas possui algumas opções e configurações a mais como é o caso da [realocação da classes](http://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html){:target="_blank"}.
{: style="text-align: justify;"}

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.2.4</version>
    <executions>
        <execution>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>tech.murilo.githubuserrepositories.main.Main</mainClass>
                    </transformer>
                </transformers>
                <finalName>github-user-repositories</finalName>
            </configuration>
            <goals>
                <goal>shade</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

As configurações aqui não fogem muito do que já foi visto, com exceção da tag específica `transformers/transformer` onde é definida a estratégia para criação do pacote. Neste caso estamos usando o `ManifestResourceTransformer` que adiciona entradas no arquivo de manifesto. Outras opções podem ser vistas nesse [link](https://maven.apache.org/plugins/maven-shade-plugin/examples/resource-transformers.html){:target="_blank"}.
{: style="text-align: justify;"}

Após um `mvn clean package` temos os arquivos.
{: style="text-align: justify;"}

![target maven-shade-plugin](/assets/img/posts/criando-jar-com-dependencias-usando-maven/maven-shade-plugin.png)

    $ java -jar target/github-user-repositories.jar murilo-ramos
    Repositórios de murilo-ramos:

    Nome: alura-imersao-react, Forks: 0, Estrelas: 0
    Nome: alura-jquery, Forks: 0, Estrelas: 0
    Nome: alura-springmvc, Forks: 0, Estrelas: 0
    Nome: byte-string-format, Forks: 0, Estrelas: 0
    Nome: CameraAndroid, Forks: 0, Estrelas: 0
    Nome: curso-go, Forks: 0, Estrelas: 0
    ...


E é isso!
{: style="text-align: justify;"}

Ficou com alguma dúvida ou conhece alguma outra forma de gerar um JAR com dependências? Compartilhe comigo!
{: style="text-align: justify;"}

O código completo pode ser encontrado no [Github](https://github.com/murilo-ramos/murilo-tech/tree/master/maven-dependency){:target="_blank"}.
{: style="text-align: justify;"}