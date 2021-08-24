---
title: Criptografia
author: Murilo Costa
date: 2021-08-24 08:00:00 -0300
categories: [Blogging, Segurança]
tags: [criptografia, crypto, des, 3des, aes, rsa, hash, md5, assinatura, digital, digital, signature]
---

Durante uma parte da minha vida profissional eu estive trabalhando com uma tecnologia mais ou menos desconhecida para a maioria das pessoas, que são os [smart cards](https://pt.wikipedia.org/wiki/Cart%C3%A3o_inteligente){:target="_blank"}, conhecidos por serem os chips presentes nos cartões de banco, SIM cards de celular e por estarem presentes nos cartões usados para bater ponto no trabalho (quando ainda fazíamos isso antes do home-office rs).
{: style="text-align: justify;"}

E ao se trabalhar com smart cards (ou cartões inteligentes se você preferir em português) uma coisa que é muito utilizada são os mecanismos criptográficos, que são algo de extrema importância nesses cartões, e é basicamente por isso que eles ainda existem pois carregam chaves criptográficas que não podem ser removidas do chip, permitindo o dispositivo atuar como uma caixa preta e garantindo a segurança desejada.
{: style="text-align: justify;"}

E enquanto eu trabalhava com essa tecnologia acabei conhecendo um pouco sobre alguns mecanismos criptográficos que são altamente utilizados ainda nos dias atuais, e é sobre isso que vou estar escrevendo aqui, mostrando um pouco dos que eu conheço e colocando alguns exemplos.
{: style="text-align: justify;"}

## O que é a criptografia

Criptografia é o nome dado a mecanismos (em sua maioria matemáticos) que são utilizados para se esconder uma informação, ou muitas vezes validá-la. Esses mecanismos surgiram junto com as primeiras civilizações avançadas, como por exemplo a cifra de césar na Roma antiga, e foram sendo aprimorados para os mais diversos fins, principalmente para proteger documentos e informações sigilosas em épocas de guerra, mas acabou sendo na tecnologia que seu uso mais se intensificou e se tornou primordial, visto que com o advento das redes de computadores, incluindo a internet, ficou muito fácil de se obter informações, onde passamos a ter que proteger as informações sigilosas.
{: style="text-align: justify;"}

Uma curiosidade sobre a criptografia é o pensamento errado que muitos tem de que ela permite impossibilitar plenamente o acesso a uma informação, porém essa linha de pensamento está errada pois todo tipo de criptografia pode ser quebrado utilizando a famosa 'força bruta', que consiste em tentar realizar uma quebra testando todos valores possíveis. Nesse cenário que entra o real objetivo da criptografia, que é **tornar o custo de acesso a uma informação muito maior que o valor da própria informação**. Tentar quebrar uma criptografia por força bruta é sim possível, porém o custo que será necessário em termos de recursos (ex: computadores superpotentes) e tempo acaba inviabilizando a quebra. Podemos fazer uma analogia a um grupo criminoso que gasta milhões de reais em equipamentos para roubar um banco que possui em seu cofre somente alguns milhares de reais. É por isso que sempre precisamos recorrer a mecanismos criptográficos mais modernos e seguros, que não viabilizem esse tipo de ataque.
{: style="text-align: justify;"}

Falando sobre criptografia computacional uma dúvida que as vezes aparece está em como mecanismos matemáticos podem atuar sobre tipos de dados que não são número, como textos, imagens, arquivos de áudio e etc, e a resposta para essa pergunta é simples: **tudo é bit e byte!**
{: style="text-align: justify;"}

Seja um texto, um arquivo de música ou até mesmo os números, tudo é representado computacionalmente como bits e bytes, que basicamente são números de uma forma que o computador consegue entender, e é utilizando essa representação que a criptografia computacional funciona, obtendo os bytes das informações e aplicando os mecanismos criptográficos sobre eles. Por isso que nos exemplos que serão mostrados a seguir, toda a informação utilizada a ser encriptada, as chaves, o dado encriptado e decriptado, tudo é representado como byte, e depois pode ser retornado ao original.
{: style="text-align: justify;"}

Inclusive um dos grandes pontos de atenção quando falamos sobre criptografia de textos está no famoso [encoding](https://en.wikipedia.org/wiki/Character_encoding){:target="_blank"}, que é a forma como um texto será representado computacionalmente. Ao encriptar um texto é necessário sempre extrair seus bytes levando em conta o encoding do texto, pois se obtermos os bytes de um texto com o encoding errado, ao encriptá-lo e decriptá-lo o texto pode ser perdido.
{: style="text-align: justify;"}

Tendo um pouco dessas noções sobre criptografia vamos então ver alguns dos mecanismos criptógraficos mais conhecidos com exemplos de utilização na linguagem Java (versão 11).
{: style="text-align: justify;"}

## DES

O [DES - Data Encryption Standard](https://pt.wikipedia.org/wiki/Data_Encryption_Standard){:target="_blank"} é um algoritmo criptográfico simétrico, o que signficia que ele utiliza somente uma chave, no seu caso de 56 bits. Sua encriptação se dá por meio de uma [cifra de Feistel](https://en.wikipedia.org/wiki/Feistel_cipher){:target="_blank"}, usando blocos de 64 bits.
{: style="text-align: justify;"}

Esse algoritmo foi criado pela IBM em meados de 1970 e foi um dos primeiros a ser utilizado em grande escala, computacionalmente falando, sendo muito utilizado por governos. Atualmente ele não é mais recomendado pois é considerado fraco devido a sua chave ser muito pequena, o que já foi comprovado em ataques de força bruta que demoraram menos de um dia para quebrá-lo.
{: style="text-align: justify;"}

![des](/assets/img/posts/criptografia/des.jpeg)

Vamos ver um exemplo de uso do DES em Java.
{: style="text-align: justify;"}

```java
Cipher cipher = Cipher.getInstance("DES");
SecretKey key = new SecretKeySpec("12345678".getBytes(StandardCharsets.US_ASCII), "DES");

String message = "Olá mundo!";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);

cipher.init(Cipher.ENCRYPT_MODE, key);
byte[] encrypted = cipher.doFinal(data);
System.out.println("Mensagem encriptada: " + new String(encrypted, StandardCharsets.UTF_8));

cipher.init(Cipher.DECRYPT_MODE, key);
byte[] decrypted = cipher.doFinal(encrypted);
System.out.println("Mensagem decriptada: " + new String(decrypted, StandardCharsets.UTF_8));
```
    Mensagem: Olá mundo!
    Mensagem encriptada: ??????.????\
    Mensagem decriptada: Olá mundo!


## Triple DES

O [Triple DES](https://pt.wikipedia.org/wiki/3DES){:target="_blank"}, muitas vezes chamado também de 3DES, foi criado em 1995 é uma evolução natural do DES, que também usa blocos de 64 bits porém com chaves muito maiores de 112 ou 168 bits.
{: style="text-align: justify;"}

Esse algoritmo tem seu funcionamento baseado em seu antecessor, porém com um método onde o DES é aplicado três vezes, sendo o dado encriptado com um pedaço inicial da chave, decriptado com um pedaço intermediário da chave e encriptado com o pedaço final da chave, o que gera um dado encriptado muito seguro, e é justamente por conta dessa tripla encriptação que ele se chama Triple DES.
{: style="text-align: justify;"}

![3des](/assets/img/posts/criptografia/3des.jpeg)

Na imagem anterior podemos ver que temos um elemento novo chamado `IV`, que é o vetor de inicialização [(initialization vector)](https://pt.wikipedia.org/wiki/Vetor_de_inicializa%C3%A7%C3%A3o){:target="_blank"}, sendo um valor aleatório que é anexado ao dado original para encriptação, prevenindo que o resultado da encriptação seja sempre o mesmo para dados iguais, dificultando a sua quebra. Esse é um mecanismo que o 3DES suporta para melhorar ainda mais a segurança da encriptação.
{: style="text-align: justify;"}

E assim temos uma exemplo do 3DES em Java, onde podemos reparar que o `IV` é gerado por um processo aleatório.
{: style="text-align: justify;"}

```java
Cipher cipher = Cipher.getInstance("TripleDES/CBC/PKCS5Padding");
SecretKey key = new SecretKeySpec("abcdefghijklmnopqrstuvwx".getBytes(StandardCharsets.US_ASCII), "TripleDES");
IvParameterSpec iv = new IvParameterSpec(new SecureRandom().generateSeed(8));

String message = "Olá mundo!";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8); 

cipher.init(Cipher.ENCRYPT_MODE, key, iv);
byte[] encrypted = cipher.doFinal(data);
System.out.println("Mensagem encriptada: " + new String(encrypted, StandardCharsets.UTF_8));

cipher.init(Cipher.DECRYPT_MODE, key, iv);
byte[] decrypted = cipher.doFinal(encrypted);
System.out.println("Mensagem decriptada: " + new String(decrypted, StandardCharsets.UTF_8));
```

    Mensagem: Olá mundo!
    Mensagem encriptada: ??????=?*eTFW?
    Mensagem decriptada: Olá mundo!

Para garantia de segurança o `IV` deve ser diferente para cada dado encriptado, e se tem como boa prática utilizá-lo como prefixo na mensagem encriptada, porém isso não está contemplado no código para facilitar a leitura.
{: style="text-align: justify;"}

    Dado encriptado: f839hf09js9hf3dfojfsdor3i[\3#
    IV: fj4lc0*^f
    Mensagem encriptada: fj4lc0*^ff839hf09js9hf3dfojfsdor3i[\3#

Um detalhe que deve ser observado no código é o modo como inicializamos o objeto `Cipher`, onde passamos o valor `TripleDES/CBC/PKCS5Padding`, o que significa que ele foi inicializado com o mecanismo 3DES no modo CBC e padding, o que influencia no comportamento da implementação da criptografia. Para verificar os modos disponíveis deve-se conferir a [documentação oficial da linguagem](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/javax/crypto/Cipher.html){:target="_blank"}.
{: style="text-align: justify;"}

No período em que eu trabalhei com smart cards, a criptografia 3DES era bastante utilizada nos processos de autenticação dos cartões.
{: style="text-align: justify;"}

## AES

O [AES - Advanced Encryption Standard](https://pt.wikipedia.org/wiki/Advanced_Encryption_Standard){:target="_blank"} é também um algoritmo simétrico, proposto por Vincent Rijmen e Joan Daemen em 1998 como uma alternativa mais segura ao DES e 3DES. O algoritmo também é muitas vezes conhecido pelo seu nome original Rijndael, que foi utilizado por seus criadores.
{: style="text-align: justify;"}

Esse algoritmo usa blocos de 128 bits e chaves de 128, 192 ou 256 bits em sua encriptação, usando uma [rede de substituição-permutação](https://en.wikipedia.org/wiki/Substitution%E2%80%93permutation_network){:target="_blank"}, o que difere dos seus antecessores.
{: style="text-align: justify;"}

Atualmente esse é um dos algoritmos mais confiáveis e utilizados, sendo inclusive o algoritmo criptográfico adotado pelo governo dos estados unidos.
{: style="text-align: justify;"}

![aes](/assets/img/posts/criptografia/aes.jpeg)

Como podemos ver pela imagem anterior e pelo código a seguir, também é possível utilizar o `IV` com o AES.
{: style="text-align: justify;"}

```java
Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
SecretKey key = new SecretKeySpec("abcdefghijklmnopqrstuvwx".getBytes(StandardCharsets.US_ASCII), "AES");
IvParameterSpec iv = new IvParameterSpec(new SecureRandom().generateSeed(16));

String message = "Olá mundo!";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);

cipher.init(Cipher.ENCRYPT_MODE, key, iv);
byte[] encrypted = cipher.doFinal(data);
System.out.println("Mensagem encriptada: " + new String(encrypted, StandardCharsets.UTF_8));

cipher.init(Cipher.DECRYPT_MODE, key, iv);
byte[] decrypted = cipher.doFinal(encrypted);
System.out.println("Mensagem decriptada: " + new String(decrypted, StandardCharsets.UTF_8));
```

    Mensagem: Olá mundo!
    Mensagem encriptada: ?0??b????S???
    Mensagem decriptada: Olá mundo!


## RSA

O [RSA](https://pt.wikipedia.org/wiki/RSA_(sistema_criptogr%C3%A1fico)){:target="_blank"} é um algoritmo criptográfico bem diferente dos apresentados até o momento, pois ele é do tipo assimétrico que utiliza duas chaves para encriptação, uma pública e outra privada, sendo esse modelo conhecido também por algoritmo de chave pública. Ele foi proposto por Ron **R**ivest, Adi **S**hamir e Leonard **A**dleman em 1977, e são as iniciais de seus nomes que dão nome ao algoritmo.
{: style="text-align: justify;"}

Esse algoritmo usa duas chaves para a encriptação e transmissão de dados onde o dado encriptado por uma chave só pode ser decriptado pela outra, e vice versa. O tamanho mínimo de chaves usadas no RSA é de 512 bits, porém atualmente o mínimo recomendável é 1024 bits, sendo mais seguras as de 2048 ou 4096 bits.
{: style="text-align: justify;"}

Seu uso é extremamente importante nos dias atuais pois consegue garantir uma comunicação segura entre dois pontos sem a necessidade de que ambas as partes conheçam uma chave em sua totalidade, podendo cada parte ter acesso a somente chave privada ou a pública. Geralmente o dono da chave é quem fica com a chave privada e transmite informações encriptadas com essa chave, e somente quem tem a chave pública é quem irá conseguir visualizar a informação, e caso essas pessoas queiram transmitir uma informação tendo a certeza que somente o destinatário terá acesso basta encriptar com a chave pública do destinatário que fica garantido que somente ele com sua chave privada é quem irá conseguir decriptar a informação.
{: style="text-align: justify;"}

![rsa](/assets/img/posts/criptografia/rsa.jpeg)

A combinação de algoritmos simétricos com algoritmos assimétricos é a base de toda a segurança que existe na internet hoje, incluindo a base do [TLS](https://www.hostinger.com.br/tutoriais/o-que-e-ssl-tls-https){:target="_blank"}, que provê a conexão https.
{: style="text-align: justify;"}

Vamos ao exemplo de código usando RSA.
{: style="text-align: justify;"}

```java
Cipher cipher = Cipher.getInstance("RSA");
KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
keyPairGenerator.initialize(512);
KeyPair keyPair = keyPairGenerator.generateKeyPair();

String message = "Olá mundo!";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);

cipher.init(Cipher.ENCRYPT_MODE, keyPair.getPrivate());
byte[] encrypted = cipher.doFinal(data);
System.out.println("Mensagem encriptada: " + new String(encrypted, StandardCharsets.UTF_8));

cipher.init(Cipher.DECRYPT_MODE, keyPair.getPublic());
byte[] decrypted = cipher.doFinal(encrypted);
System.out.println("Mensagem decriptada: " + new String(decrypted, StandardCharsets.UTF_8));
```

    Mensagem: Olá mundo!
    Mensagem encriptada: L?,?????`??k?e????]?H??^1$??_B$??1z??w?/)R?6P??n;??!
    Mensagem decriptada: Olá mundo!

Uma limitação que existe no RSA está no tamanho de dados que conseguem ser encriptados, que caso sejam grandes são necessárias chaves muito grandes para realizar a criptografia, o que acaba se tornando inviável. Por isso quando é necessário encriptar dados muito grandes é aconselhado o uso de uma criptografia simétrica intermediária para encriptar o dado maior, gerando uma chave aleatória e usando o algoritmo assimétrico para encriptar essa chave aleatória.
{: style="text-align: justify;"}

![rsabigdata](/assets/img/posts/criptografia/rsabigdata.jpeg)

Podemos ver um exemplo disso no código abaixo, onde para encriptar um dado grande foi gerada uma chave 3DES e encriptado o dado com ela, e posteriormente a chave foi encriptada por RSA.
{: style="text-align: justify;"}

```java
String message = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);

Cipher cipher3DES = Cipher.getInstance("TripleDES/ECB/PKCS5Padding");
KeyGenerator keyGenerator  = KeyGenerator.getInstance("TripleDES");
keyGenerator.init(112);
SecretKey key3DES = keyGenerator.generateKey();
byte[] key3DESData = key3DES.getEncoded();

System.out.println("Chave 3DES intermediária: " + new String(key3DESData));

cipher3DES.init(Cipher.ENCRYPT_MODE, key3DES);
byte[] encryptedMessage = cipher3DES.doFinal(data);

System.out.println("Mensagem encriptada com chave intermediária: " + new String(encryptedMessage, StandardCharsets.UTF_8));

Cipher cipherRSA = Cipher.getInstance("RSA");
KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
keyPairGenerator.initialize(512);
KeyPair keyPair = keyPairGenerator.generateKeyPair();

cipherRSA.init(Cipher.ENCRYPT_MODE, keyPair.getPublic());
byte[] encrypted = cipherRSA.doFinal(key3DESData);
System.out.println("Chave 3DES encriptada: " + new String(encrypted));

cipherRSA.init(Cipher.DECRYPT_MODE, keyPair.getPrivate());
byte[] decrypted = cipherRSA.doFinal(encrypted);
System.out.println("Chave 3DES decriptada: " + new String(decrypted));

SecretKey decryptedKey = new SecretKeySpec(decrypted, "TripleDES");

cipher3DES.init(Cipher.DECRYPT_MODE, decryptedKey);
byte[] decryptedMessage = cipher3DES.doFinal(encryptedMessage);
System.out.println("Mensagem decriptada: " + new String(decryptedMessage, StandardCharsets.UTF_8));
```

    Mensagem: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
    Chave 3DES intermediária: aÙ¨kò#sWé=y¤=LsaÙ¨kò#sW
    Mensagem encriptada com chave intermediária: ??A_?<?s4?t`???'?????EW?Xd?_???F?S?)&???rA??M&??@	??H,???(?S???2?uZRW??Z??$]?WC?X?n|?\`????bv??#R??q?S,?????cmK??jn?4??#???@??:?]??T?+?????O???=??i?Z???H??g;????Sv]~X???bRf???l+`??[F!?8I????O?=?l?'??RE?8L?"@GFk5??g}??W8?m????v)??oc???????F????X??o??g?E?7??7??????I????g???i??}$????MV???????QA???\C?????o[?P?z???n{Os9?3?8d.??2Q8Z&\.;?oP??????{"0??E???`"??!?s??FA
    Chave 3DES encriptada: Ø}¹ÚRv¾¥\bk|sœ›ç?3ÏÑÄü}ó1ªíÞú0ÚŽ–Ú"G—èÙÓ7øü3%‰Õ\AÚ›Ý:ª?
    Chave 3DES decriptada: aÙ¨kò#sWé=y¤=LsaÙ¨kò#sW
    Mensagem decriptada: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Hash

O hashing é considerado também um mecanismo criptográfico, porém seu uso não é para esconder uma informação e sim para identificá-la, onde são utilizados mecanismos matemáticos em cima dos dados para gerar essa identificação, sendo geralmente um código.
{: style="text-align: justify;"}

Esse tipo de criptografia é conhecido também como criptografia de caminho único, pois não é possível voltar ao dado original a partir da identificação gerada. Seu maior uso é para verificar integridade e autenticidade de informações, pois a identificação gerada pelo processo (hash) será diferente mesmo com a mínima mudança no dado original.
{: style="text-align: justify;"}

![md5](/assets/img/posts/criptografia/md5.jpeg)

Como exemplo de código aqui vou colocar o mecanismo [MD5](https://pt.wikipedia.org/wiki/MD5){:target="_blank"} de hashing, que trabalha com 128 bits e produz um identificador de 32 caracteres.
{: style="text-align: justify;"}

```java
MessageDigest messageDigest = MessageDigest.getInstance("MD5");
		
String message = "Olá mundo!";

System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);	        

messageDigest.update(data);
byte[] md5 = messageDigest.digest();

System.out.println("MD5 da mensagem: " + Hex.encodeHexString(md5));
```

    Mensagem: Olá mundo!
    MD5 da mensagem: 8d595f21e04dffc4f863bb7d37940b78

Atualmente o MD5 já não é um algoritmo de hashing seguro de ser utilizado, pois ele apresenta uma taxa relevante de colisão, que é quando um identificador gerado é igual para dados completamente diferentes, por isso ele deve ser usado somente para processos mais simples. Uma alternativa mais segura ao MD5 é o [SHA-2](https://en.wikipedia.org/wiki/SHA-2){:target="_blank"}.
{: style="text-align: justify;"}

## Assinatura digital

Um dos grandes usos do mecanismo de hashing é prover a assinatura digital, que é uma forma eletrônica/digital de garantir a autenticidade de uma informação, como um texto, arquivo ou documento.
{: style="text-align: justify;"}

A assinatura digital é feita a partir da combinação de hashing com RSA, onde primeiramente geramos um hash da informação e em seguida encriptamos com a chave pública do destinatário, produzindo a assinatura digital. Assim enviamos a informação e a assinatura, onde o destinatário recebe, decripta a assinatura e com o hash em mãos ele pode gerar novamente o hash da informação e verificar a integridade e autenticidade daquela informação, garantindo que ela não foi alterada no meio do caminho.
{: style="text-align: justify;"}

![digitalsignature](/assets/img/posts/criptografia/digitalsignature.jpeg)

```java
String message = "Olá mundo!";
		
System.out.println("Mensagem: " + message);

byte[] data = message.getBytes(StandardCharsets.UTF_8);

MessageDigest messageDigest = MessageDigest.getInstance("MD5");
messageDigest.update(data);
byte[] md5 = messageDigest.digest();
System.out.println("MD5 da mensagem: " + Hex.encodeHexString(md5));

Cipher cipher = Cipher.getInstance("RSA");
KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
keyPairGenerator.initialize(512);
KeyPair keyPair = keyPairGenerator.generateKeyPair();

cipher.init(Cipher.ENCRYPT_MODE, keyPair.getPublic());
byte[] encrypted = cipher.doFinal(md5);
System.out.println("Assinatura digital da mensagem: " + Hex.encodeHexString(encrypted));

cipher.init(Cipher.DECRYPT_MODE, keyPair.getPrivate());
byte[] decrypted = cipher.doFinal(encrypted);
System.out.println("MD5 da mensagem para verificação: " + Hex.encodeHexString(decrypted));
```

    Mensagem: Olá mundo!
    MD5 da mensagem: 8d595f21e04dffc4f863bb7d37940b78
    Assinatura digital da mensagem: 4f42a5530a8717b7ca23b65c60fe2b94181f0d7a02438b5c84d5898fbc8e762688d0e03306e3d9d84fe5ab482e593eccab1dd46780ae8398b108e53e6b47b576
    MD5 da mensagem para verificação: 8d595f21e04dffc4f863bb7d37940b78


E assim concluímos.
{: style="text-align: justify;"}

Todos os códigos mostrados nesse post podem ser encontrados neste [link](https://github.com/murilo-ramos/murilo-tech/tree/master/javacrypto){:target="_blank"}, incluindo as várias declarações de exceptions que precisam ser tratadas ao se trabalhar com criptografia em Java, que foram omitidas para facilitar o entendimento do código.
{: style="text-align: justify;"}

Outro ponto importante importante que deve ser observado no código é a geração das chaves, que em alguns casos é feita diretamente no código. Para sistema reais deve-se sempre procurar meios [mais seguros](https://en.wikipedia.org/wiki/Key_generation){:target="_blank"} de geração de chaves, que possam gerar chaves altamente aleatórias e seguras, muitas vezes sendo geradas por meio de hardware, o que aumenta muito a segurança.

Até o próximo post.
{: style="text-align: justify;"}