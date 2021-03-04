Creración de una imagen de Docker

Este código nos muestra un ejemplo de cómo podemos crear un DockerFile y probar la creación de contenedores
1.-Nos posicionamos en la carpeta donde se encuentra nuestro Dockerfile y ejecutamos el siguiente comando:
docker image build -t  pruebaf .
Traduciendo: Crea una imagen con la etiqueta pruebaf  y busca en la carpeta actual el Dockerfile

2.-Ahora veremos si la imagen se encuentra dentro de nuestro Docker hub
docker  image ls

3.- levantamos un contenedor de prueba con la imagen que acabamos de crear 
docker container run --rm -it -p 8081:80 --name pruebacontainer pruebaf
Traduciendo: ejecuta un contenedor que se llame pruebacontainer y utilice la imagen pruebaf
En el puerto 8081 de mi maquina local(unimos al puerto 80 del contenedor)  con el argumento –rm el contenedor se borrara automáticamente de los contenedores creados y el argumento -it Significa que puede iniciar sesión en su contenedor usando TTY(terminal)

4.- Una vez probada la imagen y que funciona ahora crearemos un ContainerRegistry para ahora subir nuestra imagen a azure (Esto lo hacemos en el portal)
4.1.- Dentro de ContainerRegistry tenemos que ir a la opción Access Keys  para activar el Admin user para subir nuestras imágenes de nuestro docker local a azure.
5.- para continuar ahora tenemos que instalar azure cli dentro de nuestra maquina local para ejecutar cli. 
6.-iniciar sesión en azure 
  az login

7. – iniciar sesión al  ContainerRegistry  con el siguiente comando.
az acr login --name registroimagenesenevf --username registroimagenesenevf --password wKTZcUDgX8MVcY+UV+1==PfnY5f6IF8e

8.- una vez que iniciemos sesión ahora tenemos que cambiar el tag de nuestra imagen con el siguiente comando
docker tag pruebaf registroimagenesenevf.azurecr.io/pruebaf

9.- Si todo lo hacemos bien entonces ahora si subiremos nuestra imagen a azure.
docker push registroimagenesenevf.azurecr.io/pruebaf

10.- Ahora creamos un contenedor con la imagen que acabamos de subir:
az container create --resource-group RGServiciosIA --name contenedorpruebaenevf --image registroimagenesenevf.azurecr.io/pruebaf --dns-name-label pruebaevfn --ports 80
Nota: nos pedirá las credenciales del admin del ContainerRegistry
