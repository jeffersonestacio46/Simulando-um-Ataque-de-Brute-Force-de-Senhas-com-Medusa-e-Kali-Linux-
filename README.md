**Simulando um ataque de Brute Force de senhas com a Medusa em nosso Kali Linux**

**Ataque simulato**

Nesse projeto iremos usar o Metasploitable2 como a máquina alvo da invasão, e usaremos nosso Kali Linux para realização dos ataques. Vale lembrar que esses ataques devem ser realizados em ambiente controlado, ou com autorização.

Antes de tudo, devemos verificar a comunicação entre ambas as máquinas por IP. Nesse caso, para verificar se está havendo comunicação entre a máquina alvo e a máquina atacante, usaremos o comando abaixo para verificar portas abertas e comunicação:

> `ping 192.168.56.102` ou `nmap -Pn -sV 192.168.56.102` (o nmap é usado para verificar as portas abertas do nosso alvo)

Após a verificação da porta aberta (nesse caso será a porta FTP que iremos utilizar para realização do ataque de brute force), iremos fazer duas wordlists, uma para usuários e outra para as senhas; nessas wordlists haverá vários possíveis usuários e senhas.
Você pode usar o comando abaixo para criar as suas wordlists:

> `echo -e "root\admin\ftp\user\teste" > Users.txt`
> `echo -e "123456\password\admin\1234\root\qwert" > Passwords.txt`

Segue uma breve explicação sobre o comando acima:

`echo` escreve texto na saída padrão.
A opção `-e` faz o `echo` interpretar escapes de barra invertida (como `\n`, `\t`, `\r`, `\a`, `\123` — octal — etc.). Ou seja: certas sequências `\X` são transformadas em caracteres especiais.
A string entre aspas:

> `"123456\password\admin\1234\root\qwert"`

Quando `echo -e` interpreta essa string, nem todos os `\` são tratados da mesma forma:

> `\p` e `\q` não são escapes reconhecidos — nestes casos o `echo` normalmente mantém a barra invertida seguida da letra (ou seja, `\p` permanece `\p`).
> `\a` é reconhecido: representa o caractere BEL (campainha) — ASCII 7. Isso pode causar um bip (ou apenas um caractere invisível) no arquivo.
> `\r` é reconhecido: representa carriage return (CR, ASCII 13). Isso é um caractere de controle que pode retornar o cursor ao início da linha; em um arquivo ficará como um byte 0x0D.
> `\123` é interpretado como escape octal (até 3 dígitos). `\123` (octal) = decimal 83 = caractere ASCII `'S'`. Note que, se aparece `\1234`, o `echo` toma `\123` (os três dígitos) e o `4` sobra como caractere literal.
> `Passwords.txt`

`>` redireciona a saída do `echo` para o ficheiro `Passwords.txt`, sobrescrevendo o arquivo (apaga o que havia antes). Se quiser acrescentar em vez de sobrescrever, use `>>`.

Depois da criação das nossas wordlists, iremos executar o seguinte código abaixo:
Observação: verifique o diretório onde foram salvas as wordlists

> `medusa -h 192.168.56.102 -U /path/to/wordlists/Users.txt -P /path/to/wordlists/Passwords.txt -M ftp`

Segue o exemplo para que possamos realizar a validação e checar se realmente iremos obter acesso à máquina alvo, com o usuário e senha encontrados como válidos para acesso:

> `smbclient -L //192.168.56.102 -U username`
> `smbclient //192.168.56.102/share -U username`

Agora vamos para um ataque de Brute Force web, em nosso DVWA, para ter acesso ao usuário e à senha com as nossas wordlists criadas (você pode criar outras wordlists, com uma gama maior de nomes de usuários e senhas, para um ataque com maiores possibilidades de sucesso).

Segue o exemplo de um ataque utilizando a Medusa em nosso Kali Linux:
Como informado anteriormente, sempre verifique o diretório das nossas wordlists para que possamos buscá-las na utilização do Medusa.

> `medusa -h 192.168.56.102 -U /path/to/wordlists/small-Users.txt -P /path/to/wordlists/Pass.txt -M smbnt`

Segue o exemplo para que possamos realizar a validação e checar se realmente iremos obter acesso ao site alvo, com o usuário e senha encontrados como válidos para acesso:

> `smbclient -L //192.168.56.102 -U username`
> `smbclient //192.168.56.102/share -U username`

Nesse repositório possui uma pasta com senhas e logins para exemplos.
