# 🐧 Guia Avançado de Diagnóstico Linux, Logs e Verificação de Erros

Este guia centraliza os comandos essenciais e os "pulos do gato" utilizados em ambientes de sustentação de alta criticidade para 
auditoria de comandos, análise de logs em tempo real e verificação de saúde do sistema.

---------------------------------------------
### Consumo de Recursos (CPU, Memória, I/O):
- Estes comandos revelam gargalos imediatos.

💻 top / htop 
- A visão em tempo real. O htop é preferido por ser mais intuitivo (permite filtrar e ordenar por memória ou CPU facilmente).

💻 vmstat 1
- Excelente para verificar se o sistema está fazendo swapping (troca de memória para disco).
Se a coluna si e so não estiverem zeradas, seu servidor está com pouca memória RAM.

💻 iostat -xz 1
- O melhor comando para diagnosticar gargalos em disco. Verifique a coluna %util: se estiver próxima de 100%, seu disco é o gargalo.

💻 free -m
- Visualização rápida da memória RAM livre, usada e em cache.

💻 sar (Sysstat)
-  "caixa preta" do Linux. O comando sar -u (CPU) ou sar -r (Memória) permite ver o histórico de uso ao longo do dia, 
não apenas o momento atual.

---------------------------------------------------
### Logs de Sistema e Aplicação
Onde a verdade está escondida.

💻 journalctl -u <serviço> -xe
- Fundamental para sistemas baseados em systemd. O -e pula para o final, e o -x traz explicações adicionais sobre o erro.

💻 tail -f /var/log/<arquivo>
- O padrão para acompanhar logs em tempo real. 
Dica: use com grep para filtrar apenas o que importa: 💻 tail -f /var/log/syslog | grep -i "error".

💻 dmesg -T | tail -n 50 
- Verifica as mensagens do kernel. Problemas de hardware, travamento de driver ou Out of Memory Killer (OOM) aparecem aqui.

💻 /var/log/syslog ou /var/log/messages
- O repositório central de eventos do sistema.

---------------------------------------------------
### Conectividade e Redes (Conflitos e Portas)
Para problemas onde a aplicação não responde ou está "perdendo" conexões.

💻 ss -tulpn 
- Substitui o antigo netstat. Mostra quais portas estão abertas e qual processo (PID) está "ouvindo" em cada uma.

💻 tcpdump -i any port 80
- Para análise profunda de tráfego. Essencial para verificar se o pacote está chegando ao servidor ou sendo bloqueado pelo firewall.

💻 mtr <destino> 
- Uma mistura de ping e traceroute. Mostra em qual salto (hop) da rede a perda de pacotes ou latência está ocorrendo.

---------------------------------------------------
### Análise de Processos e Conflitos
Quando um processo trava ou causa lentidão extrema.

💻 strace -p <PID> 
- O comando dos especialistas. Ele intercepta as chamadas de sistema (syscalls) do processo. 
Se um programa está "travado" aguardando um arquivo ou rede, o strace vai te mostrar exatamente em qual linha o sistema 
operacional está esperando.

💻 lsof -p <PID>
- Lista todos os arquivos que um processo específico está mantendo abertos. Vital para diagnosticar vazamento de file descriptors.

💻 ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
- Lista os processos que mais consomem recursos, ordenados pelo uso de CPU.

---------------------------------------------------
## 🕒 1. O Poder do Histórico (`history` + `grep`)
Em ambientes de produção, rastrear o que já foi executado ou recuperar aquele comando complexo de madrugada é vital. 
O histórico é a sua primeira linha de diagnóstico.

### Buscar um termo específico no histórico
💻 history | grep "termo_que_voce_quer_buscar"
- Exemplo Prático: Para recuperar a sintaxe exata daquele comando complexo que você esqueceu:
💻 history | grep "log_erros"

- Filtrar os últimos 10 usos de um comando específico.
Explicação: Filtra todas as ocorrências do comando informado e exibe apenas as 10 últimas linhas, evitando poluir a tela do terminal.
💻 history | grep "tail" | tail -n 10

- Rastrear navegação ou uso de ferramentas.
Explicação: Permite mapear por quais diretórios o operador navegou recentemente durante uma sessão de troubleshooting.
💻 history | grep "cd"

---------------------------------------------------
### O "Workflow" do Analista de Elite:
Sintoma: O sistema está lento.

Passo 1 (Recursos):
-  💻 htop 
= (verificar se é CPU ou RAM).

- Passo 2 (Kernel):
💻 dmesg -T 
= (verificar se o kernel matou algum processo por falta de memória).

- Passo 3 (Logs):
  💻 journalctl -u <app> -n 100 
= (verificar o erro específico na aplicação).

- Passo 4 (Deep Dive):
  💻 strace 
= (ver o comportamento interno do processo se o erro persistir).

### Dica final: 
⚙️🛠️ Domine o grep, awk e sed. Saber filtrar a saída desses comandos é o que diferencia quem lê milhares de linhas de log de 
quem encontra o erro em segundos.





