# <abbr title="Advanced Host Controller Interface" lang="en">AHCI</abbr> não funciona no **Windows** 10 ou 11

A **interface <abbr title="Advanced Host Controller Interface" lang="en">AHCI</abbr> é suportada pelo <em lang="en">Windows</em>** através de <span lang="en">drivers</span> nativos desde a versão Windows 95, pelo menos.  
O problema acontece quando instalamos o <abbr title="Sistema Operacional">SO</abbr> em uma máquina que está configurada (na <abbr title="Basic Input/Output System" lang="en">BIOS</abbr>) com uma interface diferente (<abbr title="Integrated Drive Electronics" lang="en">IDE</acronym>, por exemplo), e depois mudamos a interface para AHCI na BIOS. O <em>Windows 10</em> e o <em>Windows 11</em> não suportam essa mudança sem que antes sejam feitas algumas alterações.

## Como Habilitar AHCI no Windows 10/11

Portanto, para habilitar AHCI nesses sistemas da Microsoft é necessário:

1. Remover algumas configurações de carregamento (boot)
2. Reiniciar o SO
3. Fazer a alteração de interface para AHCI na BIOS
4. Reiniciar o SO
5. Refazer as configurações de carregamento (boot)

> Para **habilitar AHCI no Windows** 10 e 11 **não basta editar a BIOS** (alterando a interface - de IDE/RAID/SATA - para AHCI)

Compreendido isso, existem formas diferentes para atender a essas 5 etapas: **editando o REGEDIT** ou usando o <strong lang="en">Safe Mode</strong> (modo seguro de boot).
  
### Habilitando AHCI com Boot em Safe Mode - Modo Seguro de Inicialização

  O modo seguro (safe mode) que usaremos é aquele com menor quantidades de recursos possível, para garantir que não haverá incompatibilidade do SO com a inferface AHCI:
  
1. Abrir o MSConfig: <kbd><kbd>cmd</kbd> + <kbd>r</kbd></kbd> ⇒ <code>msconfig</code>
    - Abrir aba <kbd><samp>inicialização do Sistema</samp></kbd>
    - Marcar a opção <kbd><samp>Inicialização segura</samp></kbd>
2. Continua....
