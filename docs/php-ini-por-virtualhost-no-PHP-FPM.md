
Neste exemplo estou usando o [PHP-FPM](https://www.php.net/manual/pt_BR/install.fpm.php) <var>8.1</var> e o servidor web [Apache](https://httpd.apache.org/) <var>2.4</var>. Para as demais versẽs do PHP-FPM e Apache, ou outros servidores Web, é necessário apenas fazer ajustes nos diretórios e no nome dos serviços `apache` e `php-fpm`.

## GNU/Linux

### Novo `php.ini` para um Novo VirtualHost
Fazer uma cópia do `php.ini` e fazer todos os ajustes no novo, como por exemplo, ativar extensões.
```sh
sudo cp /etc/php/8.1/fpm/php.ini /etc/php/8.1/fpm/php.novo-virtualhost.ini
```

### Novo Pool <abbr title="FastCGI process manager" lang="en">FPM</abbr>
1. Copiar a configuração padrão do PHP-FPM para um **novo arquivo de configuração PHP-FPM**
  ```sh
  sudo cp /etc/php/8.1/fpm/php-fpm.conf /etc/php/8.1/fpm/php-fpm.novo-virtualhost.conf
  ```
2. E configurar, nesse novo arquivo `php-fpm.novo-virtualhost.conf`, um novo diretório de pool (+-) na linha 145
  ```INI
  ; Relative path can also be used. They will be prefixed by:
  ;  - the global prefix if it's been set (-p argument)
  ;  - /usr otherwise
  ; 
  ; Criando um diretório personalizado para virtuahosts que usarão o novo php.ini (php.novo-virtualhost.ini)
  ; include=/etc/php/8.1/fpm/pool.d/*.conf
  include=/etc/php/8.1/fpm/pool.novo-virtualhost.d/*.conf
  ```
3. Especificar um **caminho alternativo para o arquivo de configuração do FPM** (Gerenciador de Processos FastCGI FastCGI)
  ```sh
  php-fpm -c /etc/php/8.1/fpm/php.novo-virtualhost.ini --fpm-config /etc/php/8.1/fpm/php-fpm.novo-virtualhost.conf
  ```
### Executando o Novo VirtualHost com um Novo `php.ini`

Reiniciar os serviços:
```sh
sudo systemctl start apache2 php8.1-fpm
```
Para testar se o virtuahost está usando o novo `php.novo-virtualhost.ini`, basta, por exemplo, usar a função `phpinfo()` **dentro do novo virtualhost**.
Lembrando que para os demais virtualhosts, assim como para os comandos em php executados em linha de comando (php-cli), a versão do `php.ini` continuará inalterada.

## Windows

Com exeção dos comandos relativos ao Linux (sudo, cp) não muda muita nada no Windows. Bastando apenas abrir um terminal como administrador, ignorar o comando `sudo` e trocar o comando `cp` por `copy`.

Em substituição ao **comando `systemctl` para reiniciar os serviços**, no Windows existe por padrão a ferramenta gráfica *services*.  
Para reiniciar na linha de comando, é necessário estar no diretório de binários do Apache e do PHP, ou, configurar esses diretórios como parte da variável de ambiente PATH. Então, poderá usar o comando `httpd -k restart` para **reiniciar o Apache** e o comando <code>php<var><versão do php></var>-fpm restart</code> para **reiniciar o PHP-FPM**.

## macOS

Usar o `apachectl` e `launchctl` em substituição ao `systemctl`, o resto, provavelmente é igual.
