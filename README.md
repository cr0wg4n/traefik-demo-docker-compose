# Traefik docker-compose

Traefik es un proxy inverso HTTP moderno y equilibrador de carga (load balancer).

Características:

*  Renovación automática de certificados SSL junto a [Let's Script](https://letsencrypt.org/).
*  Proxy reverso entre contenedores, maneja contenedores con distintos subdominios.


Pasos para correr este docker-compose:

1. Antes de cualquier cosa corre el script `run_create_traefik_network.sh` para crear una red de docker, toto ello para que traefik pueda comunicarse con tus contenedores, la red generada tiene el nombre de `traefik_network` y es la es el nombre que debes usar como red externa en tus contenedores.

2. modifica los siguientes datos en el archivo `traefik.toml`:
   * `email="your_email@your_domain.com"`, agrega tu email.
   * `domain = "your_domain.com"`, agrega tu dominio.
   * `network = "traefik_network"`, solo si personalizaste el nombre de la red, de lo contrario no lo cambies.

3. Posteriormente ya puedes usar el archivo `example/docker-compose.nginx-demo.yml` con el comando:

```
docker-compose -f docker-compose.nginx-demo.yml up -d
```


El archivo `.env` es interesante ya que en el agregas los dominios asociados al contenedor, además del puerto en el cual esta corriendo tu aplicación/programa/servicio/etc.

Puedes usar uno o mas subdominios para apuntar a un mismo sitio, si usas
múltiples dominios usa `,` (coma) para separalos:

```
CONTAINER_DOMAIN=sub_domain_1.your_domain.com,sub_domain_2.your_domain.com
CONTAINER_PORT=80
```

Un ejemplo clásico es:

```
CONTAINER_DOMAIN=www.mi_sitio.com,mi_sitio.com
CONTAINER_PORT=80
```