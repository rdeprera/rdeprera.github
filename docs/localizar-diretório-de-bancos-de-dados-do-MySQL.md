# Como **Localizar** o **Diretório de Bancos de Dados do MySQL** :question:

A diretiva `datadir` controla onde o <abbr title="Sistema Gerenciador de Banco de Dados">SGDB</abbr> MySQL irá armazenar os binários dos bancos de dados. Essa diretiva está no arquivo de configuração do MySQL e fica armazenada na memória como constante, disponível no daemon do serviço MySQL.

## :fast_forward: <abbr title="Too Long; Didn’t Read" lang="en">TL;DR</abbr>

Portanto, podemos descobrir seu valor:
- No arquivo de configuração do MySQL (normalmente `mysql.conf`)
- Consulta <abbr title="Structured Query Language" lang="en">SQL</abbr> `select @@datadir;`
- Usando o <abbr title="Command Line Interface" lang="en">CLI</abbr> `mysqld --help --verbose`

## por Consulta SQL :abacus:

É o **método mais confiável**, visto que existe a possibilidade de haver **vários serviços MySQL em execução**, ou, **arquivos de configurações inválidos** - que normalmente são sujeiras procedentes de outras instalações ou algum acidente de cópia de arquivo. Para quem não tem muita experiência em depurar processos de serviços e daemons através da linha de comando, descobrir consultando diretamente no serviço em execução é a maneira mais segura.

1. Acessar a shell do [<abbr title="My Structured Query Language" lang="en">MySQL</abbr>](https://www.oracle.com/br/mysql/)  
  1.1 Solicitar a autenticação no serviço MySQL com conta <dfn>sudoer<dfn> (que possa acessar comandos/serviços do root), neste exemplo, a conta <kbd><var>usuario</var></kbd> do Linux  
  1.2 Usar uma conta com autorização para visualizar a diretiva `datadir`, neste caso a conta <kbd><var>root</var></kbd> do MySQL  
  1.3 Informar a senha de ambas as contas, respectivamente  
  ```console
  usuario@linux:~$ sudo mysql -u root -p
  [sudo] senha para <usuário>:
  Enter password:
  ```
  Quando logamos através de um terminal, o SGDB MySQL nos coloca dentro de uma shell <samp>mysql></samp> com ferramentas que fazem interface com quase todos os recursos desse **Banco de Dados**.
  
2. Consultar o local que o <abbr title="Sistema Gerenciador de Banco de Dados">SGDB</abbr> armazena os arquivos binários dos bancos de dados com o comando:
  ```console
  mysql> select @@datadir;  
  # o resultado será similar a:
  +-----------------+
  | @@datadir       |
  +-----------------+
  | /var/lib/mysql/ |
  +-----------------+
  1 row in set (0.00 sec)
  ```

## por Consulta no **Daemon MySQL** `mysqld` :brain:

O daemon do MySQL tem uma interface CLI chamada `mysqld` que mostra o valor da diretiva `datadir` usando os parametros `--verbose --help`:  

```console
mysqld --help --verbose
... 
...
...
datadir                                                      /var/lib/mysql/
...
...
...
# os ... são para resumir a saída do comando, que é bem extensa
```

###  No Linux

```console
# normalmente o mysqld fica instalado em /usr/bin/mysqld
usuario@linux:~$ which mysqld
/usr/sbin/mysqld
# se o caminho /usr/sbin estiver na variável de ambiente PATH, então não será necessário usar o caminho absoluto
```
```console
# exibindo apenas o valor da diretiva datadir
usuario@linux:~$ /usr/sbin/mysqld --help --verbose | grep "datadir " |  awk '{ print $2 }'
/var/lib/mysql/
```
