Proyecto final del bootcamp  en donde se integran practicas de devops usando contenedores, kubernetes, jenkins, recoleccion de metricas con prometheus , visualizacion de las mismas con grafana y unas cuestiones de seguridad. 

Este es un fork del proyecto original : https://github.com/krishnaacharyaa/wanderlust , sin embargo para mayor comodidad y  revision del mismo, solo se incluyen a partir de los Dockerfile y docker-compose.yml que son el inicio de la contenedorizacion de la aplicacion , por lo que la gran cantidad de archivos que la aplicacion contenia para ser ejecutada localmente no se incluyen , ya que para su posterior ejecucion se empaquetaron y subieron al registry de Docker hub las imagenes que fueron usadas para poder ejecutar el cluster de kubernetes. 
