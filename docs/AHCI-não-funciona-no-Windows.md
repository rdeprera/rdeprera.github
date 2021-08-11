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

> Para **habilitar AHCI no Windows** 10 e 11 **não basta editar a BIOS** (alterando a interface - de IDE/RAID/SATA - para AHCI). É necessário preparar o sistema para que ele possa reinicializar com interface AHCI sem dar erros

Compreendido isso, existem formas diferentes para atender a essas 5 etapas: **editando o <abbr title="Registry Editor" lang="en">REGEDIT</abbr>** ou usando o <strong lang="en">Safe Mode</strong> (modo seguro de boot).
  
### Habilitando AHCI com Boot em <em lang="en">Safe Mode</em> - Modo Seguro de Inicialização

  O modo seguro (safe mode) que usaremos é aquele com menor quantidades de recursos possível para garantir que não haverá incompatibilidade do SO com a inferface AHCI:
  
1. Entrar no MSConfig: <kbd><kbd>cmd</kbd> + <kbd>r</kbd></kbd> ⇒ <code>msconfig</code>
    - Abrir aba <kbd><samp>inicialização do Sistema</samp></kbd>
    - Marcar o item <kbd><samp>Inicialização segura</samp></kbd>
    - Marcar o subitem <kbd><samp>Mínima</samp></kbd>
2. Reiniciar o Windows
3. Entrar na BIOS: normalmente pressionando a tecla <kbd>del</kbd> antes da inicialização do sistema operacional
    - Ativar o AHCI nas configurações de interface de controlador de disco rígido. O local dessa configuração e como ela está escrita, depende de modelo para modelo de firmware (BIOS/<abbr title="Unified Extensible Firmware Interface" lang="en">UEFI</abbr>)
    - Salvar as alterações e sair
4. Reiniciar o Windows
5. Entrar novamente no MSConfig e refazer as configurações de boot:
    - Desmarcar a opção <kbd><samp>Inicialização segura</samp></kbd>
    - Marcar a opção <kbd><samp>Tornar permanente todas as configurações de inicialização</samp></kbd>
    - Ao salvar as configurações pressionando <kbd><samp>OK</samp></kbd>, novamente o MSConfig mostrará um alerta para reiniciar o sistema. Devemos reiniciar para efetivar as alterações

Pronto! Está feito a alteração. Como pedimos para o Windows reiniciar com configuração mínima, ele reiniciou sem nenhum problema e instalou os drivers para a interface do controlador AHCI automaticamente.

### Habilitando **AHCI usando o REGEDIT**

1. Abrir o editor de registro do Windows (REGEDIT): <kbd><kbd>cmd</kbd> + <kbd>r</kbd></kbd> ⇒ <code>regedit</code>
    - Se o Controle de Conta de Usuário do Windows (<abbr title="User Account Control" lang="en">UAC</abbr>) solicitar permissão administrativa, informe <kbd><samp>Sim</samp></kbd> para abrir o REGEDIT
    - Editar o valor <kbd><samp>Start</samp></kbd> na chave <code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iaStorV</code>

