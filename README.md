# PARTE 1

## **Para generar un nuevo virtual-host se ha de acceder al archivo backend.pp alojado en la ruta C:\Users\Carmen\backend\puppet\manifests.**

### Se realiza copia de las líneas 54 a la 97 y se copian a la línea 98.
Una vez copiado se modifica y se añade:
##### Línea 98: 
```
nginx::resource::server{"practica.aplicaciones.web":
```
###### Línea 101:
```
www_root  => "/var/www/practica.aplicaciones.web/public",
```
##### Línea 106 :
```
nginx::resource::location{ 'practica.aplicaciones.web/':
```
##### Línea 108: 
```
  server      => "practica.aplicaciones.web",
```
##### Línea 123:
```
nginx::resource::location { 'practica.aplicaciones.web/index.php':
```
##### Línea 125:
```
  server         => "practica.aplicaciones.web",
```
## Se levanta servidor realizando **vagrant up**.
## Una vez realizado se realiza **vagrant provision--provision-with=puppet**
## Se abre aplicación notepad como administrador y se abre el archivo **host**. y se añade el nombre del dominio a la linea donde disponemos de la IP de nuestro servidor. 
#####**192.168.33.10 aplicaciones.web proyecto.web tuttifrutti.shop practica.aplicaciones.web**
## Una vez realizado provision, nos vamos a github y copiamos el enlace 
## En GitBush tenemos que realizar es un ```vagrant ssh``` desde la carpeta **/backend/** para acceder a nuestro servidor e introducirnos dentro de la carpeta del dominio. **/var/www/practica.aplicaciones.web**.
## Una vez ubicados en esta carpeta se ejecuta ``` composer dump-autoload```.


# PARTE 2

##**RAMA Musa-front**
### Nos vamos a Git Bash y accedemos a la carpeta del dominio */backend/www/practica.aplicaciones.web*.
### Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Musa-front```.
### Una vez en la rama de Musa-front, nos colocamos en el navegador de internet y escribimos el dominio **practica.aplicaciones.web**.
### Verificamos que no carga nada en pantalla.
### Se revisa index.php y se elimina las líneas de la 3 a la 10.
### Se vuelve a verificar funcionamiento en el navegador de internet añadiendo la ruta del dominio /home y en el html que muestra se ve el resultado del ```var_dump($this->request1_uri."\r");``` de la línea 24 del archivo **FrontController.php**
### Al tener que modificar index.php **no está cumpliendo objetivo**.


##**RAMA walaa-Routing**
### Para acceder a la rama de walaa-Routing se ha de descartar los cambios realizados para la verificación de Musa-front.
### Para ello vamos a Git Bash y accedemos a la carpeta del dominio */backend/www/practica.aplicaciones.web*.
### Ejecutamos ``` git checkout -- public/index.php```.
### Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout walaa-Routing```.
### Para verificar que $routes de Routing carga la ruta de config.php o de routes.json se añade en la linea 23 del archivo Routing.php un ```var_dump($this->routes);```.
### Una vez añadido y guardado,  se abre el dominio desde navegador de internet. 
### No se visualiza nada ya que en index.php dispone de las líneas 3 y 4 que hace que la aplicación pare y no realice la funcion de FrontController. 
### Si se comentan dichas líneas, aparece un error de sintaxis en Routing.php, en la línea 22 falta un **;**.
### En caso de solventar el problema, el var_dump($this->routes) nos entrega un booleano con valor true, por lo que **no nos está cumpliendo el objetivo**.


##**RAMA Alex_RoutingInterface**
### Para acceder a la rama de Alex_RoutingInterface se ha de descartar los cambios realizados para la verificación de walaa-Routing.
### Para ello vamos a Git Bash y accedemos a la carpeta del dominio */backend/www/practica.aplicaciones.web*.
### Ejecutamos ``` git checkout -- public/index.php``` y ``` git checkout -- src/Routing.php```
### Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Alex_RoutingInterface```.
### Para verificar si funciona nos tenemos que posicionar en el archivo Routing.php y cambiar el nombre de la función **getController** o **getAction**.
### Si al modificar estos nombres y cargar la pagina en el navegador de internet nos dá un error **Class Routing contains 1 abstract method and must therefore be declared abstract or implement the remaining methods (RoutingInterface::getController) in <b>/var/www/practica.aplicaciones.web/src/Routing.php** quiere decir que está correcto.


##**RAMA Jordi-HTML_Response**
###Para acceder a la rama de Jordi-HTML_Response se ha de descartar los cambios realizados para la verificación de Alex_RoutingInterface.
### Para ello vamos a Git Bash y accedemos a la carpeta del dominio */backend/www/practica.aplicaciones.web*.
### Ejecutamos ``` git checkout -- public/index.php``` y ``` git checkout -- src/Routing.php```
### Para colocarnos en la rama que necesitamos y ejecutamos ``` git checkout Jordi-HTML_Response```.
