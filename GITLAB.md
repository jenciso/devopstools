## GITLAB

Instalar servidor

	./new-vm.sh -n gitlab -m 4096 -c 2 -i 192.168.122.41

Install and configure the necessary dependencies

```
sudo yum install -y curl policycoreutils-python openssh-server openssh-clients firewalld
sudo systemctl enable sshd
sudo systemctl start sshd

sudo systemctl start firewalld

sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```

Add the GitLab package repository and install the package

	curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

Setup Software

	sudo EXTERNAL_URL="http://gitlab.enciso.website" yum install -y gitlab-ce

Verificando o que está fazendo 

	tail -f /var/log/gitlab/gitlab-rails/production.log

Configurando acesso https

	wget 
	ssh gitlab.enciso.website
	cd /etc/gitlab
	vim gitlab.rb



## GITLAB CONTAINER


Baixando a imagem

```
docker pull gitlab/gitlab-ce
```

Salvando a imagem

```
docker save gitlab/gitlab-ce:latest > /tmp/gitlab-ce-latest.tar
gzip /tmp/gitlab-ce-latest.tar

gunzip /tmp/gitlab-ce-latest.tar.gz
docker load -i /tmp/gitlab-ce-latest.tar
```


## REFERENCES:

https://about.gitlab.com/install/#centos-7?version=ce
https://docs.gitlab.com/omnibus/settings/nginx.html#manually-configuring-https


