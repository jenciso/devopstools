## INSTALL Ansible

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

## Configure your ansible config file

```
mkdir ~/ansible
```

Create a file named `~/.ansible.cfg`

```
[defaults]
inventory         = ~/ansible/hosts
roles_path        = ~/ansible/roles
host_key_checking = False

[privilege_escalation]
become=True
become_method=sudo
become_user=root
```

### Test

Create a host inventory file `~/ansible/hosts`

``` 
[gitlab]
gitlab.enciso.website

[windows]
win-ad.enciso.website
```

## Verify setup 

Get ansible version

```
ansible --version
```

Execute adhoc command

```
ansible -m ping -u centos all
ansible -m ping -u centos gitlab

ansible -m raw -u centos -a "cat /etc/redhat-release" gitlab
ansible -m shell -u centos -a "cat /etc/redhat-release" gitlab
ansible -m command -u centos -a "cat /etc/redhat-release" gitlab
```

https://www.unixarena.com/2018/07/ansible-command-vs-shell-vs-raw-modules.html/	

## Ansible Windows 2008 R2

* Acessar pelo Remote Desktop

* Instalar o PowerShell via o Server Manager -> Add Features

* Permitir execução remota

```
Set-ExecutionPolicy RemoteSigned
```

* Upgrade PowerShell. Baixar o script https://raw.githubusercontent.com/jborean93/ansible-windows/master/scripts/Upgrade-PowerShell.ps1
	
* Executar 

```	
./Upgrade-PowerShell.ps1
```

* Instalar Microsoft Net Framework 4.5

  [dotNetFx45_Full_setup.exe](https://www.microsoft.com/en-us/download/confirmation.aspx?id=30653)


* Instalar o pacote Management Framework 3.0

  [Windows6.0-KB2506146-x64.msu](https://www.microsoft.com/en-us/download/details.aspx?id=34595)


* Instalar o hotfix (WinRM Memory)

  https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#winrm-memory-hotfix

* Verificar que seja a versão 3 do powershell

```
$PSVersionTable.PSVersion
```

* Baixar o script para habilitar o Ansible

```
https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

./Ansible.ps1
```

* Executar no ansible host

```
sudo apt-get install -y libkrb5-dev
sudo apt-get install -y python-pip
sudo pip install pywinrm[Kerberos]
sudo pip install --upgrade pip
```

* No ansible host

```
mkdir -p ~/ansible/group_vars/windows
vim main.yml
```

* Copiar o seguinte conteúdo

```
---
ansible_user: Administrator
ansible_password: SuperSecret2012
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
ansible_become: false
```

* Testar

```
ansible -m win_ping windows

ansible -m win_command -a "whoami" windows
ansible -m win_shell -a 'Restart-Computer -Force' windows
ansible -m win_shell -a 'dsquery group -name OPS' windows 
ansible -m win_shell -a 'dsquery user -name Juan*' windows
ansible -m win_shell -a "Get-Hotfix" windows
```

## Notas

Limitar o acesso para algumas IP's

```
no-port-forwarding,from="10.64.4.13,10.64.4.65,10.64.4.66,10.64.4.67" ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAA
BAQC7JqHkL0tE3x7PG4kfyRllyDYKu6/Hr2mX0jtXPa90ibF31EBWIW0ojnyIS5N6NAIcSx/SV3k51bbv4CN75ok7TENZQxgkzZEJw1Da
+3tofx3drSqIzcwEFE/wr37miIVqIEaYuWkcNkiuE7YVACx9NKYbjU3BiPTOVmiZ4sIk3/0t0kYErota4Jkhf09gXjg9zo+zpSvdrax+G
a6pGp8fDP9pXYELNEiqzyxAAYWG4SL root@myserver
```

