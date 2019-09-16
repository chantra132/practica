# **PARTE 1**

## Punto 1. Generar nuevo Virtual-host
Para generar un nuevo virtual-host se ha de acceder al archivo **backend.pp** alojado en la ruta **C:\Users\Carmen\backend\puppet\manifests.**

Se añade el siguiente código en la **línea 98**
```
nginx::resource::server{"practica.aplicaciones.web":
  use_default_location => false,
  listen_port => 80,
  www_root  => "/var/www/practica.aplicaciones.web/public",
  index_files => [ 'index.html', 'index.php' ],
}

# Qué hará nginx por defecto con este "server"
nginx::resource::location{ 'practica.aplicaciones.web/':
  location    => "/",
  server      => "practica.aplicaciones.web",
  # Esta configuración pertenece al dominio que digamos
  ensure         => present,
  # Esta configuración debe quedar escrita siempre
  # en algún fichero de configuración de nginx 
  # por lo tanto puppet creará un archivo en /etc/nginx/conf.d/
   autoindex   => 'off',
  # No se listará el contenido de la carpeta
  location_cfg_append =>{
    try_files => '$uri /index.php$is_args$args' 
  },
  index_files => ['index.php']
}


nginx::resource::location { 'practica.aplicaciones.web/index.php':
  location       => "~ ^/index\.php(/|$)",
  server         => "practica.aplicaciones.web",
  ensure         => present,
  fastcgi        => "${url_servidor_aplicaciones}",
  # location of fastcgi 
  fastcgi_split_path => '^(.+\.php)(/.*)$',

  # Info para el servidor de aplicaciones
  #location_cfg_append =>{
  # Info para el servidor de aplicaciones
  # fastcgi_split_path_info => '^(.+\.php)(/.*)$'
  #},
  fastcgi_param  => {
    'APP_ENV'          => 'dev',
    'SCRIPT_FILENAME'  => '$realpath_root$fastcgi_script_name',
    'DOCUMENT_ROOT'    => '$realpath_root'
  }
}
``` 
- Mediante Git Bash se accede a la carpeta backend donde disponemos del servidor virtual. 
    ``cd backend``
- En caso de tener el servidor parado, se levanta servidor realizando la siguiente ejecución:
    ``vagrant up`` 
- En caso de tenerlo ya arrancado, pasa directamente al siguiente punto. 
- Una vez arrancado el servidor y posicionado en la carpeta se ejecuta:
    ``vagrant provision--provision-with=puppet``

## Punto 2 Añadir el dominio en el archivo host
- Se abre aplicación notepad como administrador (botón derecho encima del icono - abrir como administrador)
- Se abre Archivo - abrir y se busca la carpeta c:\windows\system32\driver\etc
- En la parte baja a la derecha de la ventana donde estamos, tenemos que seleccionar **todos los archivos (*.*).
- Entonces nos aparecerá un listado de archivos para seleccionar. Seleccionamos **hosts** y le damos a abrir. 4
- Una vez abierto, en la última linea añadimos:
  ``192.168.33.10 practica.aplicaciones.web``
- Cerramos y guardamos cambio. 

## Punto 3 Despliegue de la aplicación. 
- Se accede al enlace http://github.com/temple/Aplicaciones_web
- A la derecha nos encontramos un botón verde donde indica **clone or download**
- Se copia el enlace que aparece al desplegarse. 
- Nos vamos a Git Bash 
 
## Punto 4 Comprobación de carga de la aplicación.  
- Desde la carpeta \backend ejecutamos:
``vagrant ssh``
- Una vez accedemos por ssh a nuestro servidor accedemos a la carpeta **/var/www/practica.aplicaciones.web** y ejecutamos:
``composer dump-autoload`` para que el archivo composer que dispone la aplicación se ejecute. 
- Desde un explorador de internet se verifica que añadiendo el dominio carga la aplicación. 


# PARTE 2

## Punto 1: RAMA Musa-front

-  Nos vamos a Git Bash y accedemos a la carpeta del dominio ``/backend/www/practica.aplicaciones.web``
-  Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Musa-front```
-  Una vez en la rama de Musa-front, nos colocamos en el navegador de internet y escribimos el dominio ``practica.aplicaciones.web``
-  Verificamos que no carga nada en pantalla.
-  Se revisa index.php y se elimina las líneas de la 3 a la 10.
-  Se vuelve a verificar funcionamiento en el navegador de internet añadiendo la ruta del dominio /home y en el html que muestra se ve el resultado del ```var_dump($this->request1_uri."\r");``` de la línea 24 del archivo **FrontController.php**
- **Al tener que modificar index.php no está cumpliendo objetivo**.


## Punto 2: RAMA walaa-Routing

-  Para acceder a la rama de walaa-Routing se ha de descartar los cambios realizados para la verificación de Musa-front. Para ello vamos a Git Bash y accedemos a la carpeta del dominio **/backend/www/practica.aplicaciones.web**.
-  Ejecutamos ``` git checkout -- public/index.php``` 
-  Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout walaa-Routing```
-  Para verificar que $routes de Routing carga la ruta de config.php o de routes.json se añade en la linea 23 del archivo Routing.php un ```var_dump($this->routes);```.
-  Una vez añadido y guardado,  se abre el dominio desde navegador de internet. 
-  No se visualiza nada ya que en index.php dispone de las líneas 3 y 4 que hace que la aplicación pare y no realice la funcion de FrontController.  Si se comentan dichas líneas, aparece un error de sintaxis en Routing.php, en la línea 22 falta un **;**.
-  En caso de solventar el problema, el var_dump($this->routes) nos entrega un booleano con valor true, por lo que **no nos está cumpliendo el objetivo**.


## Punto 3: RAMA Alex_RoutingInterface

- Para acceder a la rama de Alex_RoutingInterface se ha de descartar los cambios realizados para la verificación de walaa-Routing.
-  Para ello vamos a Git Bash y accedemos a la carpeta del dominio **/backend/www/practica.aplicaciones.web**.
-  Ejecutamos ``` git checkout -- public/index.php``` y ``` git checkout -- src/Routing.php```
-  Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Alex_RoutingInterface```
-  Para verificar si funciona nos tenemos que posicionar en el archivo Routing.php y cambiar el nombre de la función **getController** o **getAction**.
- Si antes de modificar nada no aparece error y al modificar estos nombres y cargar la pagina en el navegador de internet nos dá un error **Class Routing contains 1 abstract method and must therefore be declared abstract or implement the remaining methods (RoutingInterface::getController) in <b>/var/www/practica.aplicaciones.web/src/Routing.php** quiere decir que está correcto.


## Punto 4: RAMA Jordi-HTML_Response

- Para acceder a la rama de Jordi-HTML_Response se ha de descartar los cambios realizados para la verificación de Alex_RoutingInterface.
-  Para ello vamos a Git Bash y accedemos a la carpeta del dominio **/backend/www/practica.aplicaciones.web**.
-  Ejecutamos ``` git checkout -- public/index.php``` y ``` git checkout -- src/Routing.php```
-  Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Jordi-HTML_Response```
-  Al cargar dominio /home en el explorador de internet nos carga la vista creada y llamada en HomeController.
-  También carga la vista si se carga dominio/home/premium. 
-  **Funciona correctamente.**

## Punto 5: RAMA kyomi_AJAXform

-  Para acceder a la rama de kyomi_AJAXform ejecutamos: ` git checkout Jordi-HTML_Response`
-  Accedemos desde navegador de internet a la ruta ** practica.aplicaciones.web/contact**
-  **Vemos que no funciona ya que no carga el formulario y nos dá error.**

## Punto 6: RAMA Carmen-errors

-  Para acceder a la rama de Carmen-errors ejecutamos: ` git checkout Carmen-errors`
-  Accedemos desde navegador de internet a la ruta ** practica.aplicaciones.web/404**
-  Se realiza botón derecho encima del html que carga y damos a **inspeccionar elemento**.
-  Nos ponemos en la pestaña red o Network (dependiendo del navegador que usemos)
-  Se verifica que en la primera columna llamada **estado** nos devuelve código 404.
-  **Funciona correctamente ya que responde con el código de error correcto. **
