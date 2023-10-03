# ManualEjercicio2
Introducció a Vagrant
Per a cada projecte vagrant utilitza un directori, com a exemple podem crear el directori example per treballar amb una MV d'Ubuntu 22.04 Jammy Jellyfish.

*[alumne@elpuig ~]$ mkdir example
*[alumne@elpuig ~]$ cd example/

En el primer pas, farem igual que la propera

*[alumne@elpuig example]$ vagrant init ubuntu/jammy64

..
 A `Vagrantfile` has been placed in this directory. You are now
 ready to `vagrant up` your first virtual environment! Please read
 the comments in the Vagrantfile as well as documentation on
 `vagrantup.com` for more information on using Vagrant.

..
*[alumne@elpuig example]$ ll
total 4
-rw-rw-r--. 1 alumne alumne 3020 15 jul. 14:23 Vagrantfile
*[alumne@elpuig example]$

Despres d'aquesta configuració fem aquest pas 

..
*[alumne@elpuig example]$ vagrant up --provider=virtualbox


    default: Clearing any previously set network interfaces...

    default: Preparing network interfaces based on configuration...

    default: Adapter 1: nat
    
     default: Forwarding ports...

    default: 22 (guest) => 2222 (host) (adapter 1)

    default: Running 'pre-boot' VM customizations...

    default: Booting VM...

    default: Waiting for machine to boot. This may take a few minutes...

    default: SSH address: 127.0.0.1:2222

    default: SSH username: vagrant

    default: SSH auth method: private key

    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace

    default: this with a newly generated keypair for better security.
    default: 

    default: Inserting generated public key within guest...

    default: Removing insecure key from the guest if it's present...

    default: Key inserted! Disconnecting and reconnecting using new SSH key...

    default: Machine booted and ready!

    default: Checking for guest additions in VM...

    default: The guest additions on this VM do not match the installed version of

    default: VirtualBox! In most cases this is fine, but in rare cases it can

    default: prevent things such as shared folders from working properly. If you see

    default: shared folder errors, please make sure the guest additions within the

    default: virtual machine match the version of VirtualBox you have installed on

    default: your host and reload your VM.
    default: 

    default: Guest Additions Version: 6.0.0 r127566

    default: VirtualBox Version: 6.1

    default: Mounting shared folders...

    default: /vagrant => /home/alumne/example


*[alumne@elpuig example]$

Aquesta comanda descàrrega (les màquines descarregades es guarden a ~/.vagrant.d/) —si cal— la màquina utilitzada, crea una MV a VirtualBox, la configura, l'encén i la prepara amb la clau pública del host per poder iniciar sessió amb ssh.

**[alumne@elpuig example]$ vagrant ssh

Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul 15 12:38:47 UTC 2021

  System load:  0.0               Processes:               110
  Usage of /:   3.2% of 38.71GB   Users logged in:         0
  Memory usage: 19%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%


1 update can be applied immediately.
To see these additional updates run: apt list --upgradable



*vagrant@ubuntu-jammy:~$vagrant@ubuntu-jammy:~$ ll /vagrant


total 8

drwxrwxr-x  1 vagrant vagrant   38 Jul 15 12:32 ./

drwxr-xr-x 20 root    root    4096 Jul 15 12:33 ../

drwxrwxr-x  1 vagrant vagrant   32 Jul 15 12:32 .vagrant/

-rw-rw-r--  1 vagrant vagrant 3020 Jul 15 12:23 Vagrantfile


*vagrant@ubuntu-jammy:~$

Per fer visible el servidor apache de la MV a http://localhost:8080 de la màquina física únicament haurem de descomentar la línia següent:


config.vm.network "forwarded_port", guest: 80, host: 8080



    Actualització de la màquina.

*apt update


*apt upgrade

    Instal·lació del servidor web apache2.


*apt install -y apache2

    Instal·lació del servidor de bases de dades mysql-server.

*apt install -y mysql-server

    Instal·lació d'algunes llibreries de php, el llenguatge principal que utilitzen les aplicacions.

*apt install -y php libapache2-mod-php
*apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl

    Reiniciem el servidor apache2

*systemctl restart apache2

Des d'un terminal on siguem root hem d'executar la següent comanda:

*root@elpuig:~$ mysql
Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom bbdd.

*CREATE DATABASE bbdd;

*CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

*GRANT ALL ON bbdd.* to 'usuario'@'localhost';


Sortim de la base de dades

*exit
Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

*alumne@elpuig:~$ mysql -u usuario -p

Reiniciem el servidor

*systemctl restart mysql

Un cop descomprimits els fitxers de l'aplicació web al directori /var/www/html, apliquem els següents permisos al directori /var/www/html

**cd /var/www/html

chmod -R 775 .

chown -R root:www-data .


SEGONA VERSIÓ 
Introducció a Vagrant

Vagrant és una eina lliure per a la creació i el treball amb entorns de desenvolupament. Aquests entorns de desenvolupament se sustenten sobre alguna eina de virtualització com VirtualBox, libvirt o Docker, per la qual cosa a la pràctica Vagrant ens permetrà definir en un arxiu Vagrantfile la nostra infraestructura.

A partir del fitxer Vagrantfile l'eina vagrant s'encarregarà de:

    Descarregar les imatges
    Construir les MVs amb la configuració especificada
    Executar les tasques necessàries per instal·lar programari al seu interior, crear usuaris, ...
    La gestió (creació, encès, desament, detenció i destrucció) de les MVs
    Compartir el directori del projecte amb les MVs
    Gestioneu l'accés amb ssh

D'aquesta manera és molt senzill desplegar infraestructura a partir d'un fitxer i, quan ja no calgui, esborrar el directori del projecte i deixar la màquina neta.
Un directori per al projecte

Per a cada projecte vagrant utilitza un directori, com a exemple podem crear el directori example per treballar amb una MV d'Ubuntu 22.04 Jammy Jellyfish.

[alumne@elpuig ~]$ mkdir example
[alumne@elpuig ~]$ cd example/

La configuració del projecte s'escriu al fitxer Vagrantfile que podem crear directament amb l'ordre vagrant init <box> indicant una de les MVs que es troben a Vagrant Cloud. Per exemple, la distribució Ubuntu manté imatges oficials a https://app.vagrantup.com/ubuntu.

[alumne@elpuig example]$ vagrant init ubuntu/jammy64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
[alumne@elpuig example]$ ll
total 4
-rw-rw-r--. 1 alumne alumne 3020 15 jul. 14:23 Vagrantfile
[alumne@elpuig example]$

Ara podem aixecar tota la infraestructura (afegint el paràmetre --provider=virtualbox perquè en algunes màquines el provider per defecte és libvirt) amb vagrant up:

[alumne@elpuig example]$ vagrant up --provider=virtualbox
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 6.0.0 r127566
    default: VirtualBox Version: 6.1
==> default: Mounting shared folders...
    default: /vagrant => /home/alumne/example

[alumne@elpuig example]$

Aquesta comanda descàrrega (les màquines descarregades es guarden a ~/.vagrant.d/) —si cal— la màquina utilitzada, crea una MV a VirtualBox, la configura, l'encén i la prepara amb la clau pública del host per poder iniciar sessió amb ssh.

[alumne@elpuig example]$ vagrant ssh
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul 15 12:38:47 UTC 2021

  System load:  0.0               Processes:               110
  Usage of /:   3.2% of 38.71GB   Users logged in:         0
  Memory usage: 19%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%


1 update can be applied immediately.
To see these additional updates run: apt list --upgradable


vagrant@ubuntu-jammy:~$

A més, per defecte s'ha compartit el directori del projecte (~/example) entre la màquina física i la màquina virtual. Així resulta molt senzill compartir fitxers ja que el directori del projecte estarà muntat a la MV al directori /vagrant:

vagrant@ubuntu-jammy:~$ ll /vagrant
total 8
drwxrwxr-x  1 vagrant vagrant   38 Jul 15 12:32 ./
drwxr-xr-x 20 root    root    4096 Jul 15 12:33 ../
drwxrwxr-x  1 vagrant vagrant   32 Jul 15 12:32 .vagrant/
-rw-rw-r--  1 vagrant vagrant 3020 Jul 15 12:23 Vagrantfile
vagrant@ubuntu-jammy:~$

La comanda vagrant status ens mostrarà informació sobre l'estat de les nostres MVs i les podrem aturar amb vagrant halt o suspendre amb vagrant suspend. En qualsevol cas, es podran tornar a encendre amb vagrant up.

Quan ja no siguin necessàries les MVs es podran esborrar amb vagrant destroy.
Algunes opcions bàsiques de Vagrantfile

L'entorn que prepara vagrant està definit al fitxer Vagrantfile del projecte. Podeu consultar la documentació sobre els fitxers Vagrantfile a https://www.vagrantup.com/docs/vagrantfile.

El fitxer Vagrantfile creat de manera automàtica al pas anterior té moltes línies comentades que mostren com realitzar algunes operacions senzilles.
Memòria assignada a la MV

La configuració per defecte del nostre Vagrantfile no especifica la quantitat de memòria per a la nostra màquina virtual així que s'utilitza 1GB.

Però podem descomentar les línies següents del nostre fitxer Vagrantfile i canviar especificar el valor de memòria per a la nostra màquina.

config.vm.provider "virtualbox" do |vb|
  # Display the VirtualBox GUI when booting the machine
  # vb.gui = true

  # Customize the amount of memory on the VM:
  vb.memory = "2048"
end

Preparar la MV una cop creada

Un cop creada la MV a partir de la imatge oficial, s'haurà de preparar d'alguna manera perquè compleixi la funció esperada per l'usuari.

Per aquesta tasca vagrant utilitza diferents provisioners entre els quals es troba l'intèrpret shell i Ansible. Amb ells es pot especificar qualsevol tasca automàtica que s'hagi de fer per preparar les MVs.

Per exemple, si volguéssim instal·lar un servidor web apache a la nostra MV podrem trobar un exemple comentat al nostre Vagrantfile.

Si descomentem les línies següents:

config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  apt-get install -y apache2
SHELL

S'instal·larà el servidor web apache de manera automàtica en crear l'entorn, encara que sense afegir una redirecció per a un port o una nova interfície encara no serà possible accedir al servidor web des de la nostra màquina.
Redirecció de ports

La manera més senzilla d'accedir a un servei, com el nostre servidor Apache, que s'executa a la MV és redirigir el trànsit d'un port de la nostra màquina física a la MV.

Com ja resulta habitual trobarem un exemple preparat al nostre Vagrantfile que precisament redirigeix el trànsit del port 8080 TCP de la nostra màquina física al port 80 TCPde la MV.

Per fer visible el servidor apache de la MV a http://localhost:8080 de la màquina física únicament haurem de descomentar la línia següent:

# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080

Xarxa privada

També és possible afegir una interfície de xarxa nova a la MV en una xarxa privada diferent de la xarxa a la qual està connectat l'ordinador amfitrió.

Descomentant la següent línia del fitxer Vagrantfile:

# Create a private network, which allows host-only access to the machine
# using a specific IP.
config.vm.network "private_network", ip: "192.168.33.10"

S'afegirà una interfície de xarxa nova a la MV amb la IP 192.168.33.10 i a l'equip amfitrió s'afegirà automàticament una interfície de xarxa vboxnet0 (si és la primera) amb la IP 192.168.33.1 per permetre la comunicació.

La xarxa privada també pot servir per comunicar diferents MVs que sexecuten a l'equip.

Vagrant sap com configurar la xarxa en diferents sistemes operatius, per exemple al nostre Ubuntu 20.04 s'ha afegit el fitxer /etc/netplan/50-vagrant.yaml amb la configuració de la nova interfície:

---
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s8:
      addresses:
      - 192.168.33.10/24

També serà possible assignar les adreces de manera automàtica utilitzant:

config.vm.network "private_network", type: "dhcp"

2.0

Instal·lació i configuració d'aplicacions web

Per instal·lar una aplicació web hem de baixar el seu codi font i portar-lo al directori arrel del nostre servidor d'aplicacions, en el nostre cas, apache2. Quan instal·lem apache2 es crea una carpeta a /var/www/html on, per defecte, el servidor web utilitza com a directori arrel.

Llavors, si portem la nostra aplicació al directori /var/www/html tindrem accés a la nostra aplicació mitjaçant l'adreça http://localhost.
Instal·lació d'apache2, mysql i algunes llibreries al contenidor

    Actualització de la màquina.

apt update
apt upgrade

    Instal·lació del servidor web apache2.

apt install -y apache2

    Instal·lació del servidor de bases de dades mysql-server.

apt install -y mysql-server

    Instal·lació d'algunes llibreries de php, el llenguatge principal que utilitzen les aplicacions.

apt install -y php libapache2-mod-php
apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl

    Reiniciem el servidor apache2

systemctl restart apache2

Configuració de MySQL
Accedim a la consola de MySQL

Des d'un terminal on siguem root hem d'executar la següent comanda:

root@elpuig:~$ mysql

Creació de la base de dades:

Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom bbdd.

CREATE DATABASE bbdd;

Creació d'un usuari

Tingueu en compte que s'haurà d'identificar la IP des de la qual s'accedirà a la base de dades, en aquest cas, localhost.

CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

Donem privilegis a l'usuari:

GRANT ALL ON bbdd.* to 'usuario'@'localhost';

Sortim de la base de dades

exit

Probem la connexió a la base de dades

Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

alumne@elpuig:~$ mysql -u usuario -p

Extra: permetre la connexió des d'una màquina remota

Per seguretat, MySQL no permet per defecte connexions que no siguin des de localhost. Si volem canviar aquest comportament hem de crear un altre usuari que accedirà des d'una màquina remota i estarà identificat pel nom d'usuari i la seva IP. Així doncs, poden existir diferents usuaris anomenats usuario que connecten des de diferents màquines.
Canviem l'accés per defecte a la nostra màquina

Permetem l'accés des de qualsevol equip a la nostra base de dades.

cat /etc/mysql/mysql.conf.d/mysqld.cnf | grep bind-address
bind-address = 127.0.0.1

Hem de canviar bind-address per 0.0.0.0

bind-address = 0.0.0.0

Reiniciem el servidor

systemctl restart mysql

Creació d'un usuari per a accedir des d'una màquina remota

Per accedir des d'una màquina remota, hauriem de crear un usuari nou identificat pel nom d'usuari i la IP de la màquina des de la qual accedirà.

CREATE USER 'usuario'@'192.168.22.100' IDENTIFIED WITH mysql_native_password BY 'password';

Hem de donar privilegis a l'usuari que accedirà des de la màquina remota. Per accedir des de fora, hauriem de donar-li també privilegis a l'usuari a l'altra màquina:

GRANT ALL ON bbdd.* to 'usuario'@'192.168.22.100';

Aplicació de permisos a les nostres aplicacions web

Un cop descomprimits els fitxers de l'aplicació web al directori /var/www/html, apliquem els següents permisos al directori /var/www/html

cd /var/www/html
chmod -R 775 .
chown -R root:www-data .
