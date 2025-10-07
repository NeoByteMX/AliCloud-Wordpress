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

# AliCloud-Wordpress
Tutorial about how to create your WordPress site on Alibaba Cloud using their free trial.

* The first think you need of course is an account, create your and then follow me.

## Creating the free trial.

Once you have created your account and added a card, is time to use the free trial. Go to https://www.alibabacloud.com/en/free?_p_lc=1&TargetUserType=for-enterprise

And select the ECS t5 Instance (1C1G 1 Year).

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/b62bc5d5-7a6c-4a50-b37b-2d2508313ea9)


1. Select your region, I'll choose US(Silicon Valley)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/81d3a864-b273-4ab3-adfc-6bc28b867e9f)

2. Select your OS, I'll choose Ubuntu as is the most popular

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/c3f34454-1c21-4805-bdbc-2811901ff7b3)

3. You can select your Customer Password or use a Key Pair, I'll use a custom password as is beginner-friendly.

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/763b7344-612d-494b-9787-a2907a9695fd)

Let the other things as default (or you can't use the free trial)

Then select the box ECS Terms of Service | Product Terms of Service and create order.

## Login in through SSH
After creating your ECS free trial, go to your Console.

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/3ad9587a-d6ec-4c9b-bee0-3a67a8d057ff)
![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/f8db298c-6ae3-476c-aeae-d81df57f67ab)

And go to ECS

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/307b7211-ef22-4be6-a3be-ee7e3c83f723)

This is the direct link: https://ecs.console.aliyun.com/home

We can see one running ECS instance in the region which you set, in my case US (silicon Valley)



Select "instances" in the left menu bar:

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/9aeceafa-c51d-4208-9d33-0a8fa160db0a)

You will see again your instance (check your region)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/8d50a487-a14e-48b8-b0f2-3fc527858d4a)

You can see your instance Public IP (I'm using and EIP but doesn't matter)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/e2e3a9db-02f0-47b1-89d5-e3f0847c2aba)

#### SSH
The command you will need is `SSH root@YOUR_IP`, once you enter this it will ask you for your fingerprint, type yes, and then type your password (that you use to create your ECS).

After logging on to the ECS with SSH, run the apt update command to update apt source: 
`` apt update ``
<img width="960" height="480" alt="image" src="https://github.com/user-attachments/assets/108bd37e-ec4d-439c-aa5e-1b7ef6d97cbc" />

Install the MySQL service and client:
`` apt install -y mysql-server mysql-client ``
<img width="960" height="480" alt="image" src="https://github.com/user-attachments/assets/38bb01c7-80fa-42a0-ba86-378faef43e19" />

During installation, you will be prompted to enter a password for MySQL. Memorize this password, because later it will be used for connecting to MySQL.

Start the MySQL service:
`` service mysql start ``

<img width="847" height="64" alt="image" src="https://github.com/user-attachments/assets/40bf695e-e89c-44f7-9342-db0136947fb6" />

Enter the following command, and then enter the password you set when installing the MySQL service. A successful logon represents a successful installation:
`` mysql -uroot -p ``

<img width="842" height="241" alt="image" src="https://github.com/user-attachments/assets/e041f611-63d9-42e8-bbfb-247dcec6176d" />

When you are logged on, create a WordPress database in MySQL:
``CREATE DATABASE wordpress; ``
Exit MySQL:
`` exit; ``

Install the Nginx, PHP 7.0-FPM, and PHP 7.0-MySQL packages:
`` apt install -y nginx php7.0-fpm php7.0-mysql ``

<img width="834" height="665" alt="image" src="https://github.com/user-attachments/assets/5b8ab46f-8bee-4c0a-a7b0-c5452ba33254" />

Start the Nginx Web service:
`` service nginx restart ``

When Nginx is started, enter the EIP bound to the ECS in your browser, and the following screen appears:
<img width="571" height="215" alt="image" src="https://github.com/user-attachments/assets/4d41f8c8-ed52-4def-bd3d-699e9a62803c" />

4 Download the WordPress installation package
Download the WordPress installation package:
`` wget https://labex-ali-data.oss-us-west-1.aliyuncs.com/wordpress/wordpress-6.1.1.tar.gz ``

<img width="1590" height="196" alt="image" src="https://github.com/user-attachments/assets/10ba0e3f-2b94-488a-a897-6c98fe196387" />

Unzip the WordPress installation package to the /var/www directory.
`` tar -xzf wordpress-6.1.1.tar.gz -C /var/www/ ``

Change the directory permission to www-data users and user groups.
`` chown -R www-data:www-data /var/www ``

5. Modify the Nginx configuration file
Before editing an important configuration file, it is best to back up the file in advance to prevent the error operation can not be restored:
`` cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak ``

Using vim to edit and modify the Nginx configuration file /etc/nginx/sites-available/default as follows:
`` vim /etc/nginx/sites-available/default ``

server {
    listen 80;
    root /var/www/wordpress;
    index index.php index.html index.htm;


    location / {
        try_files $uri $uri/ /index.php?q=$uri&$args;
    }

    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/www;
    }

    location ~ \.php$ {
        try_files $uri =404;

        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        fastcgi_index index.php;
        include /etc/nginx/fastcgi_params;
          include /etc/nginx/fastcgi.conf;
    }
}


Go to the /var/www/wordpress directory, copy wp-config-sample.php to wp-config.php, and then modify wp-config.php with vim:

`` cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php ``

`` vim /var/www/wordpress/wp-config.php ``

Configure the database connection information in the file, as shown in the following figure:
<img width="840" height="668" alt="image" src="https://github.com/user-attachments/assets/90454686-90a8-49bc-b267-ca159577b31f" />

Restart Nginx and PHP 7.0-FPM:
``service nginx restart``

``service php7.0-fpm restart``

6. WordPress welcome screen
Enter the following address in your browser. Make sure you enter the EIP address for the IP address. If that directs you to the language selection screen, it means that WordPress is installed successfully.

http://<EIP address>/wp-admin/install.php
<img width="1151" height="689" alt="image" src="https://github.com/user-attachments/assets/0b37f1e0-5caa-4f10-91e4-046d4a685edc" />

Click Continue to continue setting WordPress:
<img width="884" height="914" alt="image" src="https://github.com/user-attachments/assets/ee2abccd-1631-464d-8859-cf29d9ee5a5b" />

In this page, set the title of the WordPress site, the username and password of the site administrator, and the mailbox information.

image desc
Wordpress site deployment success! Login WordPress Background:

image desc
View WordPress Website Page:

image desc

I hope this quick tutorial helps you!
