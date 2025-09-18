# Máquina Tagged
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 192.168.5.190

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p80,7746 -vvv -Pn 192.168.5.190

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://192.168.5.190/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

ingresamos a la web.

![alt text](image-4.png)

presionamos ctrl + u

![alt text](image-5.png)

verifico el subdominio:

wfuzz -H "Host: FUZZ.tagged.hmv" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://tagged.hmv --hw=5


No encontramos nada

### Explotación

Tenemos el puerto 7746, lo intercepto con netcat

![alt text](image-6.png)

observo el archivo magiccode.go

![alt text](image-7.png)

![alt text](image-8.png)

tengo que hacer el mismo procedimiento anterior pero colocarme en escucha con netcat en el puerto 7777 en la máquina víctima pero debo escribir la palabra Deva

![alt text](image-9.png)

### Escalar privilegios

siendo el usuario shyla:

![alt text](image-10.png)

![alt text](image-11.png)

![alt text](image-12.png)

### user.txt

![alt text](image-13.png)

### root.txt

![alt text](image-14.png)