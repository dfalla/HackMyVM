# Máquina Stardust
### Reconocimiento de la Ip de la máquina víctima

![alt text](image-2.png)

![alt text](image-3.png)

agregué el dominio al /etc/hosts

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.31

![alt text](image.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.31

![alt text](image-1.png)

### Fuzzing Web

feroxbuster --url http://10.0.2.31/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-4.png)

![alt text](image-5.png)

![alt text](image-6.png)

### Explotación

Entramos en la web 

![alt text](image-7.png)

en el fuzzing encontré el directorio: /files

![alt text](image-8.png)

inicié sesión con glpi:glpi

![alt text](image-9.png)

En la parte de Management > Documents me permite subir archivos, pero por defecto no me permite subir archivos php por lo tanto tengo que agregar la extensión para poder subir archivos y esto lo hago en Setup > Dropdowns > Management > Document types 

Por lo tanto subo un reverse shell

![alt text](image-10.png)

Al subir un archivo php se guarda en /files/_tmp

![alt text](image-11.png)

![alt text](image-12.png)

vi los usuarios:

![alt text](image-16.png)

en /var/www/html/config encontré:

![alt text](image-13.png)

inicié sesión en mysql:

![alt text](image-14.png)

![alt text](image-15.png)

copié el hash bcrypt y lo crackié con jhontheripper

![alt text](image-17.png)

cambié al usuario tally:

![alt text](image-18.png)


### Escalar privilegios

Siendo el usuario tally, el usuario tally tiene permisos ACL en la carpeta opt en el archivo config.json y también existe un archivo meteo que básicamente es un script que se ejecuta cada cierto tiempo y que utiliza el archivo config.json para generar backups de root en /var/backups.

modifico el archivo config.json

![alt text](image-19.png)

![alt text](image-20.png)

copié el archivo a tmp y luego lo descomprimí y me generó:

![alt text](image-21.png)

abri el id_rsa

![alt text](image-22.png)

lo copié e inicié sesión por ssh con el usuario root

![alt text](image-23.png)

### user.txt

![alt text](image-24.png)

### root.txt

![alt text](image-25.png)