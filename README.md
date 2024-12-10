Kubernetes es ahora funcional, se agregaron unos pequeños cambios en los siguientes archivos: 

1- backend-deployment-yml : se agregó el puerto de mongoDB: 27017


2- mongo-deployment.yml : se agregó al patch la ruta absoluta del data del backend ( usen su ruta ). 



ATENCION: 
para poder correr el proyecto deben de utilizar este comando: kubectl port-forward svc/api-service 8080:8080

este comando lo que hará es redireccionar del puerto 8080 del localhost -> al puerto 8080 del pod del backend , esto para evitar conflictos con el frontend que usa localhost:3000
