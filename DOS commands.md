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

