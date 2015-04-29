layout: oishi
title: Aumentando o disco com Vagrant e VirtualBox
date: 2015-03-20 10:41:57
tags:
- Vagrant
- DevOps
- Oracle
- VirtualBox
---
Ao provisionar um servidor Oracle Standard com Vagrant e VirtualBox, me deparei com o seguinte problema: espaço em disco. A box utilizada [bseller/oracle-standard](https://vagrantcloud.com/bseller/boxes/oracle-standard) contém 14GB e eu precisava de, no minimo, 30GB. Para solucionar o problema foi necessário aumentar a capacidade do disco da maquina virtual. 

<!-- more -->
````
Vagrant.configure(2) do |config|
config.vm.box = "bseller/oracle-standard"
config.vm.define :oracle do |oracle| 
  oracle.vm.hostname = 'oracle'
  config.vm.synced_folder ".", "/vagrant", owner: "oracle", group: "oinstall" 
  oracle.vm.network :private_network, ip: '192.168.33.13'
  oracle.vm.network :forwarded_port, guest: 1521, host: 1521
  oracle.vm.provider :virtualbox do |vb|
     vb.customize ["modifyvm", :id, "--memory", "3072"]
     vb.customize ["modifyvm", :id, "--name", "oracle"]
     vb.customize [
          'createhd', 
          '--filename', 'disk/oracle', 
          '--format', 'VDI', 
          '--size', 60200
          ] 
     vb.customize [
          'storageattach', :id, 
          '--storagectl', "SATA", 
          '--port', 1, '--device', 0, 
          '--type', 'hdd', '--medium', 'disk/oracle.vdi'
          ]
  end
  oracle.vm.provision "shell", path: "shell/provision.sh"
end
````


Na linha 11 eu criei um novo disco de 50GB com o comando [createhd](https://www.virtualbox.org/manual/ch08.html) do VirtualBox. E na linha 17 eu adiciono o novo disco, que foi criado em: 

```
disk
    |-- oracle.vdi
shell
    |-- provision.sh
Vagrantfile

```

O meu arquivo *shell* cria um novo disco na VM com o comando [fdisk](http://linux.die.net/man/8/fdisk) e estende o disco já existente. 

````bash
set -e
set -x

if [ -f /etc/disk_added_date ] ; then
   echo "disk already added so exiting."
   exit 0
fi

sudo fdisk -u /dev/sdb <<EOF
n
p
1


t
8e
w
EOF

sudo pvcreate /dev/sdb1
sudo vgextend VolGroup /dev/sdb1
sudo lvextend -L64GB /dev/VolGroup/lv_root
sudo resize2fs /dev/VolGroup/lv_root
date > /etc/disk_added_date
```` 

Após o ``$ vagrant up`` a maquina virtual passa ter 64GB: 

````
$ vagrant ssh 
$ df -h
Filesystem                    Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root   64G  7,8G   56G  12% /
tmpfs                         1,5G  686M  759M  48% /dev/shm
/dev/sda1                     485M   33M  427M   8% /boot
vagrant                       356G   62G  294G  18% /vagrant

````

O script foi adaptado de [SHC](http://askubuntu.com/questions/317338/how-can-i-increase-disk-size-on-a-vagrant-vm) para a box [bseller/oracle-standard](https://vagrantcloud.com/bseller/boxes/oracle-standard) e publicado no [Stackoverflow](http://stackoverflow.com/questions/14917353/resizing-disk-space-on-vagrant-box/29154627#29154627) em inglês. E ainda assim, você pode adaptá-lo facilmente para outras boxes. 