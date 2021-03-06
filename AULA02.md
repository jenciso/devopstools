## SSH configuração

* Criar umas chaves SSH 

```
ssh-keygen
```

* Instalar SSH server

```
sudo apt-get -y install openssh-server
``` 

* Averiguar tua IP Address

```
ip addr | grep "inet " | egrep -v "(docker0|virbr0)" | grep 'scope global'
```

* Copiando chaves

```
ssh-copy-id target@10.0.1.149
```

* Testando a execução de comando remoto

```
ssh target@10.0.1.149 "date"
ssh target@10.0.1.149 "cat myarchive.txt"
```

## KVM CUSTOM PROVISION

* Olhar o arquivo `config.conf`


* Criar um password novo. Ex `target`

```
openssl passwd -1 -salt SaltSalt target
```

* Copiar tua chave publica q fica aqui 

``` 
cat ~/.ssh/id_rsa.pub 
```
* Editar as seguintes variaveis com os valores obtidos do processo anterior, escolher 20G para o disco 

```
 	PASSWORD
	SSH_KEY
	DISK_SIZE
``` 

* criar um servidor `demo01`

```
./new-vm.sh -n demo01 -m 1024 -c 2 -i 192.168.122.12
```

* Verificar entrando na console

```
virsh console demo01
``` 

Exemplo:
```
✔ 14:31:07 [lat3480i7] kvm-provision [master ✚1] $ virsh console demo01
Connected to domain demo01
Escape character is ^]
[  OK  ] Reached target Multi-User System.
         Starting Update UTMP about System Runlevel Changes...
[  OK  ] Started Update UTMP about System Runlevel Changes.
ci-info: ++++++++++Authorized keys from /home/centos/.ssh/authorized_keys for user centos+++++++++++
ci-info: +---------+-------------------------------------------------+---------+-------------------+
ci-info: | Keytype |                Fingerprint (md5)                | Options |      Comment      |
ci-info: +---------+-------------------------------------------------+---------+-------------------+
ci-info: | ssh-rsa | 3d:72:de:3e:be:05:03:31:3a:df:7d:7c:de:8a:51:91 |    -    | jenciso@lat3480i7 |
ci-info: +---------+-------------------------------------------------+---------+-------------------+
[  OK  ] Started cloud-final.service.

CentOS Linux 7 (Core)
Kernel 3.10.0-957.27.2.el7.x86_64 on an x86_64

demo01 login: centos
Password:
[centos@demo01 ~]$
``` 

Para sair digitar: `CTRL + ]`


* Testar o acesso ssh 

```
ssh centos@192.168.122.12
```

* Executar alguns comandos na VM

```
cat /etc/hostname
ip addr
netstat -arn
sudo su -
cat /etc/sudoers.d/90-cloud-init-users
cat /etc/redhat-release
reboot
```

* Verificar as VM's criadas, executar na Hospedeira

```
virt-manager
virsh list --all
```

