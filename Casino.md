# Máquina Casino
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.30

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p22,80 -vvv -Pn 10.0.2.30

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://10.0.2.30/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

![alt text](image-3.png)

![alt text](image-4.png)

### Explotación

Al entrar en la web tienes que registrarte e iniciar sesión

![alt text](image-5.png)

luego te dan una cantidad de dinero para que puedas jugar, consumes todo el dinero y te llevara a un SSRF

![alt text](image-6.png)

![alt text](image-7.png)

Comprobando el SSRF

![alt text](image-8.png)

creé el archivo nums desde el 1 al 65535

![alt text](image-9.png)

Busqué puertos en la máquina casino accesibles desde el localhost.

![alt text](image-10.png)

![alt text](image-11.png)

Busqué directorios:

![alt text](image-12.png)

![alt text](image-13.png)

realizo un curl al subdirectorio codebreakers

![alt text](image-14.png)

encuentro un id_rsa

![alt text](image-15.png)

por la web lo visualizo

![alt text](image-16.png)

lo copié, le dí permisos 600 y lo guardé.

![alt text](image-17.png)

![alt text](image-18.png)

### Escalar privilegios

descargué el archivo y visualizo que tipo de archivo es:

![alt text](image-19.png)

lo analicé con ghidra

![alt text](image-20.png)

el binario pide 2 contreaseñas la primera contraseña debemos descifrar la 2da es ultrasecretpassword luego de eso se abrirá una shell de manera paralela se abrira el archivo /opt/root.pass y no se cerrará.

utilicé Ejecución simbólica:

![alt text](image-21.png)

![alt text](image-22.png)

ejecuto el binario pass

![alt text](image-23.png)

![alt text](image-24.png)

ejecuto su root y escribo la contraseña:

![alt text](image-25.png)

### user.txt

![alt text](image-26.png)

### root.txt

![alt text](image-27.png)