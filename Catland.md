# Máquina Catland
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

agregamos el dominio al /etc/hosts

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.26

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.26

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://catland.hmv/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

haciendo ctrl + u: visualicé al usuario laura

![alt text](image-9.png)

busqué subdominios

wfuzz -H "Host: FUZZ.catland.hmv" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://catland.hmv --hw=97

![alt text](image-4.png)

lo agregué al /etc/hosts

Busqué archivos .php dentro del subdominio:

wfuzz -c --hc=404 --hl=6 -t 200 -w /usr/share/seclists/Discovery/Web-Content/big.txt  -z list,php-txt-md 'http://admin.catland.hmv/FUZZ.FUZ2Z'

![alt text](image-5.png)

### Explotación

al entrar al subdominio por la url, me redirige a catland.hmv, entonces lo intercepté con burpsuite y me salté el redirect

![alt text](image-6.png)

le doy en forward y elimino redirectToSubdomain();

![alt text](image-7.png)

y luego en forward otra vez, listo pude acceder

![alt text](image-8.png)

ahora para encontrar la contraseña, lo que hice fue utilizar la herramienta cupp

python3 cupp.py -i

me generó el archivo de contraseñas laura.txt

utilicé la herramienta ffuf para encontrar la contraseña:

ffuf -c -w laura.txt -H 'Content-Type: application/x-www-form-urlencoded' -u http://admin.catland.hmv/index.php -d "username=laura&password=FUZZ" -fs 1068

![alt text](image-10.png)

ingreso al sistema

![alt text](image-11.png)

le di clic en go

![alt text](image-12.png)

tiene pinta de LFI

![alt text](image-13.png)

tenemos al usuario laura

Previamente hicimos fuzzing al subdominio para encontrar archivos, y vimos el archivo upload.php

![alt text](image-14.png)

solo me deja subir archivos comprimidos en .rar o en .zip, entonces lo que hice fue subir un archivo .php comprimido a zip y luego concatenarlo con el LFI

![alt text](image-15.png)

lo subo:

![alt text](image-16.png)

la concatenación

![alt text](image-17.png)

me lanzo una reverse shell con busybox

![alt text](image-18.png)

![alt text](image-19.png)

hice tratamiento de la tty

![alt text](image-20.png)

ingresé a la base de datos y encontré lo siguiente:

![alt text](image-21.png)

![alt text](image-22.png)

entonces busqué archivos de grub:

![alt text](image-23.png)

![alt text](image-24.png)

usé el hash desde grub.pbkdf2.sha512.... hasta ...BBC y lo guardé como password_grub.txt y lo crackié con JohnTheRipper

john --wordlist=/usr/share/wordlists/rockyou.txt password_grub.txt

![alt text](image-25.png)

utilicé esa contraseña para cambiar al usuario laura

### Escalar privilegios

![alt text](image-26.png)

vi el tipo de archivo que era el binario

![alt text](image-27.png)

visualicé el archivo archivo rtv

![alt text](image-28.png)

busqué la librería y sus arcgivos y encontré metadata.py que tiene permisos de escritura:

![alt text](image-29.png)

modifiqué el archivo metadata.py y le agregué la siguiente línea de código:

![alt text](image-30.png)

luego:  

![alt text](image-31.png)

### user.txt

![alt text](image-32.png)

### root.txt

![alt text](image-33.png)