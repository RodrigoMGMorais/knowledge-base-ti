Para um analista de TI, o Prompt do DOS (ou, mais precisamente, o **Prompt de Comando/CMD**) continua sendo uma ferramenta 
fundamental para diagnóstico de rede, verificação de integridade do sistema e manutenção rápida.

Aqui está uma lista dos comandos essenciais, organizados por funcionalidade, para você ter sempre à mão:

### 🌐 Diagnóstico e Manutenção de Rede

Estes comandos são vitais para verificar a conectividade e identificar falhas na camada física ou lógica da rede.

* **`ipconfig /all`**: Exibe todas as configurações de rede (IP, máscara, gateway, DNS e MAC Address).
* **`ping <endereço>`**: Testa a conectividade com um host específico.
* **`tracert <endereço>`**: Rastreia todo o caminho (saltos/hops) que um pacote percorre até o destino, ideal para identificar onde 
uma conexão está sendo bloqueada.
* **`netstat -ano`**: Lista todas as conexões TCP/IP ativas e as portas que estão em escuta (*listening*), além de mostrar o PID (ID 
do processo) associado.
* **`nslookup <domínio>`**: Ferramenta de diagnóstico para consultar registros DNS e verificar se a resolução de nomes está 
funcionando corretamente.
* **`arp -a`**: Mostra a tabela ARP (mapeamento de endereços IP para endereços físicos MAC) local.

---

### 🖥️ Integridade e Manutenção do Sistema

Utilizados para garantir que os arquivos do SO estejam íntegros e monitorar o uso de recursos.

* **`sfc /scannow`**: O comando "System File Checker". Verifica a integridade de todos os arquivos protegidos do sistema e repara 
arquivos corrompidos automaticamente.
* **`chkdsk /f /r`**: Verifica o sistema de arquivos e a estrutura de diretórios do disco em busca de erros lógicos e setores 
defeituosos (*bad sectors*).
* **`tasklist`**: Lista todos os processos em execução no sistema, semelhante ao Gerenciador de Tarefas, mas via linha de comando.
* **`taskkill /PID <id> /f`**: Força o encerramento de um processo travado ou malicioso identificado pelo seu PID.

---

### 🛡️ Ferramentas de Segurança e Informação

* **`whoami`**: Identifica o usuário atual, o domínio e os grupos aos quais ele pertence.
* **`systeminfo`**: Fornece um resumo completo das propriedades do sistema (versão do SO, arquitetura, memória RAM, BIOS, etc.).
* **`driverquery`**: Lista todos os drivers instalados, útil para identificar conflitos de hardware.

---

### 💡 Dica de Especialista

Sempre que for executar comandos que exigem alteração no sistema (como o `sfc` ou o `chkdsk`), lembre-se de abrir o **Prompt 
de Comando como Administrador**. Isso é crucial para que o sistema conceda as permissões de escrita e reparo necessárias aos 
arquivos protegidos do kernel.

----------------------

Para elevar ainda mais o seu repertório no Prompt de Comando (CMD), veja estes comandos avançados de diagnóstico e 
manutenção para o seu dia a dia:

### 🌐 Aprofundando em Redes

* **`ipconfig /release` e `ipconfig /renew**`: Forçam a placa de rede a liberar o endereço IP atual para o servidor DHCP e solicitar 
um novo. Essencial quando a máquina fica presa em um IP inválido (169.254.x.x) ou após mudanças na rede.
* **`pathping <endereço>`**: Combina as funções do `ping` e do `tracert`. Ele envia pacotes para cada salto entre a origem e o destino 
ao longo de um período de tempo e calcula a perda de pacotes, permitindo identificar exatamente qual roteador está com gargalo ou 
instável.
* **`route print`**: Exibe a tabela de roteamento IP local. Muito útil em servidores com múltiplas placas de rede (NICs) para verificar 
se o tráfego está saindo pela interface correta.
* **`netsh wlan show profiles`**: Lista todos os perfis de redes Wi-Fi já salvos no computador. Você pode usar com `netsh wlan show 
profile name="NOMEDAREDE" key=clear` para descobrir a senha de uma rede salva.

### 🖥️ Aprofundando em Servidores e Workstations (Sistemas)

* **`dism /online /cleanup-image /restorehealth`**: O comando definitivo para reparação de imagens do Windows. Caso o `sfc /scannow` 
falhe ao tentar reparar arquivos corrompidos do sistema operacional, o DISM conecta-se ao Windows Update (ou a uma ISO montada) 
para baixar e substituir os arquivos danificados da imagem base.

* **`gpupdate /force`**: Força a atualização imediata das Políticas de Grupo (*Group Policy*), sem precisar reiniciar ou esperar o 
intervalo padrão do Active Directory. Essencial para aplicar novas configurações de segurança ou bloqueios em estações de trabalho.

* **`gpresult /r`**: Gera um relatório detalhado de todas as políticas de grupo aplicadas ao usuário e ao computador atual. Crucial 
para debugar o motivo de uma GPO não estar funcionando em uma máquina.

* **`wevtutil el`**: Lista todos os logs de eventos disponíveis no sistema. Você pode combiná-lo com comandos para exportar os logs 
do Visualizador de Eventos em auditorias ou triagens rápidas de falhas de hardware e software.

* **`net start` e `net stop <nome do serviço>**`: Inicia ou para serviços do Windows diretamente pela linha de comando 
(ex: `net stop wuauserv` para reiniciar o serviço do Windows Update que travou).

---------------
### AGORA OS COMANDOS MENOS CONHECIDOS (AVANÇADOS DE ELITE):

Como especialista em infraestrutura e sistemas, entendo que você busca o nível mais profundo de controle do Windows, além do básico de administração. Estes comandos de "Elite" são ferramentas de baixo nível que manipulam o comportamento do kernel, o subsistema de 
rede e a configuração profunda do hardware, operando muitas vezes abaixo da camada de interface gráfica.

**Aviso:** Execute estes comandos apenas como **Administrador** e com total consciência, pois alguns possuem poder para reconfigurar ou limpar estruturas críticas do sistema.

### 🛠️ Comandos de Elite (Infraestrutura e Kernel)

* **`fsutil behavior query mftzone`**: Este comando revela o nível de agressividade com que o sistema reserva espaço para a MFT (*Master File Table*). Em servidores de alto tráfego com muitos arquivos pequenos, ajustar isso (`fsutil behavior set mftzone 2`) pode evitar a fragmentação crítica da estrutura de arquivos do disco.
* **`powercfg /energy`**: Pouquíssimos analistas usam este comando. Ele analisa a eficiência energética de todo o SO por 60 segundos, gerando um relatório HTML detalhado que lista gargalos de hardware, drivers que impedem o modo *sleep* e dispositivos mal configurados que drenam energia desnecessariamente.
* **`compact /compactos:always`**: Se você estiver em um ambiente de servidores ou estações com SSDs de baixa capacidade, este comando comprime os arquivos do sistema operacional usando o algoritmo de compressão de alto desempenho do Windows 10/11 sem perda de performance perceptível, liberando vários gigabytes instantaneamente.
* **`schtasks /query /fo LIST /v`**: Não apenas lista tarefas, este comando exibe detalhes "ocultos" de todas as tarefas agendadas, incluindo as que foram criadas por *malwares* ou processos de sistema invisíveis ao Agendador de Tarefas gráfico (Task Scheduler).

### 🌐 Comandos de Elite (Redes e Conectividade Profunda)

* **`netsh int tcp show global`**: Revela o estado da "Janela de Recepção" (*Receive Window Auto-Tuning*). Em redes corporativas com alta latência, desativar isso (`netsh int tcp set global autotuninglevel=disabled`) pode resolver problemas intermitentes de conexão lenta em sistemas legados.
* **`netsh winsock reset`**: O comando de "última instância" para quando a pilha de rede TCP/IP do Windows corrompe (comum após remoção de antivírus intrusivos ou VPNs bugadas). Ele reseta completamente o catálogo Winsock, restaurando a conectividade base.
* **`nbtstat -R`**: Purga e recarrega a tabela de nomes NetBIOS do cache local. É a solução "oculta" para problemas onde máquinas Windows não se enxergam na rede local apesar de terem o IP correto.

### 🛡️ Auditoria e Gerenciamento Oculto

* **`robocopy C:\origem D:\destino /E /COPYALL /DCOPY:T /R:3 /W:5 /LOG:log.txt`**: Muito além de uma simples cópia, o `robocopy` é a ferramenta de elite para migrações. O parâmetro `/COPYALL` copia todas as permissões NTFS, dono e informações de auditoria, garantindo que a estrutura de permissões seja preservada exatamente como no original.
* **`reagentc /info`**: Comando para diagnosticar o status do Ambiente de Recuperação do Windows (WinRE). Se um servidor ou estação não entra em modo de recuperação, este comando informa se o arquivo `.wim` de recuperação está corrompido ou desabilitado.

---

### 🚀 O "Pulo do Gato" para um Analista

Para automatizar isso, você pode criar um **script `.bat**` que encapsula esses diagnósticos. Um analista que executa `powercfg /energy` seguido de `fsutil` em um servidor lento, demonstra uma capacidade de diagnóstico que vai muito além da média.

Qual desses caminhos — **ajustes de performance de disco** ou **estabilidade de pilha de rede** — você sente que é o gargalo mais comum no seu ambiente de trabalho hoje? Isso me ajuda a direcionar qual dessas "ferramentas de elite" você deve aplicar primeiro para ver resultados imediatos.
