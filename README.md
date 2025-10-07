# Despliega WordPress en Alibaba Cloud con ECS

¡Hola! 👋 Este es un tutorial paso a paso para crear tu propio sitio de WordPress desde cero en **Alibaba Cloud**, aprovechando su prueba gratuita del servicio **Elastic Compute Service (ECS)**.

---

## Prerrequisitos

* Una cuenta de Alibaba Cloud.
* Un método de pago válido (necesario para activar la prueba gratuita, no se te cobrará nada).

---

## 1. Creación de la Instancia ECS (Prueba Gratuita)

Lo primero es activar la prueba gratuita y configurar nuestra máquina virtual (instancia).

1.  **Ve a la página de la prueba gratuita:** [Alibaba Cloud Free Trial](https://www.alibabacloud.com/es/free).
2.  Busca y selecciona la opción **Elastic Compute Service (ECS)**. Generalmente ofrecen una instancia `t5` o `ecs.t6-c1m1.large` de 1 CPU y 1 GB de RAM por 1 año. Haz clic en **"Probar ahora"**.

    ![Imagen de la página de selección de prueba gratuita de Alibaba Cloud](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/b62bc5d5-7a6c-4a50-b37b-2d2508313ea9)

3.  **Configura tu instancia:**
    * **Región:** Elige la que prefieras. Para este tutorial, usaré **US (Silicon Valley)**.
    * **Sistema Operativo:** Selecciona **Ubuntu** (usaremos la versión 22.04 LTS). Es muy popular y tiene un gran soporte de la comunidad.
    * **Autenticación:** Puedes usar una contraseña o un par de claves SSH. Para que sea más sencillo para principiantes, elegiremos **Contraseña Personalizada**. Asegúrate de crear una contraseña fuerte y guárdala en un lugar seguro.

    ![Imagen de las opciones de configuración de la instancia ECS](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/763b7344-612d-494b-9787-a2907a9695fd)

4.  Deja las demás opciones con sus valores por defecto.
5.  Acepta los términos del servicio y haz clic en **"Create Order"** para finalizar la creación.

---

## 2. Conexión a la Instancia vía SSH

Una vez creada la instancia, necesitamos conectarnos a ella para empezar a instalar todo.

1.  Ve a la [Consola de Alibaba Cloud](https://ecs.console.aliyun.com/home).
2.  En el menú de la izquierda, navega a **Instances & Images** > **Instances**.
3.  Asegúrate de estar en la región correcta. Verás tu nueva instancia en estado "Running".
4.  Localiza y copia la **Dirección IP Pública** (`Public IP Address`) de tu instancia.

    ![Imagen del panel de control de la instancia ECS mostrando la IP Pública](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/e2e3a9db-02f0-47b1-89d5-e3f0847c2aba)

5.  Abre una terminal en tu computadora (Terminal en macOS/Linux o PowerShell/WSL en Windows) y conéctate usando el siguiente comando. Reemplaza `TU_IP_PÚBLICA` con la IP que copiaste.

    ```bash
    ssh root@TU_IP_PÚBLICA
    ```

6.  La primera vez que te conectes, te pedirá confirmar la autenticidad del host. Escribe `yes` y presiona Enter.
7.  Luego, introduce la contraseña que creaste para la instancia. Si todo es correcto, ¡ya estarás dentro de tu servidor! 💻

---

## 3. Instalación del Servidor Web (LEMP Stack)

Para que WordPress funcione, necesitamos instalar un conjunto de software conocido como "stack LEMP", que consiste en:
* **L**inux (nuestro sistema operativo Ubuntu).
* **E**Nginx (nuestro servidor web, se pronuncia "Engine-X").
* **M**ySQL (nuestra base de datos).
* **P**HP (el lenguaje en el que está escrito WordPress).

### Actualización de Paquetes
Primero, actualizamos la lista de paquetes del sistema operativo.

```bash
apt update
apt upgrade -y
```

### Instalación de Nginx, MySQL y PHP

Ahora instalamos todo el software necesario con un solo comando. Usaremos PHP 8.1, que es una versión moderna y segura.
```bash
apt install -y nginx mysql-server php8.1-fpm php8.1-mysql
```

### Configuración de la Base de Datos
WordPress necesita una base de datos para guardar toda la información de tu sitio (publicaciones, usuarios, configuraciones, etc.).

Inicia el servicio de MySQL.

```Bash
service mysql start
```

Accede a la consola de MySQL. Te pedirá la contraseña de root que configuraste durante la instalación.

```Bash
mysql -uroot -p
```

Crea la base de datos para WordPress.

```SQL
CREATE DATABASE wordpress;
```

Sal de la consola de MySQL.

```SQL
exit;
```

## 4. Instalación y Configuración de WordPress
¡Es hora de descargar y preparar WordPress!

Descarga la última versión de WordPress directamente desde su sitio oficial. Este enlace siempre apunta al archivo más reciente.

```Bash
wget [https://wordpress.org/latest.tar.gz](https://wordpress.org/latest.tar.gz)
```
Descomprime el archivo en el directorio /var/www/, que es donde Nginx busca los sitios web por defecto.

```Bash
tar -xzvf latest.tar.gz -C /var/www/
```

Dale a Nginx los permisos necesarios sobre los archivos de WordPress.

```Bash
chown -R www-data:www-data /var/www/wordpress
```

Crea el archivo de configuración de WordPress a partir del archivo de ejemplo.

```Bash
cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
```

Ahora, edita el archivo de configuración para conectar WordPress con la base de datos que creamos. Puedes usar nano o vim.

```Bash
nano /var/www/wordpress/wp-config.php
```

Busca las siguientes líneas y modifícalas con tus datos:

DB_NAME: 'wordpress'

DB_USER: 'root'

DB_PASSWORD: 'la_contraseña_que_pusiste_para_mysql'

```PHP
// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'root' );

/** Database password */
define( 'DB_PASSWORD', 'tu_contraseña_de_mysql' );
```

Guarda los cambios y cierra el editor (en nano, es Ctrl+X, luego Y, y Enter).

## 5. Configuración de Nginx para WordPress
El último paso en la terminal es decirle a Nginx cómo servir nuestro sitio de WordPress.

(Opcional pero recomendado) Haz una copia de seguridad del archivo de configuración por defecto.

```Bash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
```

Edita el archivo de configuración de Nginx.

```Bash
nano /etc/nginx/sites-available/default
```

Borra todo el contenido y reemplázalo con la siguiente configuración. Importante: Fíjate en la línea fastcgi_pass, que ahora apunta a php8.1-fpm.sock.

```Nginx

server {
    listen 80;
    root /var/www/wordpress;
    index index.php index.html index.htm;
    server_name TU_IP_PÚBLICA; # O tu dominio si ya tienes uno

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Guarda y cierra el archivo.

Reinicia los servicios de Nginx y PHP para que los cambios surtan efecto.

```Bash
service nginx restart
service php8.1-fpm restart
```

## 6. Finalizar la Instalación desde el Navegador
¡Lo lograste! La configuración en el servidor está completa. Ahora solo queda finalizar la instalación desde la famosa interfaz de WordPress.

Abre tu navegador web y ve a la dirección IP pública de tu instancia:
http://TU_IP_PÚBLICA

Serás recibido por la pantalla de bienvenida de WordPress, donde deberás elegir tu idioma.

Sigue los pasos en pantalla:

Título del sitio: El nombre que quieres para tu web.

Nombre de usuario: El nombre de tu usuario administrador (¡no uses "admin"!).

Contraseña: Una contraseña fuerte para tu usuario.

Tu correo electrónico: Para notificaciones y recuperación de contraseña.

Haz clic en "Instalar WordPress" y, en unos segundos, ¡tu sitio estará listo!

<img width="884" height="914" alt="image" src="https://github.com/user-attachments/assets/ee2abccd-1631-464d-8859-cf29d9ee5a5b" />


# ¡Felicidades! 🎉
Has desplegado exitosamente un sitio de WordPress en Alibaba Cloud. Ahora puedes iniciar sesión en tu panel de administración (http://TU_IP_PÚBLICA/wp-admin) y empezar a crear contenido.

Próximos Pasos
Apuntar un dominio: Compra un dominio y apúntalo a la IP de tu instancia.

Instalar un certificado SSL: Usa Let's Encrypt para añadir HTTPS y asegurar tu sitio.

Explorar temas y plugins: ¡Personaliza tu sitio a tu gusto!

Espero que este tutorial te haya sido de gran ayuda. ¡Te deseo mucho éxito!
