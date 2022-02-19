# <small>Erro no Android:</small> <code><samp lang="en">read-only file system</samp></code> | <code><samp lang="pt">Sistema de arquivos somente leitura</samp></code>

## :fast_forward: TL;DR

Remontar, com permissão de escrita, o diretório ou a imagem de mídia com o comando mount

---

> Erro <code><samp>Sistema de arquivos somente leitura</samp></code>: em sistema Android com tradução em português nos componentes de desenvolvimento  
> Erro <code><samp>read-only file system</samp></code>: em sistema Android não traduzidos

Assim como no GNU\Linux o problema está relacionado ao modo de montagem do sistema de arquivos.
Se o erro ocorrer ao criar ou remover um arquivo em `/system`, por exemplo, então talvez seja necessário **remontar o diretório com permissão de escrita**:
```sh
adb shell
su
mount -o remount,rw /system
```
