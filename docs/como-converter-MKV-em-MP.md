
# Convertendo <samp>.mkv</samp> para <samp>.mp4</samp>
Como **converter formato de arquivo** *Matroska* em *MPEG-4* **sem perder qualidade**?  
<mark>Útil para, entre outras coisas, **transmitir vídeo para uma Android TV** por streaming</mark>.

<details>
  <summary>TL;DR</summary>
  Usando o GPAC e o MKVToolNix podemos fazer conversões, extrações, compactações e muito mais.<br />
  Aqui, mostro como fazer a conversão de <samp><abbr title="Matroska">MKV</abbr></samp> para <samp><abbr title="MPEG-4" lang="en">MP4</abbr></samp>.
</details>

## Softwares necessários
- [x] [GPAC](http://gpac.io) é um framework para processar, inspecionar, empacotar, transmitir, reproduzir e interagir com o conteúdo de mídia. Qualquer combinação de áudio, vídeo, legendas, metadados, gráficos escalonáveis, mídia criptografada, gráficos 2D/3D e ECMAScript. O GPAC é mais conhecido por seus amplos recursos de MP4
- [x] [MKVToolNix](https://mkvtoolnix.download/) é um conjunto de ferramentas para criar, alterar e inspecionar arquivos Matroska
- [ ] \(opcional) MKVToolNix GUI É a interface gráfica escrita com GTK, que fornece uma visão de metadados ampla e rápida, além de recursos de edição. Como remover legendas e áudios que não serão utilizados, deixando o arquivo menor <mark>e mais fácil para converter em outros formátos</mark>

Ambos estão disponíveis para Linux, Windows, macOS - e acho que outros sistemas também. 

### Instalação do GPAC e MKVToolNix
Ambos podem ser instalados em uma das seguintes formas:
- [Através de gerenciadores de pacotes](#gerenciadores-de-pacotes)
  - Pré-compilados em repositórios oficiais de distribuições Linux
  - Binários em repositório do MKVToolNix para algumas distruibuições Linux
  - Binários em repositórios de gerenciadores de pacotes para Windows como o Chocolatey ou o HomeBrew para macOS
- Utilizando-se de binários (executáveis) disponível no site de cada software ou no repositório de cada um
- Compilando o código fonte


#### Linux

#### Gerenciadores de Pacotes

Base Debian:
``` shell
; aptitude|apt-get|apt
sudo apt install gpac mkvtoolnix
# GUI MKVToolNix (opcional)
; sudo apt install mkvtoolnix-gui
```
Base Arch:
``` shell
sudo pacman -S gpac mkvtoolnix-cli
# GUI MKVToolNix (opcional)
; sudo pacman -S mkvtoolnix-gtk
```
#### Windows e macOS
Consulte a documentação nos respectivos sites:

GPAC
: [GitHub](https://github.com/gpac/gpac) <kbd>https://github.com/gpac/gpac</kbd>
: [Site](http://gpac.io) <kbd>http://gpac.io/</kbd> ou <kbd>https://gpac.wp.imt.fr</kbd>
MKVToolNix
: [GitHub](https://github.com/nmaier/mkvtoolnix) <kbd>https://github.com/nmaier/mkvtoolnix</kbd>
: [Site](https://mkvtoolnix.download) <kbd>https://mkvtoolnix.download</kbd>


## Convertendo Nosso MKV em MP4

### Extrair o Áudio e o Vídeo do Arquivo MKV

Primeiro precisamos checar as trilhas existentes no <samp>.mkv</samp> (mastroska).
``` shell {#exemplo1}
mkvmerge --identify video.mkv
```
<pre>
<samp>
Arquivo 'video.mkv': contêiner: Matroska  
ID da faixa 0: video (AVC/H.264/MPEG-4p10)  
ID da faixa 1: audio (AAC)  
ID da faixa 2: audio (AAC)  
ID da faixa 3: audio (AAC)  
ID da faixa 4: audio (AAC)  
ID da faixa 5: audio (AAC)
</samp>
</pre>

No [exemplo acima](#exemplo1), a saída do comando <code>mkvmerge --identify video.mkv</code> resultou em 6 trilhas:  
1. ID 0: vídeo usando os codecs AVC, H.264 e MPEG-4p10
2. ID 1: áudio usando o codec AAC
3. ID 2: áudio usando o codec AAC
4. ID 3: áudio usando o codec AAC
5. ID 4: áudio usando o codec AAC
6. ID 5: áudio usando o codec AAC

> As trilhas de áudio e vídeo do [exemplo](#exemplo1) usam os codecs AAC, AVC, H.264 e MPEG-4p10. Mas poderiam aparecer outros codecs, como: A_AC3, V_MPEG4, ISO <abbr lang="latin" title="et cetera">etc...</abbr>

<details>
  <summary>
Agora que temos as ID de cada trilha já podemos fazer a extração do áudio e do vídeo que compõem o arquivo mkv  
  </summary>
  
  <code>mkvextract tracks <var>\<video.mkv\></var> <var>\<ID da trilha de vídeo\></var>:<var>\<nome-do-arquivo-de-trilha-de-video\></var>.<var>\<codec\></var> <var>\<ID da trilha de áudio\></var>:<var>\<nome-do-arquivo-de-trilha-de-audio\></var>.<var>\<codec\></var></code>
</details>

No nosso caso:
```shell
mkvextract tracks video.mkv 0:video.h264 3:audio.ac3
```
<pre>
<samp>Extraindo faixa <var>0</var> com o ID de codec '<var>V_MPEG4/ISO/AVC</var>' para o arquivo '<kbd>video.h264</kbd>'. Formato do contêiner: <var>AVC/H.264</var> elementary stream  
Extraindo faixa <var>3</var> com o ID de codec '<var>A_AAC</var>' para o arquivo '<kbd>audio.ac3</kbd>'. Formato do contêiner: raw <var>AAC</var> file with ADTS headers  
Progresso: 100%
</samp>
</pre>

### Compilar o Áudio e o Vídeo em um **Arquivo MP4**
Usaremos o programa MP4Box, que vem junto com o GPAC que foi previamente instalado, para unir nosso arquivo de áudio (audio.ac3) e o de vídeo (video.h264), em um arquivo único no formato MP4:
```shell
MP4Box -add video.h264 -add audio.ac3 video.mp4
```
> Em alguns casos o MP4Box pode não detectar o <abbr title="Frames per Seconds" lang="en">fps</abbr> do vídeo. Nesse caso temos que informar o fps manualmente usando o parâmetro <kbd>-fps</kbd>. Por exemplo (com 24fps, que é bem comum):
```shell
MP4Box -fps 24 -add video.h264 -add audio.ac3 video.mp4
```
<pre>
<samp>
Track Importing MPEG-4 AVC - Width 1920 Height 1040 FPS 24/1 SAR 1/1
AVC|H264 Import results: 137424 samples (138667 NALUs) - Slices: 1414 I 70966 P 65045 B - 1 SEI - 1240 IDR
AVC|H264 Stream uses forward prediction - stream CTS offset: 4 frames
Track Importing AAC  - SampleRate 44100 Num Channels 2
Saving video.mp4: 0.500 secs Interleaving
</samp>
</pre>

> Note que as saídas das instruções do MP4Box estão em inglês, enquanto que as instruções dos programas do MKVToolNix estão em português. 
Isso pode variar de acordo com a disponibilidade de tradução para a versão do sofware e de acordo com a configuração de idioma do sistema operacional.

Pronto! Está concluída a conversão e temos um lindo arquivo com extensão .mp4 que pode ser usada para fazer o **<i lang="en">casting</i> em uma Android TV** ou qualquer outra coisa que não é possível usando o formato Matroska 🔚
