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

      Modo grafico 
      - PGAdmin "https://www.pgadmin.org/" administrar toda nuestra base de datos la cual tambien nos permite correr consultas de forma sql.
      - Podemos correrlo en Docker como un nuevo servicio. Configuramos dento de docker-compose.yml y no necesita establecer un volumen.

      Para levantar el servicio de pgadmin ejecutamos el comando docker-compose up -d pgadmin.Verificamos con docker-compose ps, nos vamos a localhost:5050 de forma grafica.
      Debemos decirle a que servidor queremos conectarnos Object/Register/Server.

      - docker ps -- nos muestra mas detalles de los contenedores.
      - tomamos id del contenedor de la bd y lo inspeccionamos docker inspect "id" donde tendremos toda la información del contenedor.

      -Despues de conectarnos crearemos primera tabla llamada "tasks".