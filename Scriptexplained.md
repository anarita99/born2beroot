# Script monitoring.sh

### Linha `#!/bin/bash`

bash

`#!/bin/bash`

- Esta linha indica que o script deve ser executado usando o interpretador `bash`.

---

### 1. Informações da arquitetura do sistema

bash

`arch=$(uname -a)
printf "#Architecture: $arch\n"`

- `uname -a`: Obtém informações sobre o kernel e a arquitetura do sistema.
- `arch`: Variável que armazena o resultado do comando.
- `printf`: Exibe a arquitetura coletada.

---

### 2. Número de CPUs físicas

bash

`pcpu=$(cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l)
printf "#CPU physical: $pcpu\n"`

- `/proc/cpuinfo`: Arquivo do sistema que contém informações sobre os CPUs.
- `grep "physical id"`: Filtra linhas contendo `physical id` (indica CPUs físicas).
- `sort | uniq`: Remove duplicatas para contar apenas CPUs únicas.
- `wc -l`: Conta o número de CPUs físicas.
- `pcpu`: Variável que armazena o número de CPUs físicas.

---

### 3. Número de CPUs virtuais (vCPUs)

bash

`vcpu=$(cat /proc/cpuinfo | grep "processor" | sort | uniq | wc -l)
printf "#vCPU: $vcpu\n"`

- `grep "processor"`: Filtra as linhas que indicam processadores (núcleos virtuais).
- `vcpu`: Variável que armazena o número de núcleos virtuais (vCPUs).

---

### 4. Uso da memória RAM

bash

`fram=$(free -m | grep Mem | awk '{print $2}')
uram=$(free -m | grep Mem | awk '{print $3}')
pram=$(free -m | grep Mem | awk '{print $3/$2 * 100}')
printf "#Memory Usage: $uram/$fram%s (%.2f%%)\n" "MB" "$pram"`

- `free -m`: Mostra o uso da memória em megabytes.
- `grep Mem`: Filtra a linha com informações da memória.
- `awk '{print $2}'`: Extrai a quantidade total de memória (`fram`).
- `awk '{print $3}'`: Extrai a memória usada (`uram`).
- `awk '{print $3/$2 * 100}'`: Calcula a porcentagem de uso da memória (`pram`).
- `printf`: Exibe o uso de memória no formato `usada/total (porcentagem)`.

---

### 5. Uso do disco

bash

`sdisk=$(df -h --total | awk END'{print $2}' | cut -d G -f1)
udisk=$(df -h --total | awk END'{print $3}' | cut -d G -f1)
pdisk=$(df -h --total | awk END'{print $3/$2 * 100}')
printf "#Disk Usage: $udisk/$sdisk%s (%.2f%%)\n" "Gb" "$pdisk"`

- `df -h --total`: Mostra o uso total do disco em formato legível.
- `awk END'{print $2}'`: Pega o total do disco (`sdisk`).
- `awk END'{print $3}'`: Pega o usado (`udisk`).
- `awk END'{print $3/$2 * 100}'`: Calcula o uso percentual (`pdisk`).
- `cut -d G -f1`: Remove o sufixo `G` (gigabytes).
- `printf`: Exibe o uso de disco no formato `usado/total (porcentagem)`.

---

### 6. Carga da CPU

bash

`cpul=$(mpstat | tail -n 1 | awk '{print 100-$NF}')
printf "#CPU load: %.1f%%\n" "$cpul"`

- `mpstat`: Mostra estatísticas de uso da CPU.
- `tail -n 1`: Pega a última linha com dados relevantes.
- `awk '{print 100-$NF}'`: Calcula a carga da CPU (100% menos o tempo ocioso).
- `cpul`: Variável que armazena a carga da CPU.

---

### 7. Última inicialização do sistema

bash

`boot=$(who -b | awk '{print $3" "$4}')
printf "#Last boot: $boot\n"`

- `who -b`: Mostra a última vez que o sistema foi inicializado.
- `awk '{print $3" "$4}'`: Extrai a data e hora da inicialização.
- `boot`: Variável que armazena a última inicialização.

---

### 8. Verificação de uso de LVM

bash

`lvm=$(lsblk | grep "lvm" | wc -l)
if [ $lvm -eq 0 ]
thenprintf "#LVM use: no\n"elseprintf "#LVM use: yes\n"fi`

- `lsblk`: Mostra informações sobre dispositivos de bloco.
- `grep "lvm"`: Verifica se há volumes lógicos (LVM).
- `wc -l`: Conta o número de ocorrências.
- Condicional:
    - Se `lvm` for 0, exibe `LVM use: no`.
    - Caso contrário, exibe `LVM use: yes`.

---

### 9. Conexões TCP ativas

bash

`ctcp=$(ss | grep "tcp" | wc -l)
printf "#Connections TCP: $ctcp %s\n" "ESTABLISHED"`

- `ss`: Mostra conexões ativas.
- `grep "tcp"`: Filtra conexões TCP.
- `wc -l`: Conta o número de conexões TCP ativas.
- `ctcp`: Variável que armazena a contagem.

---

### 10. Número de usuários conectados

bash

`usr=$(users | wc -w)
printf "#User log: $usr\n"`

- `users`: Mostra os usuários atualmente conectados.
- `wc -w`: Conta o número de palavras (usuários).
- `usr`: Variável que armazena a contagem de usuários conectados.

---

### 11. Informações de rede (IP e MAC)

bash

`ip=$(hostname -I | awk '{print $1}')
mac=$(ip address | grep "ether" | awk '{print $2}')
printf "#Network: IP $ip ($mac)\n"`

- `hostname -I`: Obtém os endereços IP do sistema.
- `awk '{print $1}'`: Extrai o primeiro IP.
- `ip address`: Mostra informações sobre interfaces de rede.
- `grep "ether"`: Filtra a linha com o endereço MAC.
- `awk '{print $2}'`: Extrai o endereço MAC.
- `ip` e `mac`: Variáveis que armazenam IP e MAC.

---

### 12. Número de comandos sudo executados

bash

`sudo=$(cat /var/log/sudo/sudo.log | grep "COMMAND" | wc -l)
printf "#Sudo: $sudo\n"`

- `/var/log/sudo/sudo.log`: Arquivo de log do `sudo`.
- `grep "COMMAND"`: Filtra linhas com comandos executados via `sudo`.
- `wc -l`: Conta o número de comandos.
- `sudo`: Variável que armazena essa contagem.
