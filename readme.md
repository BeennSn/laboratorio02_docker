
# Laboratorio 02 - Docker Compose

Autor: Sinacay Nuñez, Ahrat Benjamin

## Descripción

El proyecto ejecuta tres copias de una API y una base de datos
PostgreSQL usando Docker Compose.

Las API se construyen localmente desde la carpeta api y muestran
mi nombre. La base de datos se prueba por separado; la API no
realiza consultas a PostgreSQL.

## Requisitos

- Docker funcionando con contenedores Linux.
- Docker Compose.
- Puertos 3000, 3001 y 3002 disponibles.

## Configuración

Copiar la plantilla de variables en PowerShell:

```powershell
Copy-Item .env.example .env
```

Completar la contraseña en .env.

Variables utilizadas:

- MESSAGE: mensaje que devuelve la API.
- POSTGRES_DB: nombre de la base de datos.
- POSTGRES_USER: usuario de PostgreSQL.
- POSTGRES_PASSWORD: contraseña de PostgreSQL.

El archivo .env está excluido mediante .gitignore.
.env.example se incluye como plantilla sin contraseña.

## Ejecutar el proyecto

Desde la carpeta del proyecto:

```powershell
docker compose config --quiet
docker compose up -d --build
docker compose ps
```

Consultar las tres API:

```powershell
curl.exe http://localhost:3000
curl.exe http://localhost:3001
curl.exe http://localhost:3002
```

Cada una responde con mi nombre y su número de instancia.
Todas usan el puerto interno 3000, pero tienen puertos externos
diferentes.

Para ver los logs:

```powershell
docker compose logs --tail 30
```

Para detener y eliminar los contenedores conservando los datos:

```powershell
docker compose down
```

## Base de datos y volumen

PostgreSQL usa el puerto interno 5432 y el volumen datos_postgres,
montado en /var/lib/postgresql/data.

Para ingresar con los valores del ejemplo:

```powershell
docker compose exec db psql -U benjamin -d laboratorio02
```

Se creó la tabla prueba_volumen y se guardó un registro.
Luego se salió con \q y se recreó el contenedor:

```powershell
docker compose up -d --force-recreate db
```

Se comprobó el registro con:

```powershell
docker compose exec db psql -U benjamin -d laboratorio02 -c "SELECT * FROM prueba_volumen;"
```

El registro permaneció, comprobando la persistencia del volumen.
La tabla de prueba se creó manualmente; no se crea al desplegar.

No utilizar docker compose down -v si se quieren conservar los datos.

## Tipos de redes en Docker

- bridge: conecta contenedores en un mismo host.
- host: comparte la red del host.
- overlay: conecta contenedores de distintos hosts mediante Swarm.
- macvlan: asigna una MAC propia a cada contenedor.
- ipvlan: permite distintas IP compartiendo la MAC de la interfaz principal.
- none: deja al contenedor sin conexión a otras redes.

En este proyecto usamos red_lab de tipo bridge.

## Tipos de volúmenes y montajes

- Volumen nombrado: Docker lo administra y tiene un nombre elegido.
- Volumen anónimo: Docker lo administra y le asigna un nombre aleatorio.

Otros montajes:

- Bind mount: comparte una carpeta o archivo del host.
- tmpfs: guarda datos temporales en memoria.

En este proyecto usamos un volumen nombrado: datos_postgres.

## Créditos y referencias

API original de Nobuyuki Matsui, con licencia MIT.
Se conserva su licencia en la carpeta api.

- [API original](https://github.com/nmatsui/hello-world-api)
- [Redes Docker](https://docs.docker.com/engine/network/drivers/)
- [Volúmenes Docker](https://docs.docker.com/engine/storage/volumes/)
- [Almacenamiento Docker](https://docs.docker.com/engine/storage/)
