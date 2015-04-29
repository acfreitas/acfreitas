layout: oishi
title: "Vagrant - Warning: Connection timeout. Retrying..."
date: 2015-02-10 09:04:13
tags:
- Vagrant
- DevOps
---
Ao provisionar um ambiente usando o [Vagrant](https://www.vagrantup.com/) com um Host Windows 8 e um Guest Linux 64bits, [boxes/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) e [chef/debian-6.0.10](https://vagrantcloud.com/chef/boxes/debian-6.0.10), me deparei com o seguinte problema:

<!-- more -->

```
Warning: Connection timeout. Retrying...
Warning: Connection timeout. Retrying...
Warning: Connection timeout. Retrying...
Timed out while waiting for the machine to boot. This means that
Vagrant was unable to communicate with the guest machine within
the configured ("config.vm.boot_timeout" value) time period.

If you look above, you should be able to see the error(s) that
Vagrant had when attempting to connect to the machine. These errors
are usually good hints as to what may be wrong.

If you're using a custom box, make sure that networking is properly
working and you're able to connect to the machine. It is a common
problem that networking isn't setup properly in these boxes.
Verify that authentication configurations are also setup properly,
as well.

If the box appears to be booting properly, you may want to increase
the timeout ("config.vm.boot_timeout") value.
```

Ao pesquisar o problema encontrei, principalmente, as seguintes respostas:
* Aumentar o  ``config.vm.boot_timeout`` do Vagrant, que por padrão é [300 segundos](https://docs.vagrantup.com/v2/vagrantfile/machine_settings.html) (e isso é tempo suficiente. Então, não era esse o problema); e 
* Ativar o modo gui do [VirtualBox](https://docs.vagrantup.com/v2/virtualbox/configuration.html) ``vb.gui = true``. Que não resolveu o problema, além de não ter interesse em trabalhar com a interface do VirtualBox e sim com headless mode. 

O que acontece é que as vezes a tecnologia de virtualização pode ser desativada na BIOS por algum processo ou aplicação. Para resolver o problema basta acessar a BIOS durante o boot e ativar a tecnologia de virtualização em ``Security >> Virtualization``. Algumas placas mãe podem ter o caminho diferente, como ``Advanced BIOS Features >> Virtualization Technology``, por exemplo.

Espero ter ajudado. Abraços! 