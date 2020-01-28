## Mode bridge

```shell
$ cat 50-cloud-init.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp2s0:
      dhcp4: false
      dhcp6: false
  bridges:
    br0:
      interfaces: [enp2s0]
      addresses: [10.0.1.153/24]
      gateway4: 10.0.1.1
      nameservers:
        addresses: [1.1.1.1]
      dhcp4: no
      dhcp6: no
      parameters:
        stp: true
        forward-delay: 4
```

## Mode bridge dhcp

```
$ cat /etc/netplan/50-cloud-init.yaml
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

## Check network status

```shell
networkctl status -a
```
