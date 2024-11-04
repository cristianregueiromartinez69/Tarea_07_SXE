# TAREA-7-SXE

**Índice**:hushed:
- creación de carpeta y creacion del docker compose dentro de la carpeta creada
- crear estructura en docker-compose.yml de mysql
- crear estructura en docker-compose.yml de joomla
- crear estructura en docker-compose.yml de phomyadmin
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



