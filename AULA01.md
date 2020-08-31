## SUDO

```
sudo su -
echo "targettrust ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/admin
exit
```

## GIT BASH PROMPT

Baixar o repo
```
cd ~
git clone https://github.com/jenciso/bash-git-prompt.git .bash-git-prompt --depth=1
```
Adicionar as linhas em `~/.bashrc`
```
## Bash-git
GIT_PROMPT_ONLY_IN_REPO=0
GIT_PROMPT_THEME=Single_line_Minimalist
source ~/.bash-git-prompt/gitprompt.sh
```

## KVM

Verificar se temos suporte para seu uso

```
egrep -c '(vmx|svm)' /proc/cpuinfo
sudo apt install cpu-checker
sudo kvm-ok
``` 

Instalar os seguintes pacotes

```
sudo apt update
sudo apt install qemu qemu-kvm libvirt-bin  bridge-utils  virt-manager
``` 

Inicializar o libvirtd service

```
sudo service libvirtd start
sudo update-rc.d libvirtd enable
service libvirtd status
```

Configurar modo bridge

```
sudo su - 
vim /etc/netplan/50-cloud-init.yaml
```

Example de conteúdo 
```
network:
  version: 2
  ethernets:
    enp2s0:
      dhcp4: yes
      dhcp6: no

  bridges:
    br0:
      interfaces: [enp2s0]
      dhcp4: yes
      dhcp6: no
```

Reconfigurar
```
sudo netplan apply
sudo networkctl status -a
sudo reboot
```

> A minha NIC é `enp2s0`, na sua pode ter outro nome como `eth0`

## DOCKER

Instalar

```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update

sudo apt-get install     apt-transport-https     ca-certificates     curl     gnupg-agent     software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world
```

Configurar o usuario docker

```shell
$ sudo usermod -a -G docker $(id -nu)
$ reboot
```

Instalar docker-compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

## Referências

* https://www.linuxtechi.com/install-configure-kvm-ubuntu-18-04-server/
* https://docs.docker.com/install/
* https://docs.docker.com/compose/install/
 
