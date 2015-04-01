layout: oishi
title: Provisionando um servidor Oracle com Vagrant e Shell
date: 2015-03-27 11:35:32
tags:
---
Recentemente precisei criar um servidor Oracle para testes. Quem já trabalhou ou trabalha com Oracle sabe o quão oneroso é tal atividade. Como não queria passar por isso novamente, resolvi automatizar (ainda mais tendo certeza que seria necessário criar novos servidores). 

<!-- more -->
Para criar o servidor usei o [Vagrant](https://www.vagrantup.com/) e [Shell](http://docs.vagrantup.com/v2/provisioning/shell.html) como provisioner. Ao procurar uma box na [Vagrant Cloud](https://atlas.hashicorp.com/boxes/search) encontrei uma com Red Hat 6.5 e o Oracle 11g instalado. A box [bseller/oracle-standard](https://vagrantcloud.com/bseller/boxes/oracle-standard) continha 14GB de disco, com isso foi necessário [aumentá-lo](http://acfreitas.com/2015/03/Aumentando-o-disco-com-Vagrant-e-VirtualBox/).

### Requisitos

Primeiro será necessário instalar o [VirtualBox](https://www.virtualbox.org/) e o [Vagrant](https://www.vagrantup.com/)

### Criando um novo servidor 

O projeto completo está no [GitHub](https://github.com/acfreitas/oraclebox). Para usá-lo, basta seguir os passos abaixo: 

````bash
$ git clone https://github.com/acfreitas/oraclebox.git
$ cd oraclebox
$ vagrant up
````

Após executar o ``vagrant up`` o servidor será criado, com o banco ``tests``. Para alterar as configurações do banco criado durante o provisionamento, bastar editar o [provision.sh](https://github.com/acfreitas/oraclebox/blob/master/shell/provision.sh). 

````bash
#!/bin/bash
echo "Connect with user oracle"
sudo su -l oracle

echo 'Import environment variables'
export ORACLE_SID=tests
export ORACLE_BASE=/u01/app/oracle 
export ORACLE_HOME=/u01/app/oracle/product/11.2/dbhome_1 
export PATH=$PATH:$ORACLE_HOME/bin 
  
echo 'Create database Tests'
if [ ! -d /u01/app/oracle/oradata/tests ] ; then
    su -c "dbca -silent -createDatabase -templateName General_Purpose.dbc -gdbName tests -sysPassword 0r4c13b0x -systemPassword 0r4c13b0x -scriptDest /u01/app/oracle/oradata/tests -characterSet WE8ISO8859P1" -s /bin/sh oracle
fi

echo 'Start listener'
su -c "lsnrctl start" -s /bin/sh oracle

echo 'Startup database'
su -c "sqlplus /nolog <<EOF
    conn 0r4c13b0x/sys as sysdba
    shutdown immediate;
    startup
    exit
EOF" -s /bin/sh oracle
````
___Obs.:___ O comando [DBCA](http://docs.oracle.com/cd/B28359_01/server.111/b28310/create002.htm) cria um novo banco com o characterSet ``WE8ISO8859P1`` e senha ``0r4c13b0x`` para os usuários ``system`` e ``sys``. Para alterar, basta editar o comando:

````bash
dbca -silent -createDatabase -templateName General_Purpose.dbc -gdbName tests -sysPassword 0r4c13b0x -systemPassword 0r4c13b0x -scriptDest /u01/app/oracle/oradata/tests -characterSet WE8ISO8859P1
````

O servidor contém 4GB de memória RAM. Caso queira mudar, bastar editar o [Vagrantfile](https://github.com/acfreitas/oraclebox/blob/master/Vagrantfile)

````
Vagrant.configure(2) do |config|
  config.vm.box = "bseller/oracle-standard"
  config.vm.define :oracle do |oracle| 
    oracle.vm.hostname = 'oraclebox'
    oracle.vm.synced_folder ".", "/vagrant", owner: "oracle", group: "oinstall" 
    oracle.vm.network :private_network, ip: '192.168.33.13'
    oracle.vm.network :forwarded_port, guest: 1521, host: 1521
    oracle.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "4096"]
       vb.customize ["modifyvm", :id, "--name", "oraclebox"]
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
    oracle.vm.provision "shell", path: "shell/add-oracle-disk.sh"
    oracle.vm.provision "shell", path: "shell/provision.sh"
  end
end
````

### Acessando o banco

Para acessar o banco, você pode usar os parâmetros:
* Host: localhost
* Port: 1521
* SID: tests
* User: system
* Password: 0r4c13b0x

Caso queira acessar de outra maquina na rede, basta mudar o host para o IP da sua maquina. 

### Conclusão 

Com isso a criação de novos servidores se torna muito simples, rápido, repetível e portável. Assim é possível garantir que problemas de ambiente não aconteçam, como *characterset*, por exemplo. Além de explorar o poder da virtualização e criar vários servidores com um custo acessível. 
 
Espero ter ajudado. Abraços!