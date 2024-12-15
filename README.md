# Self-Hosting Server Setup

This repository contains the resources and documentation for a project focused on setting up and managing a self-hosted web server at home.

## Objectives

The main goal of this project is to develop skills in:

- Hosting a web server on a personal network.
- Configuring domains, DNS, and router settings.
- Setting up secure connections (HTTPS).
- Automating deployments with tools like Docker, Vagrant, or Ansible.
- Conducting server performance testing.

## Features

1. **Domain and DynDNS Configuration**

   - Register a custom domain.
   - Link the domain to a dynamic IP using dynamic DNS services.

2. **Router Port Forwarding**

   - Forward HTTP (80) and HTTPS (443) ports to the server.
     ![alt text](modificacion-puertos-router.PNG)

3. **Web Server Setup**

   - Use Apache 2.4+ for server deployment.
   - Configure custom error pages (e.g., 404 errors).
   - Set up basic authentication for admin and status routes.

4. **Performance Testing**

   - Evaluate server performance under different loads with Apache Benchmark (ab).

5. **Secure Connections**

   - Implement SSL/TLS using self-signed certificates or services like Let’s Encrypt.

6. **Automated Deployment**
   - Deploy using Docker, Vagrant, or Ansible.

## Tools and Technologies

- **Web Server:** Apache 2.4+
- **SSL/TLS:** Let’s Encrypt
- **Performance Testing:** Apache Benchmark
- **Automation:** Docker, Vagrant, Ansible
- **Monitoring:** Uptime Kuma or Apache `mod_status`

## Installation and Usage

### Requirements

- A personal computer to act as the server.
- A registered domain.
- Internet access for dynamic DNS and domain linking.

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/self-hosting-server.git
   cd self-hosting-server
   ```

He creado un dominio en no-ip que se llama peanutbutterjelly y la ip es la ip publica de la red de casa
He creado una maquina virtual con vagrant a la que he asignado una ip de 192.168.68.107 para que este dentro de la red publica
He configurado las redes NAT/PAT para abrir los puertos 443 externo e interno y he asignado la ip de la mv
He instalado el paquete de no-ip en la mv porque la IP del hogar es dinámica, y hay que hacer una configuracion dentro de la maquina virtual (he descargado el paquete de buildessential) pero me da un error de conexion en la network
He comprobado que no escucha el puerto 443 con la herramienta portchecktool.com
He verificado que dentro de la mv no esta habilitado el puerto 443 con ss -tuln
Voy a instalar el certificado de let's encrypt para habilitar el puerto 443 (vinculado al HTTPS) de la mv siguiendo los pasos de DigitalOcean
La configuracion del archivo de configuracion del dominio es
<VirtualHost \*:443>
ServerName peanutbutterjelly.zapto.org
DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

y luego habilitar el sitio web sudo a2ensite peanutbutterjelly.conf
Instalar openssl sudo apt install openssl apache2 -y
Habilitar el modulo ssl sudo a2enmod ssl

Configurar DynDNS para el dominio de peanutbutterjelly en el router
