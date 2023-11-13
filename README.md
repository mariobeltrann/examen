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



