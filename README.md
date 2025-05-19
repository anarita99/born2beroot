# born2beroot
# Avaliação

## Como funciona uma maquina virtual?

Uma maquina virtual e um software que simula um computador físico, permitindo que sistemas operativos e aplicacoes sejam executados de forma isolada no hardware do sistema anfitrião.

## Diferença entre Debian e Rocky

Debian - Sistema operativo de código aberto com foco em estabilidade, usa APT como gestor de pacotes.

Rocky - voltado para ambientes empresariais, usa DNF como gestor de pacotes.

## Aptitude vs APT (Advance Packaging Tool)

Ambos sao gestores de pacotes mas o aptitude tem uma interface gráfica mais avançada e suporte para resolver dependências.

## O que e o APPArmor?

Ferramenta de segurança que restringe o acesso de aplicacoes ao sistema.

(sudo aa-status)

## Simple setup

**Verificar se tem interface grafica:**
`ls /usr/bin/*session`

**Output esperado:**

/usr/bin/dbus-run-session

/usr/bin/byebu-session

**Output nao esperado:**
/usr/bin/gnome-session

**Verificar UFW:**

`sudo UFW status`

**Verificar SSH:**

`sudo systemctl status ssh`

**Verificar OS:**

`uname - - Kernel-version`

## User

**Verificar se o user do aluno a ser avaliado existe e esta no grupo sudo e user42:**

`getent group sudo`

`getent group adores42`

**Criar novo user:**

`sudo adduser <user>`

**Como dar setup a pass policy:**

- instalar libpam-pwquality
- Adicionar ao /etc/pam.d/common-password:
    - maxrepeat = 3
    - minlength = 10
    - ucredit = -1
    - lcredit = -1
    - dcredit = -1

**Para ver as regras:**

`sudo vim /etc/pam.d/common-password`

**Password policy:**

`sudo vim /etc/logins.defs`

- chage -M 30 <user>
- chage -m 2 <user>
- chage -l <user> - ver pass policy

Mudar password:

passwd <user>
