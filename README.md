# ProyectoIV, Bot de Telegram QueToca
[![Build Status](https://travis-ci.org/Anixo/ProyectoIV.svg?branch=master)](https://travis-ci.org/Anixo/ProyectoIV)
[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/Anixo/ProyectoIV)
>Proyecto a desarrollar de la asignatura Infraestructura Virtual.  
Se puede consultar en [este enlace](https://anixo.github.io/ProyectoIV/).

# Descripción
Se va a desarrollar un bot de Telegram que al preguntarle nos dira el horario que nos corresponde.  
En un principio solo funcionará para alumnos del **grado de Ingeniería Informática**  
También nos dira el profesor correspondiente a la asignatura.  
Deberemos saber el curso y el grupo del usuario.  
Nos dirá también la asignatura actual en la que se encuentra el alumno.  

# Desarrollo
Hemos usado lenguaje Python y la API de Telegram. Se tendrá en cuenta para el desarrollo de este bot la API de un calendario realizada por un compañero (SCRUM).  
Usaremos un sistemas de test y de integración continua, que se detalla más adelante.   
Como todavía no disponemos de una BBDD, usamos el archivo *horario.json* para realizar los tests.  

# Sistema de test e Integración continua
Hemos usado el sistema de test de Python, `unittest`, y TravisCI para la integración continua, encargado de ejecutar estos test. Cuando se modifique el código del repositorio, se comprueba que el codigo generado sigue funcionando de manera correcta, verificando su integración.  
El archivo `QueToca_test.py`, dentro de la carpeta *botQueToca*, tiene desarrollados los test unitarios de las funciones de la biblioteca del bot.

# Requisitos:
Debemos instalar:  
~~~
$ sudo apt-get install python  
$ sudo apt-get install python-pip  
$ sudo pip install -r requirements.txt
~~~

# Desarrollo del PaaS
Hemos desplegado el PaaS en Heroku por tener bastantes servicios básicos y de manera gratuita, además de ofrecer una gran cantidad de herramientas.  

Para poder desplegar la aplicación debemos instalar Heroku y ejecutar el siguiente comando para estar logueados:  
~~~
$ heroku login
~~~

Una vez logueados, ejecutamos el siguiente comando para crear la aplicación en Heroku:  
~~~
$ heroku create
~~~

Nuestra aplicacion de Heroku consta de dos dynos, una para el bot y otra para un servicio web, así que debemos configurar de forma correcta el archivo `Procfile` para que tenga en cuenta estas dos aplicaciones.  

En Heroku hemos añadido la base de datos PostgreSQL, para que el bot tenga una funcionalidad mínima que devolver. Esto se puede hacer desde el dashboard de Heroku en el navegador.  

Se han definido también variables de entorno para el TOKEN del Bot y para las datos de conectividad de la base de datos creada.

Ahora lanzamos los dos dynos especificados en `Procfile` con el comando:  
~~~
$ heroku ps:scale worker=1 web=1
~~~

Se ha configurado Heroku para que cada vez que se realice un push en el repositorio, Heroku lo tiene en cuenta y se despliega automáticamente despúes de pasar los test de integración continua:
![imagen](https://github.com/Anixo/ProyectoIV/blob/master/img/heroku.png)  

Podemos probar que el servicio web funciona ejecutando  
~~~
$ heroku open
~~~
y viendo que devuelve `status: OK`  
También se puede visitar las rutas siguientes:
* https://iv-anixo.herokuapp.com/horario
* https://iv-anixo.herokuapp.com/unhorario/4  

La funcionalidad del bot se puede hacer diciéndole el comando `/horario`. El bot se llama @QueTocaBot.

Despliegue https://iv-anixo.herokuapp.com/
