Vulnerando la maquina FRIENDLY

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/a96f3793-1362-45dd-b184-eaf450418579)

Level: Easy

Platform: Linux

Primero identificamos la IP de la maquina.
Con el siguiente comando realizamos el escaneo especificando nuestra tarjeta de red: netdiscover -i eth0 

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/4bcbd7b8-0c9b-4cd4-bb7b-c664ff42a62f)

La IP objetivo es 192.168.1.104.

Luego realizamos un escaneo sobre esta IP 192.168.1.104.
El siguiente comando nos permitira identificar puertos abiertos, los servicios que se ejecutan y algunas vulnerabilidades detectadas: nmap -sV -sC 192.168.1.104.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/cd445bba-caa0-42a5-abb5-9545c4b8b3da)

Se identificaron dos puertos abiertos por los cuales se estan ejecutando dos servicios.

Puerto 21 - FTP, el escaneo nos muestra que tiene una vulnerabilidad la cual nos permite ingresar por ftp con el usuario anonymous sin la necesidad de contar con una contraseña.

Puerto 80 - HTTP, se esta ejecutando una aplicacion apache, no nos muestra mas detalles.

Al ingresar por el navegador solo nos muestra la pagina por defecto de Apache.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/c89004aa-a881-4d4e-a0d9-a494ffd5af63)

Ingresamos por ftp y el unico archivo que se observa es el de index.html

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/ad6ff932-840c-407f-a80f-9282c32d746a)

Dado el escenario, podriamos aprovechar esta vulnerabilidad para subir un archivo que nos permita obtener una reverse shell.
El archivo se llama shell.php, este archivo basicamente al ejecutarse buscara conectarse a la maquina atacante a traves del puerto 4444.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/d807dc7c-aedb-462c-90a7-62c929be94a3)

El contenido se sube con el comando put.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/7414fbbd-0c86-4dfe-b1d2-7f4f8c67b207)

En la maquina atacante preparamos el listener con nc para que acepte la conexion una vez se ejecute el archivo.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/9ca2adef-f9ef-4883-a84b-061b2d11c725)

Luego vamos al navegador y agregamos en la ruta /shell.php, esto ejecutara el archivo en la aplicacion y se establecera la conexion.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/912a018b-f955-4ab7-8bf9-87a2da7f1694)

Una vez dentro yo agrego estos dos comandos para obtener una shell interactiva y poder usar el comando 'clear'.

python3 -c 'import pty; pty.spawn("/bin/bash")'

export TERM=screen-256color

Navegamos un poco y obtenemos el flag del user, localizado en la carpeta del usuario RiJaba1.
b8cff8c9008e1c98a1f2937b4475acd6

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/70eaa36b-fbfa-4600-bd0a-950be2a3b0af)

Sigue obtener el flag del root, para ello se necesita escalar en privilegios ya que nuestra usuario es limitado.
El metodo que usaremos sera 'Abusing sudo binaries'.
Ingresamos el comando sudo -l, este comando nos permite listar que programas podemos ejecutar con privilegio de superusuario.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/b11842c2-65d6-4a24-bbc3-1ffaed78edb7)

En este caso, el usuario www-data tiene permiso para ejecutar el comando "/usr/bin/vim" en cualquier host sin necesidad de ingresar una contraseña.
Aprovechamos esta vulnerabilidad y ejecutamos sudo vim -c ':!/bin/bash', lo que hará este comando es iniciar un shell de Bash con privilegios de superusuario dentro de Vim.
Al ejecutarlo automaticamente escalamos a root.
Buscamos dentro del directorio root el archivo root.txt, al abrirlo nos indica que busquemos el archivo root.txt.
Realizamos una busqueda desde la raiz y notamos la existencia de otro archivo root.txt en otra ruta. Al abrirlo obtenemos el flag del root.

![image](https://github.com/ByteKaus/HackMyVM-pwned/assets/79665766/01fad30e-175d-4dcf-a233-abf070de877e)









