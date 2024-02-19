# Creación de un balanceador de carga con Nginx

1. Nos metemos en el directorio donde vamos a hacer el balanceo.

```bash
C:\Users\alvar\Desktop\DevOps\balanceo_carga
```

2. Con este comando creamos 3 HTML con el contenido para visualizar.

```bash
echo "<h1>Hola Server 1</h1>" > index.1.html
```

```bash
echo "<h1>Hola Server 2</h1>" > index.2.html
```

```bash
echo "<h1>Hola Server 3</h1>" > index.3.html
```

3. Creamos 3 imágenes en docker cada una con su puerto y la imagen para el balanceador

```bash
docker run --rm -v C:/Users/alvar/Desktop/DevOps/balanceo_carga/index.1.html:/usr/share/nginx/html/index.html -p 8082:80 nginx
```

```bash
docker run --rm -v C:/Users/alvar/Desktop/DevOps/balanceo_carga/index.2.html:/usr/share/nginx/html/index.html -p 8083:80 nginx
```

```bash
docker run --rm -v C:/Users/alvar/Desktop/DevOps/balanceo_carga/index.3.html:/usr/share/nginx/html/index.html -p 8084:80 nginx
```

```bash
docker run --rm -v C:/Users/alvar/Desktop/DevOps/balanceo_carga/index.3.html:/usr/share/nginx/html/index.html -p 8084:80 nginx
```

4. Usamos este comando para comprobar cada IP de los contenedores, para después usarlas en el el nginx.conf.

```bash
docker inspect
```

Así quedaría el archivo nginx.conf:

- server 172.17.0.3:80 down; sirve para desactivar un servidor.
- server 172.17.0.4:80 backup; sirve para tenerlo de repuesto.

```jsx
upstream backend {
server 172.17.0.3:80; #server 1
server 172.17.0.4:80; # server 2
server 172.17.0.5:80; # server 3
}

server {
location / {
proxy_pass http://backend/;
}
 
location /server1{
    proxy_pass <http://172.17.0.3/>;
    }

location /server2/ {
    proxy_pass <http://172.17.0.4/>;
    }
location /server3/ {
    proxy_pass <http://172.17.0.5/>;
    }
}
```
