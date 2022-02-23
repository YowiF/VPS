---
Title: Despliegue de Aplicación Web Full Stack en VPS
Author: Cristina Herrero Rodríguez, Daniel García Castro y Yoel Fernández Suárez
---

# Despliegue de Aplicación Web Full Stack en VPS

[TOC]

## Parte de virtualBACK

### Creación de la base de datos.

```bash
sudo apt-get isntall mysql-server mysql-client
```

![VPS_BBDD_1.1](Imagenes%20Tarea%20VPS/VPS_BBDD_1.1.jpg)



```bash
sudo mysql_secure_installation
```

![VPS_BBDD_1.2](Imagenes%20Tarea%20VPS/VPS_BBDD_1.2.jpg)



```bash
sudo mysql -u root -p
```

```mysql
alter user 'root'@'localhost' identified with mysql_native_password by 'admin';
create database test_virtual;
show databases;
```

![VPS_BBDD_1.3](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_BBDD_1.1-164554761371212.jpg)



```mysql
create user 'user'@'localhost' by 'user';
select user from mysql.user;
```

![VPS_BBDD_1.4](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_BBDD_1.4.jpg)



```mysql
grant all privileges on test_virtual.* to 'user'@'localhost' with grant option;
quit
```

```bash
mysql -u user -p
```

```mysql
show databases;
```

![VPS_BBDD_1.5](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_BBDD_1.5.jpg)



### Instalar java

Tras instalar java, comprobamos las versiones:

```bash
java -version
javac -version
```

![VPS_JAVA&VSFTPD_2.1](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_JAVA&VSFTPD_2.1.jpg)



Abrimos el archivo `/etc/vsftpd.conf` y descomentamos la siguiente línea:

```bash
write_enable=YES
```

![VPS_JAVA&VSFTPD_2.2](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_JAVA&VSFTPD_2.2.jpg)



```bash
sudo systemctl is-enabled vsftpd.service
```

![VPS_JAVA&VSFTPD_2.3](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_JAVA&VSFTPD_2.3.jpg)



### Utilizar el repositorio 'virtualBACK'

Arbimos el FileZilla para mandar la carpeta `virtualBACK-master` al directorio del usuario `master`:

![VPS_virtualBACK_3.1](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.1.jpg)



```bash
cd virtualBACK-master
ls
mvn clean
```

![VPS_virtualBACK_3.2](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.2.jpg)



Añadimos la siguiente línea en el `pom.xml`:

```xml
<finalName>virtual</finalName>
```

![VPS_virtualBACK_3.3](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.3.jpg)



![VPS_virtualBACK_3.4](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.4.jpg)



```bash
java -jar target/virtual.jar
```

![VPS_virtualBACK_3.5](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.5.jpg)



```bash

```

![VPS_virtualBACK_3.6](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.6.jpg)



```bash
[Unit]
Description=my spring boot app

[Service]
Restart=always
ExecStart=/home/master/virtualBACK-master/target/virtual.jar
SyccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

![VPS_virtualBACK_3.7](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualBACK_3.7.jpg)



## Parte de virtualFRONT

### Utilizar el repositorio 'virtualFRONT'

```bash
sudo systemctl start spring.service
sudo systemctl is-active spring.service
sudo apt-get install apache2
sudo systemctl is-enabled apache2
sudo systemctl is-active apache2
```

![VPS_virtualFRONT_4.1](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.1.jpg)



Se descomenta el siguiente código del archivo `/var/www/html/index.html`:

![VPS_virtualFRONT_4.2](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.2.jpg)



```bash
sudo mv /var/www/html/index.html /home/master/
ls
cd ..
ls
```

![VPS_virtualFRONT_4.3](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.3.jpg)



```bash
git clone https://github.com/cavanosa/virtualFRONT.git
cd virtualFRONT
code .
```

![VPS_virtualFRONT_4.4](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.4-16451141362101.jpg)



En el terminal del Visual Studio Code escribimos lo siguiente:

```bash
npm udpdate
ng serve -o
```

Al hacerlo se mostrará lo siguiente en el navegador:

![VPS_virtualFRONT_4.5](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.5.jpg)



En el archivo `tio.service.ts`, cambiamos la variable `tioURL = 'http://localhost:8080/tio/'` a `tioURL = 'http://172.16.21.116:8080/tio/'`:

![VPS_virtualFRONT_4.6](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.6.jpg)



Ahora en el navegador ya podemos probar a crear un nuevo "tio":

![VPS_virtualFRONT_4.7](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.7.jpg)



Escribimos la siguiente línea en la terminar del Visual Studio Code para que se cree la carpeta `.dist`:

```bash
ng build --prod
```

Después, escribimos lo siguiente:

```bash
sudo rm -R /var/www/html/
sudo mkdir /var/www/html/
sudo chown -R master /var/www/html/
sudo chmod -R 755 /var/www/html/
```

Ahora, vamos al FileZilla para pasar los archivos de la carpeta `.dist` a `var/www/html`:

![VPS_virtualFRONT_4.9](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.9.jpg)



Y finalmente, en el navegador escribimos `172.16.21.116` y comprobamos que también podemos ver y gestionar los "tios":

![VPS_virtualFRONT_4.10](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.10.jpg)





> Los siguientes pasos son extras, ya que la aplicación está desplegada y funciona correctamente:

Después, escribimos lo siguiente:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
cd /etc/apach2
ls
cd sites-available
ls
sudo cp 000-default.conf 000.default.conf.bak
sudo nano 000-default.conf
```

![VPS_virtualFRONT_4.12](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.12.jpg)



En el archivo `000-default.conf` añadimos lo siguiente:

```bash
<Directory "/var/www/html">
	AllowOverride All
</Directory>
```

![VPS_virtualFRONT_4.13](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.13-16456277000091.jpg)



```bash
sudo systemctl restart apache2
cd ..
ls
sudo cp apache2.conf apache2.conf.bak
sudo nano apache2.conf
```

![VPS_virtualFRONT_4.14](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.14.jpg)



En el archivo `apache2.conf` buscamos la parte en la que se pueden ver tres `<Directory>` y modificamos las líneas `AllowOverride` de `None` a `All`:

![VPS_virtualFRONT_4.15](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.15.jpg)



A continuación, volvemos al Visual Studio Code y en la carpeta `.dist/virtualFRONT` creamos el archivo `.htaccess` y en él, escribimos lo siguiente y lo subimos con el FileZilla:

![VPS_virtualFRONT_4.16](Despliegue-de-Aplicaci%C3%B3n-Web-Full-Stack-en-VPS.assets/VPS_virtualFRONT_4.16.jpg)



Y abrimos el navegador para ver que todo funciona correctamente.

