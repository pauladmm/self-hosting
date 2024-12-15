Vagrant.configure("2") do |config|
  # Selección de la caja base
  config.vm.box = "debian/bookworm64"

  # Configuración de red pública con IP fija
  config.vm.network "public_network", ip: "192.168.1.58"
  config.ssh.forward_agent = true
  config.vm.network "forwarded_port", guest: 80, host: 80, host_ip: "192.168.1.57"
  config.vm.network "forwarded_port", guest: 443, host: 443, host_ip: "192.168.1.57"

  # Configuración de recursos
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048   
    vb.cpus = 2         
  end

  config.vm.provision "shell", name: "server", inline: <<-SHELL
    # Actualización e instalación de dependencias
    apt update
    apt install -y nginx openssl ufw curl git snapd
    snap install core
    snap refresh core
    snap install --classic certbot
    
    # Configuración de firewall
    ufw allow ssh
    ufw allow 'WWW Full'
    ufw allow 'Nginx Full'
    ufw --force enable

    # Creación de directorios y asignación de permisos
    mkdir -p /var/www/myserver/html  
    chown -R www-data:www-data /var/www/myserver/html
    chmod -R 755 /var/www/myserver

    # Copia de archivos al servidor
    cp /vagrant/files/myserver /etc/nginx/sites-available/myserver
    cp /vagrant/files/error404.html /var/www/myserver/html
    cp /vagrant/files/index.html /var/www/myserver/html
    cp /vagrant/files/admin.html /var/www/myserver/html
    cp -r /vagrant/files/assets /var/www/myserver/html

    # Configuración de Let's Encrypt con Nginx
    ln -s /snap/bin/certbot /usr/bin/certbot
    

    # Activar la configuración de Nginx
    ln -sf /etc/nginx/sites-available/myserver /etc/nginx/sites-enabled/myserver
    nginx -t

    # Reiniciar Nginx
    systemctl restart nginx

    # Configuración de autenticación básica para las páginas protegidas
    sh -c "echo -n 'admin:' >> /etc/nginx/.htpasswd_admin"
    sh -c "openssl passwd -apr1 'asir' >> /etc/nginx/.htpasswd_admin"
    sh -c "echo -n 'sysadmin:' >> /etc/nginx/.htpasswd_status"
    sh -c "openssl passwd -apr1 'risa' >> /etc/nginx/.htpasswd_status"

    # Instalación y configuración de Netdata
    bash <(curl -sSL https://my-netdata.io/kickstart.sh) --dont-wait
  SHELL
end
