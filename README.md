# CBackendNodeBDPostgreSQL
Curso de Backend con Node.js: Base de Datos con PostgreSQL

  ## Platzi Store: instalación y presentación del proyecto
      - Utilizaremos el siguiente repositorio "https://github.com/platzi/curso-nodejs-postgres" realizamos un git clone
      - Actualizamos los paquetes con npm install
      - Herramienta que usaremos es "https://insomnia.rest/" para las pruebas.
  ## Instalación de Docker
      - Según el sistema operativo que utilices puede variar la instalación, así que te doy las indicaciones base para la instalación:
      
  ### 1.- Instalación en Windows
  - Debes descargar el instalador desde la página de Docker for Windows. "https://docs.docker.com/desktop/windows/install/".
  ![](https://docs.docker.com/desktop/windows/images/hyperv-enabled.png)

  - Instalación en Windows con WSL 🐧. Cuando tienes WSL2 debes asegurarte que tienes las siguientes características habilitadas:
  ![](https://docs.docker.com/desktop/windows/images/wsl2-enable.png)

  - Y además habilitar WSL en Docker: Puedes ver más detalles en Docker Desktop WSL 2 backend "https://docs.docker.com/docker-for-windows/wsl/"

  ### 2.- Instalación en macOS 🍎
  En Mac tienes dos opciones. Todo dependerá si tienes los nuevos chips M1 o Intel, ya que hay un instalable apropiado para ambas arquitecturas de chip. Puedes escoger el instalable desde Install Docker Desktop on Mac. "https://docs.docker.com/docker-for-mac/install/".
  
  Adicionalmente si cuentas con los nuevos chips M1, debes ejecutar la siguiente instrucción en tu terminal softwareupdate --install-rosetta

  Una vez descargues el instalador adecuado, solo debes seguir los pasos y pasar Docker a tus aplicaciones.

  ### 3.- Instalación en Ubuntu 🐧
  Estos son los pasos para instalarlo dentro de Ubuntu sin embargo también puedes ver directamente Install Docker Engine on Ubuntu. "https://docs.docker.com/engine/install/ubuntu/"

  - sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
  
  - sudo mkdir -p /etc/apt/keyrings
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

  -  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

  Instalar el motor Docker 

  - sudo apt-get update
  - sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  - Recibir un error de GPG al ejecutar apt-get update "sudo chmod a+r /etc/apt/keyrings/docker.gpg".

  ## Configuración de Postgres en Docker
      Los contenedores no tienen estado. Todas las configuraciones nuestro contendor estan en docker-compose.yml
      - Lanzar posgres dentro de un contenedor en la terminal en segundo plano.
        - docker-compose up -d postgres --> "nombre servicio"
        - ver si el servicio esta en funcionamiento docker-compose ps
        - dejar de correr servicio "docker-compose down"
        - para mantener la informacion introducida debemos asignarle un volumen al contenedor
        - https://hub.docker.com/_/postgres
  ## Explorando Postgres: interfaces gráficas vs. terminal
      Terminal
      - Conectarnos db que corre dentro de un contenedor por la terminal "docker-compose exec postgres bash"
      - conectar al host y la base de datos "psql -h localhost -d my_store -U jimmy" estructura dentro de la bd comando \d+, salirnos de la db \q, salir del contenedor exit.

      "https://forums.docker.com/t/pgadmin-container-shuts-down-automatically-after-20-seconds/125730/3"
      
      Modo grafico 
      - PGAdmin "https://www.pgadmin.org/" administrar toda nuestra base de datos la cual tambien nos permite correr consultas de forma sql.
      - Podemos correrlo en Docker como un nuevo servicio. Configuramos dento de docker-compose.yml y no necesita establecer un volumen.

      Para levantar el servicio de pgadmin ejecutamos el comando docker-compose up -d pgadmin.Verificamos con docker-compose ps, nos vamos a localhost:5050 de forma grafica.
      Debemos decirle a que servidor queremos conectarnos Object/Register/Server.

      - docker ps -- nos muestra mas detalles de los contenedores.
      - tomamos id del contenedor de la bd y lo inspeccionamos docker inspect "id" donde tendremos toda la información del contenedor.

      -Despues de conectarnos crearemos primera tabla llamada "tasks".
  
  ## Integración de node-postgres
      - Dentro de esta pagina podemos ver que es una  conexion de modulos."https://node-postgres.com/" instalamos npm install pg
      - Creamos nuestra carpeta de librerias "libs" que se encargan la conexion a terceros sean APIS o Base de Datos.
      - Para solucionar error creer un volumen para el pgadmin 
      - Tambien debemos iniciar el proyecto npm run start para comprobarlo en insomnia.
  
  ## Manejando un Pool de conexiones
      - Request matener una sola conexion "https://node-postgres.com/features/pooling"
      - Creamos un archivo postgres.pool.js lo usaremos en el servicio de prodcutos.

  ## Variables de ambiente en Node.js
      - Dentro de nuestro archivo config.js tendremos configuracion base de las variables de entorno.
      - Dentro de .env tiene los accesos localmente forma seguro de trabajar el cual no va al repositorio. "npm install dotenv" usamos dentro del archivo /config/config.js
      - Pero dentro del archivo ".env.expample" el cual si va al repo decimos cual es información que deben tener para poder acceder al proyecto.

  ## ¿Qué es un ORM? Instalación y configuración de Sequelize ORM
      Un ORM es un modelo de programación que permite mapear las estructuras de una base de datos relacionales. Usaremps el ORM de Sequelize.
	  - Instalamos el ORM "npm install --save sequelize"
	  - Drivers para PostgreSQL "npm install --save pg pg-hstore"

  ## Tu primer modelo en Sequelize
	  - Vamos a ver el esquema que va tener la base de datos. Esquema de tipos en la base de datos.

  ## Crear, actualizar y eliminar
      - De datos dentro de la tabla de los usuarios.
        
  ## Cambiando la base de datos a MySQL
      - Cambiamos la base de datos a MySQL. Donde crearemos un contenedor donde correra MySQL.
      - Creamos nuestro volumen para datos de myslq y lo ignoramos.
      - De forma grafica utilizaremos phpmyadmin.
      - Levantamo nuestros servicios docker-compose up -d mysql y docker-compose up -d phpmyadmin.
      - Para conectarnos con mysql debemos instalar el driver de mysql "npm install --save mysql2" cambios el puerto y el usuario en ".env" y en libs/sequilize.js el motor postgres por mysql.
      -ejecutamos nuestro project con npm run start o dev.
      - revisamos en insomnia y localhost:8080/phpmyadmin.

  ## ¿Qué son las migraciones? Migraciones en Sequelize ORM
      - Las migraciones son una forma de crear una base de datos con un esquema especifico. Las migraciones de base de datos comunes en los frameworks de programación como MySQL, SQLite, PostgreSQL, Oracle, etc.
      - Correr migraciones instalamos el ORM "npm install --save-dev sequelize-cli" como dependencia de desarrolo y un archivo de ".sequelizerc".

  ## Configurando y corriendo migraciones con npm scripts
      - No crea las migraciones de manera automatica al hacer alguna modificacion "no genera migraciones".
      - Configuración para que las migraciones detecten un cambio y las corra de manera automatica.
      - Scripts para correr migraciones dentro de nuestro package.json.
      - Nos vamos a la terminal y la ejecutamos npm run migrations:generate "--name" ejemplo npm run migrations:generate create-user el cual se almacena dentro de la carpeta de migrations donde tendremos que definir manualmente que migraciones queremos hacer.
      - Borramos nuestras tablas de pgadmin y probamos nuestras migraciones y ejecutamos npm run migrations:run si ocurre un error cambiar los datos dentro de ".env".

  ## Modificando una entidad
      - Con migraciones alterar una tabla que ya esta creada en la base de datos. Agrear modificaiones a una tabla.
      - Generamos una nueva migracion "npm run migrations:generate add-role"
      - Ejecutamos nuestra migración "npm run migrations:run"
  
  ## Relaciones uno a uno
      - Una tabla puede tener una relacion con otra tabla.
      - Usaremos customer para conectarlo con un usuario.
      - Comandos debemos usar son "npm run migrations:generate create-customers" para la migracion de nuestra tabla, "npm run migrations:run" para correr las migraciones

  ## Resolviendo relacion unica dentro de la tabla customer a user
      - No deberia pasar que dos clientes esten relaciones relacionados al mismo usuario.
      - Decimos que el userId sea unico lo cual generamos una nueva migracion con los cambios de la tabla "npm run migrations:generate change-user-id" el cual modificaremos la columna.
      - Corremos la migracion. "npm run migrations:run"

  ## Resolviendo las relaciones uno a uno con sus asociaciones
      - Resolver de forma anidada esta relacion, el alias muy importante para resolver la asociacion. Sabemos hacer relaciones uno a uno de forma bidireccional.

  ## Relaciones uno a muchos 1-N "hasMAny"
      - Generamos la migracion de las tablas de products y category "npm run migrations:generate  products" y arracamos nuestro migracion "npm run migrations:run".