# 1.Explica métodos para 'abrir' una consola/shell a un contenedor que se está ejecutando.
Como estamos usando Code, podemos desde el menú contextual de los contenedores hacer ‘Attach shell’, tambien podemos abrir una consola con el comando docker exec -it <nombre_del_contenedor_o_id> /bin/bash

# 2. En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor
con los parámetros -it 

# 3. ¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?

services:
  asir_bind9:
    container_name: asir_bind9
   
    image: ubuntu/bind9
   
    platform: linux/amd64
    ports:
      - 53:53
    
    volumes:
      - ./conf:/etc/bind
      - ./zonas:/var/lib/bind
  cliente:
    container_name: cliente
    image: ubuntu
    platform: linux/amd64
    tty: true
    stdin_open: true
    
    dns:
      - 172.28.5.1
   
    
networks:
  bind9_subnet:
    external: true

# 4. ¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?

networks:
      bind9_subnet:
        ipv4_address: aqui pondriamos la ip que queremos asignarle

ejemplo:

networks:
      bind9_subnet:
        ipv4_address: 172.28.5.1

# 5. ¿Que comando de consola puedo usar para saber las ips de los contenedores anteriores? Filtra todo lo que puedas la salida.

docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nombre_del_contenedor, tambien se podria hacer con el comando docker network inspect (nombre de la red del contenedor)

# 6 ¿Cual es la funcionalidad del apartado "ports" en docker compose?

Su funcionalidad es mapear los puertos con los que queremos trabajar.

# 7 ¿Para que sirve el registro CNAME? Pon un ejemplo

Sirve para establecer una asociación entre un nombre de dominio y otro nombre de dominio, también es comúnmente utilizado para crear alias  de un dominio principal.

ejemplo: 

dominio principal : www.google.com

sbdominio google.com

www.google.com CNAME google.com

# 8 ¿Como puedo hacer para que la configuración de un contenedor DNS no se borre si creo otro contenedor?

Primero tendrias que crear nuevos directorios en el host y con la opcion volumes mapearlos con los del contenedor.

# 9 Añade una zona tiendadeelectronica.int en tu docker DNS que tenga
# www a la IP 172.16.0.1
# owncloud sea un CNAME de www
# un registro de texto con el contenido "1234ASDF"
# Comprueba que todo funciona con el comando "dig"
# Muestra en los logs que el servicio arranca correctamente

se crea el archivo tiendadeelectronica y en el se escribe lo siguiente :

$TTL 38400	; 10 hours 40 minutes  
@		IN SOA	ns.tiendadeelectronica.int. some.email.address. (  
				10000002   ; serial  
				10800      ; refresh (3 hours)  
				3600       ; retry (1 hour)  
				604800     ; expire (1 week)  
				38400      ; minimum (10 hours 40 minutes)  
				)  
@		IN NS	ns.tiendadeelectronica.int.  
ns		IN A		172.28.5.1  
test	IN A		172.28.5.4  
www		IN A		172.28.5.7  
alias	IN CNAME	test  
texto	IN TXT		mensaje  
www	    IN A	    172.16.0.1  
owncloud    IN CNAME	www  
txt	    IN TXT	    “1234ASDF”  


los dig de comprobacion serian:

dig @ipcontenedor www.tiendadeelectronica.int
dig CNAME @ipcontenedor owncloud.tiendadeelectronica.int
dig -t TXT @ipcontenedor txt.tiendadeelectronica.int

y para mostrar los logs nos tendriamos que ir a la extenion de docker en visual studio code , hariamos click derecho sobre el contenero e iriamos al apartado view logs


