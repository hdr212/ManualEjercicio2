# ManualEjercicio2
Introducció a Vagrant
Per a cada projecte vagrant utilitza un directori, com a exemple podem crear el directori example per treballar amb una MV d'Ubuntu 22.04 Jammy Jellyfish.

[alumne@elpuig ~]$ mkdir example
[alumne@elpuig ~]$ cd example/

Despres farem aixo 

[alumne@elpuig example]$ vagrant init ubuntu/jammy64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
[alumne@elpuig example]$ ll
total 4
-rw-rw-r--. 1 alumne alumne 3020 15 jul. 14:23 Vagrantfile
[alumne@elpuig example]$

Despres d'aquesta configuració fem aquest pas 
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


vagrant@ubuntu-jammy:~$vagrant@ubuntu-jammy:~$ ll /vagrant
total 8
drwxrwxr-x  1 vagrant vagrant   38 Jul 15 12:32 ./
drwxr-xr-x 20 root    root    4096 Jul 15 12:33 ../
drwxrwxr-x  1 vagrant vagrant   32 Jul 15 12:32 .vagrant/
-rw-rw-r--  1 vagrant vagrant 3020 Jul 15 12:23 Vagrantfile
vagrant@ubuntu-jammy:~$

Per fer visible el servidor apache de la MV a http://localhost:8080 de la màquina física únicament haurem de descomentar la línia següent:

# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080



    Actualització de la màquina.

apt update
apt upgrade

    nstal·lació del servidor web apache2.

apt install -y apache2

    Instal·lació del servidor de bases de dades mysql-server.

apt install -y mysql-server

    Instal·lació d'algunes llibreries de php, el llenguatge principal que utilitzen les aplicacions.

apt install -y php libapache2-mod-php
apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl

    Reiniciem el servidor apache2

systemctl restart apache2

Des d'un terminal on siguem root hem d'executar la següent comanda:

root@elpuig:~$ mysql
Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom bbdd.

CREATE DATABASE bbdd;
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
Sortim de la base de dades

exit
Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

alumne@elpuig:~$ mysql -u usuario -p
Reiniciem el servidor

systemctl restart mysql

Un cop descomprimits els fitxers de l'aplicació web al directori /var/www/html, apliquem els següents permisos al directori /var/www/html

cd /var/www/html
chmod -R 775 .
chown -R root:www-data .

