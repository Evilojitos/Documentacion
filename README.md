## IoT Sensors
//descripcion

### Intel Edison
#### 1.-Armar el Intel Edison
Es necesario que aprender armar correctamente el Intel Edison por lo cual se recomienda leer la documentación de Intel o visitar la página https://software.intel.com/en-us/node/628221 para aprender a ensamblar el Intel Edison. Ademas es necesario acoplar "TARJETA ARDUINO" como en la siguiente imagen									

![20181130_170958](https://user-images.githubusercontent.com/38712579/50048311-6d436480-008e-11e9-809b-287d51e0f5d8.jpg)

#### 2.- Flashear Intel Edison
Es necesario flashear el Intel Edison con la imagen que ya cuenta con las herramientas necesarias si no cuenta con una puede descargar una versión de YOCTO para esto se deberá descargar la herramienta "Intel Phone Flash Tool Lite" de la página de Intel en la pagina https://software.intel.com/en-us/get-started-edison-windows-step2.
Nota: en caso de contar con alguna imagen ya creada con los archivos y herramientas encesarias siga el paso 2 y posteriormente salte hasta el paso 14.
	
A)Despues de la instalcion del software seleccionamos la unidad donde se encuentra el intel edison
										IMAGEN DE COMO FLASHEAR LA IMAGEN
B)seleccionamos la ruta de la imagen de YOCTO o la imagen precreada con todas las errameintas necesarias
										IMAGEN CON TODOS LOS PARAMETROS PUESTOS
#### 3.-Establecer una session de consola con el Intel Edison
Para establecer una session de consola con el Intel Edison sera necesario tener instalado Putty(Windows y Ubuntu), Bitvise, etc. La configuracion para la conexion es puerto 22, velocidad 115200 se podra ver en la siguiente ademas debes de ssaber el numero de puerto de com se puede saber en administrador de dispositivos > "ALGO AQUI" y nos dara el numero de puerto que esta siendo utlizado con esta informacion estableceremos la session de consola.
   
   										IMAGEN DE PUTTY

#### 4.-Configurar la red
    Para efectuar la configuración correctamente seguir los pasos o revisar la documentación para conectarse al wifi en https://www.intel.com/content/dam/support/us/en/documents/edison/sb/edison_wifi_331438001.pdf, si se tiene duda sobre los siguientes pasos se puede consultar la siguiente pagina https://software.intel.com/en-us/connecting-your-intel-edison-board-using-wifi
    Pasos:
        1.	Establecer una sesión de consola con el Intel Edison.
        2.	Ingrese el comando configure_edison --wifi
        3.	Cuando se le pregunte si desea configurar Wi-Fi, escriba Y y presione Enter
        4.	La tarjeta integrada al Intel Edison buscará redes wifi durante 10 segundos y se mostraran la lista de redes disponibles. Si no ve ninguna red, ingrese 0 para volver a escanear.
        5.	Elija la red a que desea conectarse, escriba el número correspondiente de la lista y presione Enter. Le preguntara si quiere conectarse a esa red escriba, presione Enter
        6.	Si su red requiere contraseña u otra información, ingrese las credenciales apropiadas.
        7.	El Intel Edison intentara hacer una conexión a la red. Cuando vea un mensaje de finalización, estará conectado a internet y le proporcionara la dirección IP que tendrá el Intel Edison
        8.	Si desea ver la dirección IP ingrese el comando ifconfig
        9.  systemctl desactivar connman
        10. reiniciar(reboot)


5.- Configurando el wpa_supplicant de Edison
El Intel Edison usa el servicio wpa_supplicant para administrar la conexión Wi-Fi en modo cliente. Cuando se configure en un ambiente controlado (no en el lugar final) si se desea borrar los registros de las otras redes el archivo de configuración wpa_supplicant se encuentra en: /etc/wpa_supplicant/wpa_supplicant.conf 

    NOTA: en caso de alguna duda se puede consultar http://rwx.io/blog/2015/08/16/edison-wifi-config/ para ver las demás configuraciones del wpa_supplicant

5.- Configurando la zona horaria en Edison
    NOTA: en caso de alguna duda se puede consultar la documentación en https://communities.intel.com/thread/56294
        
        
6.-Instalar postgres
    A) Instalación
            1.- Iniciamos con una session de consola con putty y posteiormente ingrese el siguiente comando en la terminal:
                wget ftp.postgresql.org/pub/source/v9.3.2/postgresql-9.3.2.tar.bz2
            2.- Descomprimir el archivo el archivo con el siguiente comando en terminal:
                tar jxf postgresql-9.3.2.tar.bz2
            3.- Acceder a la carpeta con el siguiente comando en terminal:
                cd postgresql-9.3.2/
            4.- Se crea la carpeta con el siguiente comando en terminal:
                mkdir /opt/PostgreSQL-9
            5.-Realizamos las siguientes confiruaciones con el comando en terminal:
                ./configure --prefix = /opt/PostgreSQL-9
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

7.-Inicializar la base de datos
    Este trabajo se puede omitir ya que el demonio 'inicial' inicializa la base de datos con información real
    Nota: Para posteriores agentes
    Anadir ubicación
    Añadir sitio
	
	
8.-Instalacion de Python
    1.- Es necesario estar conectado a internet
    2.- curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
    3.- python get-pip.py
    4.- pip install paho-mqtt
    5.- pip install psycopg2
    6.- pip install configure
    
9.-Pasar los archivos escenciales para el Edison.
    1.- Crear una carpeta llamada ScriptFuncionales en la dirección raíz puede seguir los siguientes comandos:
			cd /
            mkdir ScriptFuncionales
    2.- Copiar los archivos de la carpeta ScriptsFuncionales.zip ubicados en la carpeta funcionalidades a la carpeta del Intel Edison creada en el paso 1
    
10.-Copiar los demonios
Copiar contenido de la carpeta "Demonios" en /lib/systemd/system/

11.- Cambiamos los permisos de la carpeta SystemStarting
        chmod 777 SystemStarting
        
12.- Cambiar zona horaria por Chicago 
    timedatectl set-timezone America / Chicago
	(Si no cambia: systemctl set-local-rtc 1)
    
13.-Copiar toda la carpeta logdesensores ubicada en ProbandoScripts
    Copiar toda la carpeta logdesensores en la direccion: /home/root/.cache/ y cambiar los permisos de la nueva carpeta
    
14.-Conetar los sensores al Intel Edison
    A) El sensor light debe ser conectado en AIO PIN 0
    B) El sensor loudness debe ser conectado en AIO PIN 1
    C) El sensor temperature debe ser conectado en AIO PIN 2
    D) El sensor vibration debe ser conectado en AIO PIN 3
    E) El sensor acceletometer debe ser conectado en I2C PIN 0
    F) La balanza debe ser conectada en I2C PIN UART

15.- Probar el script "SENSORDATABSEMQTT"
