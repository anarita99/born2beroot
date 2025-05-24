#Avaliação
## Como funciona uma máquina virtual?

Uma máquina virtual é um software que simula um computador físico, permitindo que sistemas operativos e aplicações sejam executados de forma isolada no hardware do sistema anfitrião.

## Diferença entre Debian e Rocky

**Debian** - Sistema operativo de código aberto com foco em estabilidade, usa APT como gestor de pacotes.

**Rocky** - voltado para ambientes empresariais, usa DNF como gestor de pacotes.

## Aptitude vs APT (Advance Packaging Tool)

Ambos são gestores de pacotes mas o aptitude tem uma interface gráfica mais avançada e suporte para resolver dependências.

## O que e o APPArmor?

Ferramenta de segurança que restringe o acesso de aplicacoes ao sistema.

(sudo aa-status)

## Simple setup

**Verificar se tem interface grafica:**
`ls /usr/bin/*session`

**Output esperado:**

/usr/bin/dbus-run-session

/usr/bin/byebu-session

**Output não esperado:**
/usr/bin/gnome-session

**Verificar UFW:**

`sudo UFW status`

**Verificar SSH:**

`sudo systemctl status ssh`

**Verificar OS:**

`uname - - Kernel-version`

## User

**Verificar se o user do aluno a ser avaliado existe e está no grupo sudo e user42:**

`getent group sudo`

`getent group adores42`

**Criar novo user:**

`sudo adduser <user>`

**Como dar setup à pass policy:**

- instalar libpam-pwquality
- Adicionar ao /etc/pam.d/common-password:
    - maxrepeat = 3
    - minlength = 10
    - ucredit = -1 (pelo menos 1 uppercase letter)
    - lcredit = -1 (pelo menos 1 lowercase letter)
    - dcredit = -1 (pelo menos 1 digito)

**Para ver as regras:**

`sudo vim /etc/pam.d/common-password`

**Password policy:**

`sudo vim /etc/logins.defs`

- `chage -M 30 <user>`
- `chage -m 2 <user>`
- `chage -l <user> - ver pass policy`

**Mudar password:**

`passwd <user>`

**Criar um grupo:**

`sudo groupadd evaluating`

**Adicionar grupo ao novo user:**

`sudo usermod -aG evaluating <user>`

ou

`sudo adduser <user> evaluating`

- `getent group evaluating`
- `groups <user>`

## Hostname and Partitions

**Verificar hostname:**

`hostname`

**Mudar hostname:**

- mudar para root
- `cd vim /etc/hostname`
- mudar nome
- reboot

OU:

`sudo hostnamectl set-hostname <new_hostname>`

**Como ver as particoes:**

`lsblk`

**Como funciona o LVM?**

LVM significa Logical Volume Manager, e é uma tecnologia que permite gerir o espaço de armazenamento de uma forma mais flexível do que as partições tradicionais. 

Volume group combina os discos ou partições num único grupo de volume. Junta vários discos num só “recipiente”.

Logical volume group são divisões flexíveis onde se pode guardar o que quisermos.

## SUDO

**Checar sudo:**

`which sudo`

**Adicionar novo user ao grupo sudo:**

`sudo adduser <user> sudo`

`getent group sudo`

`sudo visudo`

Sudo - permissões de administrador (Superuser do)

**Verificar file:**

`cd /var/log/sudo`

- ver logs

## UFW

**Checar UFW:**

`sudo service UFW status`

**O que é o UFW?**

Uncomplicated Firewall, e usado em sistemas baseados em Debian. Protege contra acessos não autorizados e ataques.

**Rules:**

`sudo UFW status numbered`

**Criar nova rule:**

`sudo UFW allow 8080`

`sudo UFW status numbered`

`sudo UFW delete num_rule`

## SSH

Protocolo de rede criptográfico para operação de serviços de rede de forma segura sobre uma rede insegura.

Serve para aceder a outro computador remotamente de forma segura.

**Checar SSH:**

which ssh

**Para ver se esta a correr:**

`sudo service ssh status`

**Para usar:**

`ssh user@ip -p 4242`

Primeiro tentar conectar com root e ver que não funciona:

`ssh root@ip -p 4242`

Depois mostramos no ficheiro:

/etc/ssh/sshd_config

- PermitRootLogin no

## Script monitoring

Para ver o crontab:

sudo crontab -e

Minute, hour, day of month, month and day of week.

Para parar :

sudo systemctl stopcron
