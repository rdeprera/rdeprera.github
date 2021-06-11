# Como Extrair um SNAP (Pacote de Software)

Primeiro é necessário baixar o arquivo  com extensão .snap, ou seja, o pacote snap:
~~~shell
sudo snap download <nomedopacote>
~~~

No caso de receber um erro como este:
> error: cannot download snap \<nomedopacote\>: no snap revision available as specified

É porque provavelmente é a primeira versão do software ou o pacote está mal formatado. Não sei ainda ao certo. Mas neste caso basta adicionar o parâmetro --release=<versão> para forçar o download e suprimir o erro. Na <versão> podemos usar *latest*, *master*, *stable* ou o número da versão.

O comando irá baixar dois arquivos. Um de extensão .snap e outro de extensão .assert. Basta então montar o snap, que é uma como se fosse uma imagem de um outro sistema de arquivos, o squashfs. Então, para montá-lo:
~~~shell
mkdir ./<nomedopacote>/
mount -t squashfs -o ro <nomedopacote>.snap ./<nomedopacote>/
cd ./<nomedopacote>/ && ls -lna
~~~

Pronto!
