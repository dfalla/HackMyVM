# Máquina Messages
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.33

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,25,80,110,143,443,465,587,993,995 -vvv -Pn 10.0.2.33

![alt text](image-2.png)

![alt text](image-3.png)

![alt text](image-4.png)

![alt text](image-5.png)

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)

### Fuzzing Web

feroxbuster --url https://10.0.2.33/ -w /usr/share/seclists/Discovery/Web-Content/big.txt --insecure

![alt text](image-10.png)

![alt text](image-11.png)

### Explotación

Entré a la web:

![alt text](image-12.png)

vi la versión de chatbot y encontré el exploit

![alt text](image-13.png)

inicié sesión

![alt text](image-14.png)

una vez iniciada sesión aplico lo de este exploit que se trata de un RCE  https://www.exploit-db.com/exploits/50672

en Settings cargo un reverse_shell en php en Bot Avatar me pongo en escucha con netcat

![alt text](image-15.png)

en /var/www/html/chatbot encontré el archivo initialize.php

![alt text](image-16.png)

inicié sesión a la base de datos:

![alt text](image-17.png)

![alt text](image-18.png)

![alt text](image-19.png)

copie las contraseñas y las guardé en un archivo pass.txt´y luego crackié con johntheripper

![alt text](image-20.png)

inicié sesión con ruby@messages.hmv y la contraseña encontrada

![alt text](image-21.png)

en Inbox encontré un id_rsa que le pertenece a Ruby

![alt text](image-22.png)

lo copie y me conecté por ssh con el usuario ruby

![alt text](image-23.png)

### Escalar privilegios

![alt text](image-24.png)

![alt text](image-25.png)

![alt text](image-26.png)

![alt text](image-27.png)

### user.txt

![alt text](image-28.png)

### root.txt

![alt text](image-29.png)

