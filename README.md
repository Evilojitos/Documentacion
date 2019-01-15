## IoT Sensors
This repository contains a system for monitoring diverse conditions of Bees using microsensors and a sensors network

### Intel Edison
#### 1.- Armar el Intel Edison
Es necesario que aprender armar correctamente el Intel Edison por lo cual se recomienda leer la documentación de Intel o visitar la página https://software.intel.com/en-us/node/628221 para aprender a ensamblar el Intel Edison. Además, es necesario acoplar "MODULO ARDUINO BASE SHIELD" como en la siguiente imagen.

#### 2.- Flashear Intel Edison
Es necesario flashear el Intel Edison con la imagen que ya cuenta con las herramientas necesarias si no cuenta con una puede descargar una versión de YOCTO para esto se deberá descargar la herramienta "Intel Phone Flash Tool Lite" de la página de Intel en la pagina https://software.intel.com/en-us/get-started-edison-windows-step2.
Nota: en caso de contar con alguna imagen ya creada con los archivos y herramientas necesarias siga el paso 2 y posteriormente salte hasta el paso 14.
	
A) Después de la instalación del software seleccionamos la unidad donde se encuentra el Intel Edison
B) Seleccionamos la ruta de la imagen de YOCTO o la imagen pre-creada con todas las herramientas necesarias

![herramientaparaflashear](https://user-images.githubusercontent.com/38712579/51132780-c969e780-17f8-11e9-8259-c3e9ac76cab6.png)

										
#### 3.- Establecer una sesión de consola con el Intel Edison
Para establecer una sesión de consola con el Intel Edison será necesario tener instalado Putty( Windows y Ubuntu), Bitvise, etc. La configuración para la conexión es puerto 22, velocidad 115200 se podrá ver en la siguiente además debes de saber el número de puerto de COM se puede saber en administrador de dispositivos > Puertos (COM y LPT) y nos dará el número de puerto que está siendo utilizado con esta información estableceremos la sesión de consola.
   
![puttyedison](https://user-images.githubusercontent.com/38712579/51132562-22854b80-17f8-11e9-8e24-75441113cf3d.png)


#### 4.- Configurar la red
Para efectuar la configuración correctamente seguir los pasos o revisar la documentación para conectarse al wifi en https://www.intel.com/content/dam/support/us/en/documents/edison/sb/edison_wifi_331438001.pdf, si se tiene duda sobre los siguientes pasos se puede consultar la siguiente pagina https://software.intel.com/en-us/connecting-your-intel-edison-board-using-wifi
Pasos:
    1.-	Establecer una sesión de consola con el Intel Edison.
    2.-	Ingrese el comando configure_edison --wifi
    3.-	Cuando se le pregunte si desea configurar Wi-Fi, escriba Y y presione Enter
    4.-	La tarjeta integrada al Intel Edison buscará redes wifi durante 10 segundos y se mostraran la lista de redes disponibles. Si no ve ninguna red, ingrese 0 para volver a escanear.
    5.-	Elija la red a que desea conectarse, escriba el número correspondiente de la lista y presione Enter. Le preguntara si quiere conectarse a esa red escriba, presione Enter
    6.-	Si su red requiere contraseña u otra información, ingrese las credenciales apropiadas.
    7.-	El Intel Edison intentara hacer una conexión a la red. Cuando vea un mensaje de finalización, estará conectado a internet y le proporcionara la dirección IP que tendrá el Intel Edison
    8.-	Si desea ver la dirección IP ingrese el comando ifconfig
    9.- systemctl desactivar connman
    10.- Reiniciar(reboot)


#### 5.- Configurando el wpa_supplicant de Edison
El Intel Edison usa el servicio wpa_supplicant para administrar la conexión Wi-Fi en modo cliente. Cuando se configure en un ambiente controlado (no en el lugar final) si se desea borrar los registros de las otras redes el archivo de configuración wpa_supplicant se encuentra en: /etc/wpa_supplicant/wpa_supplicant.conf 

NOTA: en caso de alguna duda se puede consultar http://rwx.io/blog/2015/08/16/edison-wifi-config/ para ver las demás configuraciones del wpa_supplicant

#### 6.- Configurando la zona horaria en Edison
NOTA: en caso de alguna duda se puede consultar la documentación en https://communities.intel.com/thread/56294
        
        
#### 7.- Instalar postgres
A) Instalación
    1.- Iniciamos con una sesión de consola con Putty y posteriormente ingrese el siguiente comando en la terminal:
        wget ftp.postgresql.org/pub/source/v9.3.2/postgresql-9.3.2.tar.bz2
    2.- Descomprimir el archivo el archivo con el siguiente comando en terminal:
        tar jxf postgresql-9.3.2.tar.bz2
    3.- Acceder a la carpeta con el siguiente comando en terminal:
        cd postgresql-9.3.2/
    4.- Se crea la carpeta con el siguiente comando en terminal:
        mkdir /opt/PostgreSQL-9
    5.-Realizamos las siguientes confiruaciones con el comando en terminal:
        ./configure --prefix = /opt/Post    
		Nota: Se debe de tener primero creada la carpeta
    6.- Hacer make
        make (si es error make clean)
    7.- Hacer instalar
B) Configuración
    1.- Agregar un usuario con el siguiente comando en terminal:
        useradd postgres
    2.- Cambiar los permisos recursivamente, haga que postgres sea propietario de la carpeta de instalación de postgres con el siguiente comando en terminal:
        chown -R postgres: postgres/opt/PostgreSQL-9/
        cd /opt/PostgreSQL-9/bin/
        initdb -D /folder/for/ dbs
        ./initdb -D ../share/dbs/
    3.- Copiar base de datos en bin
    4.- Inicializamos postgres y posteriormente crearemos la base de datos donde volcaremos los datos con los siguientes comandos:
        systemctl start postgres
		./postgres -D ../share/dbs
        ./createdb iot-sensores
    5.- Restauraremos la base de datos con el siguiente comando:
        ./psql iot-sensores <DB_a_llenar.sql 
    6.- Copiar el contenido de la carpeta demonio en la siguiente ruta dentro del Intel Edison /lib/systemd/system/
    7.- Iniciamos el servicio de postgres
        systemctl start postgres
    8.- Verificamos el estado de postgres que este corriendo
        systemctl status postgres

#### 8.- Inicializar la base de datos
Este trabajo se puede omitir ya que el demonio 'inicial' inicializa la base de datos con información real
Nota: Para posteriores agentes
Anadir ubicación
Añadir sitio
	
	
#### 9.- Instalacion de Python
Pasos:
    1.- Es necesario estar conectado a internet
    2.- curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
    3.- python get-pip.py
    4.- pip install paho-mqtt
    5.- pip install psycopg2
    6.- pip install configure
    
#### 10.- Pasar los archivos esenciales para el Edison.
Si empleara la balanza Alemana Emplear la carpeta ScriptFuncionales - Alemana o ScriptFuncionales
1.- Crear una carpeta llamada ScriptFuncionales en la dirección raíz puede seguir los siguientes comandos:
    cd /
    mkdir ScriptFuncionales
2.- Copiar los archivos de la carpeta ScriptsFuncionales.zip ubicados en la carpeta Intel Edison > funcionalidad a la carpeta del Intel Edison creada en el paso 1.
    
#### 11.- Copiar los demonios
Copiar contenido de la carpeta "Demonios" en /lib/systemd/system/

#### 12.- Cambiamos los permisos de la carpeta SystemStarting
Con el siguiente comando se podrá cambiar los permisos del archivo:
    chmod 777 SystemStarting
        
#### 13.- Cambiar zona horaria por Chicago
Con los siguientes comandos se podrá cambiar la zona horaria:
timedatectl set-timezone América / Chicago
Nota: Si no cambia puede usar: systemctl set-local-rtc 1
    
#### 14.- Copiar toda la carpeta logdesensores ubicada en ProbandoScripts
Copiar toda la carpeta logdesensores en la direccion: /home/root/.cache/ y cambiar los permisos de la nueva carpeta
    
    
#### 15.- Conetar los sensores al Intel Edison
    A) El sensor light debe ser conectado en AIO PIN 0
    B) El sensor loudness debe ser conectado en AIO PIN 1
    C) El sensor temperature debe ser conectado en AIO PIN 2
    D) El sensor vibration debe ser conectado en AIO PIN 3
    E) El sensor acceletometer debe ser conectado en I2C PIN 0
    F) La balanza debe ser conectada en I2C PIN UART

![conexion_sensores_puertos_edison](https://user-images.githubusercontent.com/38712579/51132266-6c216680-17f7-11e9-9e2d-28f8f074f9ac.png)


### Raspberry Pi 3
#### 1.- Imagen del sistema
Deberá de tener una memoria microSD para montar la imagen del sistema Raspbian podrá descargarla de https://www.raspberrypi.org/downloads/
NOTA: SI SE TIENE UNA MICRO SD CON LA IMAGEN PRE CREADA REALIZE LOS PASOS 2 Y SALTE AL PASO 7

#### 2.- Armado del Raspberry Pi 3
Se deberá de conectar teclado, mouse, cable HDMI e insertar la memoria microSD en el Raspberry.

#### 3.- Configurar el Raspberry
-encender la Raspberry pi (conecte el cable de alimentación al Raspberry para encenderlo)
1.-Una vez iniciando el sistema deberá hacer clic en la frambuesa > Preferencias > Configuración de Raspberry Pi.
en la pestaña de interfaces activar cámara, shh, spi, I2C, serial port, 1-Wire, Remote GPIO.
2.-Posteriormente cambiar a la pestaña de localización y configurar
1.- local
    2.- Zona horaria
    3.- teclado
    4.- Dar aceptar y pedirá reiniciar el Raspberry y lo reinicia
3.-Se deberá de actualizar el Raspberry abra una consola de comando e ingrese el siguiente comando:
	sudo apt-get update
posteriormente ingrese el siguiente comando:
	sudo apt-get upgrade
4.-Reinicia el Raspberry.

#### 4.- Instalacion del proyecto de Git
Para el uso de la tarjeta GrovePi+ es necesario clonar el siguiente proyecto se podrá realizar con el siguiente comando en consola:
 	git clone https://github.com/DexterInd/GrovePi
 Posteriormente deberemos de instalarlo con el siguiente comando:
 	sudo bash /home/pi/Dexter/GrovePi/Script/install_scratch.sh
 Para realizar la última actualización del firmware por lo cual nos posicionaremos en la carpeta llamada "Firmware" dentro del proyecto y posteriormente cambiaremos los permisos de un script para realizar la actualización:
 	sudo chmod +x firmware_update.sh
 A continuación, ejecutaremos el archivo:
 	sudo ./firmware_update.sh

#### 5.- Instalando PostgreSQL
Se instalará PostgreSQL ingresando en la consola de comandos:
	sudo apt install postgresql libpq-dev postgresql-client postgresql-client-common -y
Accedemos a postgres ingresando:
	sudo su postgres
Crearemos un usuario
    createuser pi -P --interactive
Cuando se le solicite, ingrese una contraseña (y recuerde cuál es), seleccione n para superusuario e y para las siguientes dos preguntas
Nos conectamos a postgres usando su shell
    psql
Creamos la base de datos iot_sensors
	create database iot_sensors

Importamos la base de datos que tenemos en el archivo sql llamado "NOMBRE DEL ARCHIVO SQL BASE DE DATOS"

posteriormente el archivo sql debe de estar en el escritorio y desde la terminal nos colocamos en esa misma ruta e ingresamos 
	psql -U postgres -d iot_sensors -f {dumpfilename.sql}

NOTA: para mayor ayuda puede consultarl el siguiente post https://opensource.com/article/17/10/set-postgres-database-your-raspberry-pi
	
#### 6.- Instalar librerías necesarias
Es necesario instalar diversas librerías puede insertar las siguientes
	sudo pip install psycopg2
	sudo pip install configparser
	sudo pip install paho-mqtt
	apt install python-usb

Nota: si se tiene algún error sobre que no se encontró las librerías etc. la solución es volver actualizar el Raspberry con
	sudo apt-get update 
	sudo apt-get upgrade

#### 7.- Configurar el puerto UART
Para el uso de la balanza prototipo 2 es necesario configurar el puerto UART deberá de seguir los siguientes pasos:
1.-Ingrese en la consola el siguiente comando:
    sudo raspi-config
2.-Seleccione la opción que contenga "Interfacing Options Configure coonections to peripherals"
3.-Seleccione la opción que contenga "Serial Enable/Disable shell and kernel messages on the serial connection"
4.-Seleccione la opción "No"
5.-Seleccione la opción "Yes/Si"
6.-Cuando pregunte si desea reiniciar le damos en "Yes/Si"

#### 8.- Pasar los archivos esenciales para el Edison.
Si empleara la balanza Alemana Emplear la carpeta ScriptFuncionales - Alemana o ScriptFuncionales
1.- Crear una carpeta llamada ScriptFuncionales en la dirección raíz puede seguir los siguientes comandos:
    cd /
    sudo mkdir ScriptFuncionales
2.- Copiar los archivos de la carpeta ScriptsFuncionales.zip ubicados en la carpeta Raspberry > funcionalidad a la carpeta del Raspberry creada en el paso 1.
NOTA: PARA CREAR CARPETAS O MOVER LOS ARCHIVOS SERA ENCESARIO QUE AGREGA LOS COMANDOS "sudo" PARA EMPLEAR EL USO DE ROOT.

#### 9.- Copiar los demonios
Copiar contenido de la carpeta "Demonios" en /lib/systemd/system/

#### 10.- Cambiamos los permisos de la carpeta SystemStarting
Con el siguiente comando se podrá cambiar los permisos del archivo:
    chmod 777 SystemStarting

#### 11.- Conectar los sensores al Raspberry
    A) El sensor loudness debe ser conectado en el puerto A1
    B) El sensor temperature debe ser conectado en el puerto A2
    C) El sensor vibración debe ser conectado en el puerto A0
    D) El sensor acceletometer debe ser conectado en I2C PIN 0
    E) La balanza debe ser conectada en I2C PIN UART
    F) El sensor de temperatura y humedad debe ser conectado en el puerto D4
