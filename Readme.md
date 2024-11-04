# TAREA-7-SXE

**Índice**:hushed:
- creación de carpeta y creacion del docker compose dentro de la carpeta creada
- crear estructura en docker-compose.yml de mysql
- crear estructura en docker-compose.yml de joomla
- crear estructura en docker-compose.yml de phpmyadmin
- lanzar docker compose y comprobar joomla y mysql
- comprobar phpmyadmin

### 1. creación de carpeta y creacion del docker compose dentro de la carpeta creada 😄
Nos metemos donde tenemos el entorno de tranajo de docker e introducimos:
´´´bash
mkdir docker_compose_joomla
´´´
Luego hacemos:
```bash
#nos metemos en el directorio creado
cd docker_compose_joomla
#nos preparamos para escribir el archivo
nano docker-compose.yml
```
se nos abrirá el nano vacío y nos preparamos para editarlo :heart_eyes:

### 2. crear estructura en docker-compose.yml de mysql :hushed:
Ya estamos en el nano, así que escribimos esto en el docker-compose.yml
```bash
services:
  db:
    image: mysql:9.1.0
    restart: unless-stopped
    volumes:
      - db_data_joomla:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456789
      MYSQL_DATABASE: joomladb
      MYSQL_USER: joomlauser
      MYSQL_PASSWORD: 123456789
```
### EXPLICACIÓN: :grinning:
```bash
#services:
Es como si fuera una lista de todos los servicios que queremos levantar con el docker compose
```

```bash
#db:
el nombre que le vamos a dar al contenedor del servicio de mysql, en este caso es db
```

```bash
#image: mysql:9.1.0
se descargará la imagen de mysql especificada con el tag
```

```bash
#restart: unless-stopped
indicamos que el servicio solo se apaga si lo paramos nosotros
```

```bash
 #volumes:
      #- db_data_joomla:/var/lib/mysql
Esto es un montaje del sistemas de ficheros en donde se guardan las base de datos de mysql, es decir, si el contenedor se corrompe, se pierde o lo que sea, en este sistema de ficheros db_data_joomla:, se guardarán las bases de datos
```

```bash
# environment:
variables de entorno del servicio, tales como la contraseña root, usuario, password...
```

### 3. crear estructura en docker-compose.yml de joomla :hushed:
Seguimos en el docker-compose.yml y escribimos esto:
```bash
services:
  db:
    image: mysql:9.1.0
    restart: unless-stopped
    volumes:
      - db_data_joomla:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456789
      MYSQL_DATABASE: joomladb
      MYSQL_USER: joomlauser
      MYSQL_PASSWORD: 123456789

  joomla:
    depends_on:
      - db
    image: joomla:latest
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - joomla_data:/var/www/html
    environment:
      JOOMLA_DB_HOST: db
      JOOMLA_DB_USER: joomlauser
      JOOMLA_DB_PASSWORD: 123456789
      JOOMLA_DB_NAME: joomladb
```

## EXPLICACIÓN :grinning:
```bash
#depends_on:
      #- db
Con esto indicamos que el servicio no se puede iniciar hasta que la base de datos empiece a funcionar
```

```bash
#image: joomla:latest
Nos descargamos la imagen más reciente de joomla
```

```bash
#restart: unless-stopped
Indicamos que no se va a parar el servicio a no ser que lo paremos nosotros
```

```bash
#ports:
      #- "8080:80"
Mapeamos los puertos, es decir, a través del puerto del contenedor creamos una conexión hacia el puerto 8080 para visualizar lo que hay
```

```bash
# volumes:
    #  - joomla_data:/var/www/html
montamos un sistema de ficheros en la ruta especificada
```

```bash
#environment:
Variables de entorno del servicio, tales como nombre del servidor, usuario...
```

### 4. crear estructura en docker-compose.yml de phpmyadmin :hushed:
Seguimos en el docker-compose.yml e introducimos esto:

```bash
phmmyadmin:
    image: phpmyadmin:5.2.1-apache
    restart: unless-stopped
    depends_on:
      - db
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: 123456789


volumes:
  db_data_joomla:
  joomla_data:
```

### EXPLICACION :grinning:

```bash
#image: phpmyadmin:5.2.1-apache
descargamos la imagen indicada de php
```

```bash
#restart: unless-stopped
No paramos el servicio a menos que lo hagamos nosotros
```

```bash
# depends_on:
      #- db
El servicio de phpmyadmin no empezará hasta que se cree el servidor de base de datos mysql ya que depende de un servidor de base de datos para administrarla
```

```bash
#ports:
      #- "8081:80"
Lo mismo que con joomla, mapeamos los puertos para poder acceder al servidor de dentro del contenedor
```

```bash
#environment:
Variables de entorno de la aplicacion
```

```bash
# volumes:
  #db_data_joomla:
  #joomla_data:
Estas son las variables globales de sistemas de ficheros que van a usar joomla y mysql, se declaran fuera de la lista de servicios para que sea accesible para todos ellos
```

### 5. lanzar docker compose y comprobar joomla y mysql :blush:
Nos metemos en la carpeta donde hemos hecho el docker compose y ejecutamos

```bash
sudo docker compose up -d
```
Esperamos un poco y debería de salir esto
![servicioMaquinajoomlamysql](https://github.com/user-attachments/assets/59d47c76-8f48-4bab-bfd1-e150ccf5ac24)

**Nos vamos a un navegador**

```bash
#introducimos en las url
http://(tu_ip):8080
```
Cambias el (tu_ip) por la ip de la maquina virtual en caso de hacer esto en una máquina virtual o si no es máquina virtual pues la ip de tu ordenador. Debería de salir esto
```bash
#introducimos el nombre del joomla y avanzamos
```
![joomla1](https://github.com/user-attachments/assets/9a6fb9e2-d190-4657-b875-d5b93e402ab9)

```bash
#rellenamos los campos y avanzamos
```
![joomla2](https://github.com/user-attachments/assets/e7b1fe89-87e5-42d6-8c07-4642211017d1)

```bash
#rellenamos configuración de base de datos
```
![joomla3](https://github.com/user-attachments/assets/8fa8532e-56b2-4cd4-bb09-726bc9059436)

### ATENCION :scream:
- El nombre del servidor (hospedaje, como pone en la web) debe de coincidir con el nombre que le dimos a mysql en el docker-compose.yml, en este caso db

```bash
#si todo está bien, saldrá esto
```
![joomla4](https://github.com/user-attachments/assets/e8943d35-196d-48e8-ba84-b950224b95c4)


```bash
# rellenamos usuario y contraseña para iniciar sesión
```
![joomla5](https://github.com/user-attachments/assets/e3996706-07d3-4f07-9617-968dcc898b3f)

```bash
#si todo está bien, saldrá esto
```
![joomla6](https://github.com/user-attachments/assets/ef283cbe-6a26-4014-8674-5fc250bf2a24)

**FELICIDADES, HAS INSTALADO JOOMLA Y MYSQL, AHORA PODRÁS JUGAR Y HACER LO QUE QUIERAS CON ELLOS :clap:





