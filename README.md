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