# Traefik docker-compose

Traefik es un proxy inverso HTTP moderno y un equilibrador de carga (load balancer) al mismo tiempo.

Características (usadas en este demo):

*  Renovación automática de certificados SSL junto a [Let's Script](https://letsencrypt.org/).
*  Proxy reverso entre contenedores, maneja multiples contenedores y se las arregla para manejar subdominios bajo un mismo dominio.


Pasos para correr este docker-compose:

1. Antes de cualquier cosa corre el script `run_create_traefik_network.sh` para crear una red de docker, toto ello para que traefik pueda comunicarse con tus contenedores, la red creada tiene el nombre de `traefik_network` y es el nombre que debes usar como red externa en tus contenedores.

2. Modifica los siguientes datos en el archivo `traefik.toml`:
   * `email="your_email@your_domain.com"`, agrega tu email.
   * `domain = "your_domain.com"`, agrega tu dominio.
   * `network = "traefik_network"`, solo si personalizaste el nombre de la red, de lo contrario no lo cambies.

3. Ya puedes levantar el servicio de Traefik con el `docker-compose.yml`:

```
docker-compose up -d
```

4. Posteriormente es posible ejecutar el `docker-compose` de ejemplo con ruta `example/docker-compose.nginx-demo.yml`, el cual es un servidor nginx (con configuraciones preestablecidas) corriendo en el puerto 80:

```
docker-compose -f docker-compose.nginx-demo.yml up -d
```


El archivo `.env` de la carpeta `example` es interesante ya que en el agregas los dominios asociados al contenedor, además del puerto en el que esta corriendo tu aplicación/programa/servicio/etc.

Puedes usar uno o mas subdominios para apuntar a un mismo contenedor, si usas múltiples dominios usa `,` (coma) para separalos:

```
CONTAINER_DOMAIN=sub_domain_1.your_domain.com,sub_domain_2.your_domain.com
CONTAINER_PORT=80
```

Un ejemplo clásico es:

```
CONTAINER_DOMAIN=www.mi_sitio.com,mi_sitio.com
CONTAINER_PORT=80
```

`CONTAINER_PORT` hace referencia al puerto en el que se expone el servicio que tu aplicación/programa/servicio/etc ofrece, este puerto no se externaliza, se maneja internamente y solo Traefik se comunica con el mismo mediante la red creada (`traefik_network`).