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
