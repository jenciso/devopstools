## INSTALL

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

## Configure your home env

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

Create a host file `~/ansible/hosts`

``` 
[gitlab]
gitlab.enciso.website
```

## Verify setup 

Get version

	ansible --version

Execute adhoc command

	ansible -m ping -u centos all
	ansible -m ping -u centos gitlab

	ansible -m raw -u centos -a "cat /etc/redhat-release" gitlab
	ansible -m shell -u centos -a "cat /etc/redhat-release" gitlab
	ansible -m command -u centos -a "cat /etc/redhat-release" gitlab

https://www.unixarena.com/2018/07/ansible-command-vs-shell-vs-raw-modules.html/	

