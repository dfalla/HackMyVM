# Máquina Away
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 192.168.5.188

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p80,3306 -vvv -Pn 192.168.5.188

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://192.168.5.188/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

### Explotación

Accedemos a la web: http://192.168.5.188/

![alt text](image-4.png)

al parecer es un formato de clave ssh, también parece que han dejado las claves shh en el directorio raíz, por lo tanto con curl visualizo las claves:

![alt text](image-5.png)

Me copio el id_id_ed25519 le doy permisos 600 accedo por ssh con el usuario tula coloco como passphrase "Theclockisticking":

![alt text](image-6.png)


### Escalar privilegios

ejecuto el comando sudo -l

![alt text](image-7.png)

webhook

Es una herramienta disponible en GitHub que se utiliza para crear puntos finales HTTP en un servidor para ejecutar comandos configurados.

forma de explotar:

![alt text](image-8.png)

somos el usuario lula:

![alt text](image-9.png)

vemos la capabilities: /usr/sbin/getcap -r / 2>/dev/null

![alt text](image-10.png)

podemos leer archivos que requieren permisos elevados, para este caso veremos el archivo /root/.ssh/id_ed25519

![alt text](image-11.png)

lo copiamos y lo guardamos como id_root, le damos permisos 600 e ingresamos como root por ssh

![alt text](image-12.png)


### user.txt

![alt text](image-13.png)

### root.txt

![alt text](image-14.png)