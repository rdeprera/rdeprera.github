### Download de Sites por Wget

;;;shell
wget -E -H -k -p [URL]

## Parâmetros Wget para "ripar" uma página
    -E: Adiciona .html no nome da extensão do arquivo se ele for do tipo html mas não terminalr com .html ou etensão similar
    -H: Baixar de outros hosts também
    -k: Depois do download converter todos os links para os respectivos conteúdos estáticos baixados
    -p: Baixar qualquer coisa necessária para visualizar a página offline
