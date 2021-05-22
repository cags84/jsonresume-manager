# Json Resume Manager

Este proyecto es una práctica del curso de Antonio Sarosi en Mastermind, la cual implementa un programa con Laravel 8 y VueJS.

** Nota: Para el soporte de Docker, voy a utilizar el proyecto Laravel Sail el cual esta desarrollado por los mismos de Laravel y simplifica la configuración de docker para poder trabajar con Laravel.

## PASO 1 - Crear proyecto Laravel y subirlo al repo como primer commit.

En este paso creamos un proyecto nuevo con el siguiente comando.

```sh
> composer create-project --prefer-dist laravel/laravel jsonresume-manager
> cd jsonresume-manager
```

## PASO 2 - Subimos el primer commit a github

```sh
> git init
> git add .
> git commit -m "first commit"
> git push -u origin master
```

## PASO 3 - Creamos una rama develop para trabajar con gitflow

Aquí es importante no olvidar setear github para que la rama principal donde se reciban los commits sea develop y que solo cuando se apruben los pull request se pueda hacer un merge contra la rama main.

```sh
> git checkout -b develop
> git push -u origin develop
```

## PASO 4 - Comando utiles para git

Comandos utiles que se utilizan a lo largo del desarrollo de la aplicación.

```sh
> git fetch --all
> git pull --all
> git fetch --prune
> git branch -D name_branch
```

## PASO 5 - Instalar como dependencia de desarrollo laravel sail e inicializarlo.

Para ello y siguiendo las recomendaciones de gitflow, creamos una rama nueva donde incluiremos los cambios para despues mergearlos con la rama develop.

Nota: Siempre partiendo de la rama develop, la rama master la utilizaremos para publicar las versiones que van a producción.

Para ello dentro del directorio del proyecto ejecutamos los siguientes comandos.

```sh
> git checkout -b feature-laravel-sail
> composer require laravel/sail --dev
```

Ahora inicializamos sail con el siguente comando.

```sh
> php artisan sail:install
```

Nos da para elegir la base de datos, para ello voy a seleccionar postgresql que es la opción 1:

```sh
> jsonresume-manager git:(feature-laravel-sail) ✗ php artisan sail:install

 Which services would you like to install? [mysql]:
  [0] mysql
  [1] pgsql
  [2] mariadb
  [3] redis
  [4] memcached
  [5] meilisearch
  [6] minio
  [7] mailhog
  [8] selenium
 > 1

Sail scaffolding installed successfully.
```

Ahora realizo otra modificación y es la de setear la hora a mi zona horaria, en este caso como me encuentro en Colombia, mi zona horaria debería ser "America/Bogota", para poder hacer ello hay que tener en cuenta que para el día que estoy escribiendo este documento '22/05/2021', laravel sail nos da a elegir dos opciones de configuración de PHP, las versión 8 y la 7.4.

En este caso yo voy a estar utilizando la versión 7.4, dado que es con la que habitualmente desarrollo, para ello vamos a editar el archivo `docker-composer.yml` ubicado en la raiz del proyecto para indicarle que vamos a utilizar esa versión.

```sh
> cd jsonresume-manager
> nano docker-compose.yml
```

Y editamos las lineas

```yml
--------------------------------------
Editamos:
- ./vendor/laravel/sail/runtimes/8.0
Por esto:
- ./vendor/laravel/sail/runtimes/7.4
--------------------------------------
Tambien esta:
- image: sail-8.0/app
Por:
 - image: sail-7.4/app
---------------------------------------
Tambien modifico esta:
- '${APP_PORT:-80}:80'
Por (Colocamos el puerto 84, dado que tengo ocupado el 80 con apache2):
- 84:80
```

Ahora nos queda editar el archivo que esta en la siguiente ruta.

Dentro de la carpeta del proyecto

Nota: Esta configuración al estar excluida por el .gitignore se debe hacer si se vuelve a construir la imagen o se clona el proyecto nuevamente.

NOTA: Reemplazar America/Bogota por tu zona horaria [aquí](https://www.php.net/manual/es/timezones.php)

```yml
> nano ./vendor/laravel/sail/runtimes/7.4/Dockerfile
> nano ./vendor/laravel/sail/runtimes/8.0/Dockerfile

Editar esta línea:
- ENV TZ=UTC
Por:
- ENV TZ=America/Bogota
```

Por comodidad creo un alias para evitar ejecutar todo este comando `./vendor/bin/sail` por `sail`, para ello dentro el archivo de configuración de tu terminal crea un alias, en mi caso utiliz zsh por los cual voy a editar esa.

Es importante saber donde esta el proyecto, para esto en el directorio del proyecto ejecutamos:

```sh
> cd jsonresume-manager
> pwd
----------
/home/carlos/Code/docker/jsonresume-manager
```

Ahora con esa información editar el archivo de configuración de la terminal.

```sh
> nano -w ~/.zshrc
Y agrego el siguiente contenido al final de la linea
> # alias
> alias sail='bash /home/carlos/Code/docker/jsonresume-manager/vendor/bin/sail'
```

Recargamos para que tome los cambios

```sh
> source ~/.zshrc
```

Y con ello ya tenemos el comando `sail` disponible para trabajar con el proyecto.

## PASO 6 - Inicializamos docker

Para ello ejecutamos en la terminal, en el directorio raiz el siguiente comando y esperamos a que se construyan las imagenes y levante todo para poder trabajar con ello.

NOTA: Recordar que ahora para poder ejecutar cualquier comando dentro de los contenedores lo tendremos que hacer anteponiendo el comando `sail`, por lo cual si queremos instalar un paquete con composer lo haremos de la siguiente forma, `sail composer require ....`, o si necesitamos instalar con npm lo haremos con `sail npm install ..`.

```sh
> sail up
```

Con esto tenemos listo el entorno para poder empezar a trabajar.

## PASO 7 - Subimos cambios a github

Con la rama creada y hacemos pull request para hacer merge con develop.

```sh
> git add .
> git commit -m "add laravel sail to support docker"
> git push -u origin feature-laravel-sail
```

Una vez se realiza el proceso de merge es importante eliminar la rama creada para ese caso y actualizar todo el repo.
```sh
> git checkout develop
> git fetch --all
> git pull --all
> git fetch --prune
> git branch -D feature-laravel-sail
```

# PASO 8 - Creamos el scaffolding para la autenticación con vue

Para ello dentro del la raiz del proyecto ejecutamos los siguientes comandos:

Antes creamos la rama sobre la que vamos a trabajar

```sh
> cd jsonresume-manager
> git checkout -b feature-laravel-ui
```

Instalamos y creamos el scaffolding

```sh
> sail composer require laravel/ui
> sail php artisan ui vue --auth
```

Instalamos dependencias con npm

```sh
> sail npm install
> sail npm run dev
> sail npm run watch
```

Realizamos el PASO 7 nuevamente.

# PASO 9 - Ejecutamos la migración

```sh
> sail php artisan migrate
```

Procedemos a crear un usuario y hacer login para ingresar como usuarios registrados en la aplicación.

