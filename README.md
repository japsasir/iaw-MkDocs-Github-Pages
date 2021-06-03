# Iaw-MkDocs-Github-Pages
Creación de un sitio web estático con MkDocs y GitHub Pages

> IES Celia Viñas (Almería) - Curso 2020/2021
Módulo: IAW - Implantación de Aplicaciones Web
Ciclo: CFGS Administración de Sistemas Informáticos en Red

## Pasos a seguir

### Crea un repositorio de páginas estáticas de Github
Creamos un repositorio como en la practica Jekyll para guardar el contenido de la web.

### Prepara una máquina para MKdocs

Usaremos una máquina de Amazon con los correspondientes puertos abiertos en el grupo de seguridad:

- SSH 22
- HTTP 80
- 8000 (Acceso a web vía MKdocs)

Instalamos Git (En Amazon AWS viene por defecto), Docker y Docker-Compose en la instancia:


```bash
apt update
apt-get install git
apt install docker docker-compose -y
systemctl enable docker
systemctl start docker
```

Clonamos el contenido del repositorio en dicha máquina.

`sudo git clone https://github.com/japsasir/iaw-MkDocs-Github-Pages.git`

Le proporcionamos todos los permisos para que el usuario Docker pueda crear directorios dentro.

`chmod 777 $directorio`

Y nos dirigimos al directorio con

`cd $directorio`

### Crea tu sitio

#### Mkdocs-material
Lanzamos el contenedor de MKdocs, que creará la estructura necesaria para el proyecto.

`docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material new $proyecto`
Donde '$proyecto' será el nombre que le quieras dar a tu proyecto.

![Estructura de archivos](https://i.imgur.com/KxI7kLf.png)
La estructura creada.


#### Configurando mkdocs.yml
Podemos observar un archivo llamado **mkdocs.yml** (Misma extensión que los que usamos con docker-compose) que nos permite alterar ciertas opciones: Nombre del sitio, nav (sistema de archivos) y el tema a utilizar (theme)

![mkdocs.yml](https://i.imgur.com/v4UjO0h.png)
Mi archivo mkdocs.yml

#### Crear un servidor de desarrollo local con 'serve'

Una vez que hemos creado la estructura del proyecto podemos empezar el desarrollo de nuestro sitio iniciando un contenedor Docker con MkDocs y el theme Material.

Nota: Vamos a crear el contenedor desde el directorio principal el proyecto porque necesitamos crear un volumen de tipo bind mount entre nuestra máquina y el contenedor Docker.

`docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material`

Una vez iniciado el contenedor podemos acceder a la URL http://localhost:8000 desde un navegador web para ver el estado actual del sitio web que estamos creando.

Ahora podemos editar nuestros archivos Markdown y ver cómo se va generando el sitio web de forma inmediata.

Para actualizar el contenido del blog en nuestro repositorio usaremos los comandos git.

```bash
git add --all
git commit -m "Comentario descriptivo"
git push
```

![](https://i.imgur.com/a1kiFZV.png)
Recibiremos este aviso.

Accedemos mediante esta url
http://3.84.225.87:8000/

Es el método más adecuado para hacer diferentes pruebas. Podemos detener el proceso haciendo **control+c** en el terminal. (Recordemos, necesario para recargar la configuración del archivo .yml)

Nota: La opción `--force_polling` actualiza el contenido del sitio de forma dinámica cuando realizas cambios en los archivos del proyecto.

![](https://i.imgur.com/jMgcMsE.png)
El aspecto de la página lanzada.

## Referencias
- Guía original de Jose Juan Sánchez	https://josejuansanchez.org/iaw/practica-mkdocs/index.html

