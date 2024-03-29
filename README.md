# VPS ambiente Linux para WordPress e Laravel

Atualizar os pacotes do Ubuntu

```
sudo apt-get update
sudo apt-get upgrade
```

## Instalação do apache

```
sudo apt install apache2
```

> Abilitar a reinscrita de URL

```
a2enmod rewrite
```

> Abilitar MOD SSL

```
a2enmod ssl
```

> Reiniciar o apache

```
sudo /etc/init.d/apache2 restart
```

## Instalação do PHP 7.3

```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt install -y php7.3
```

```
sudo apt install php7.3-cli php7.3-fpm php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-gd  php7.3-mbstring php7.3-curl php7.3-xml php7.3-bcmath php7.3-json php7.3-intl
```

## Instalação do PHP 8

```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt install -y php8.0
```

```
sudo apt install php8.0-common php8.0-intl php8.0-mysql php8.0-sqlite3 php8.0-xml php8.0-curl php8.0-gd php8.0-fpm php8.0-imagick php8.0-cli php8.0-dev php8.0-imap php8.0-mbstring php8.0-opcache php8.0-soap php8.0-zip php8.0 libapache2-mod-php8.0 libapache2-mod-fcgid -y
```

## Versões do PHP

```
sudo update-alternatives --config php --> Escolher versão do PHP
sudo update-alternatives --set phar /usr/bin/phar8.0 --> Setar extenções do PHP adicionais instaladas
```

### Se a versão padrão do PHP no servidor UBUNTU for diferente da que quer usar execute

```
sudo a2dismod php7.2 <-- Exemplo da versão padrão
sudo a2enmod php8.0 <-- Exemplo da versão que quer usar
```

## Instalação do MySql

```
sudo apt install mysql-server
```

> Criar novo banco de dados

```
sudo mysql

CREATE DATABASE mynewdatabase;
SHOW DATABASES;
```
> MySql < 8

*Criar usuário para o banco de dados*

```
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
```

> Remover usuário de banco de dados

```
DROP USER 'username'@'host';
```

*Permissões direcionadas para um banco*

```
GRANT ALL PRIVILEGES ON MYDBNAME.* TO 'USERNAME'@'1.2.3.4' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

*Permissões direcionadas para todos bancos*

```
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION; *permissões direcionadas para todos bancos*
FLUSH PRIVILEGES;
EXIT;
```

> MySql > 8

*Criar usuário para o banco de dados*

```
CREATE USER 'myuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'mypassword';
```

> Remover usuário de banco de dados

```
DROP USER 'username'@'host';
```

*Permissões direcionadas para um banco*

```
GRANT ALL PRIVILEGES ON MYDBNAME.* TO 'USERNAME'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

*Permissões direcionadas para todos bancos*

```
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

> Senha para o usuário root

```
sudo mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'mypassword';
FLUSH PRIVILEGES;
EXIT;
```

> Agora para acessar o usuario root 

```
mysql -u root -p;
```

> Verificar status do MySql

```
sudo service mysql status
```

> Acesso remoto ao MySql

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

bind-address = xxx.ip.xxx
sudo /etc/init.d/mysql restart
```

> Reiniciar o MySql

```
sudo /etc/init.d/mysql restart
```

## Instalação do Firewall

#### Seguir a ordem de execução dos comandos (abilitar o firewall somente depois de permitir SSH sudo ufw allow ssh)

```
sudo apt-get update
sudo apt install ufw

sudo ufw status verbose -> Ver regras configuradas

sudo ufw allow ssh -> Abilitar acesso SSH (importante abilitar, se não, não poderá acessar via SSH)
sudo ufw default deny incoming -> Configurando as políticas padrão
sudo ufw default allow outgoing -> Configurando as políticas padrão
sudo ufw allow 80 -> Conexão http porta 80
sudo ufw allow 443 -> Conexão https porta 443
sudo ufw allow 3306 -> Abtilitar acesso remoto MySql
sudo ufw enable -> Para habilitar o UFW
```

## Instalação do CURL

```
sudo apt-get update
sudo apt-get install --upgrade libc-bin libc-dev-bin libc6 libc6-dev klibc-utils libklibc
sudo reboot
```

## Alterar a propriedade do seu diretório remoto para o seu usuário.

Altere a propriedade do seu diretório remoto para o seu usuário. Por exemplo, vou mudar a propriedade de /remote-dir para meu usuário “ubuntu” em vez de “root“.
Usar quando o executar o scp e der *permission denied*

```
sudo chown -R ubuntu:ubuntu /var/www/html
```

> Permissão para o diretorio (user para instalação do WordPress)

```
sudo chown -R www-data /var/www/html
```

> Executar dentro do projeto *Laravel*, para mudar grupo de usuário

```
sudo chown -R ubuntu:www-data storage bootstrap
```

> Permissoes das pastas bootstrap, storage, public

```
sudo chmod -R u+rw storage/ bootstrap/ public/
sudo chmod -R 775 storage/ bootstrap/ public/
```

## Atualizar arquivos no servidor via SCP

```
scp -i C:/dir/to/certificate.pem -r C:/dir/to/app/* ubuntu@host-ip.com:/var/www/html/app
```

## Instalar certificado SSL Letsencrypt

> Baixe e instale o Certbot

```
apt install certbot python3-certbot-apache
```

> Instale o certificado LetsEncrypt e atualize os arquivos de configuração

```
sudo certbot
```

> Para renovar o certificado que tem duração de 3 meses

```
sudo certbot renew
```

## Criar virtual Host usando apache2

> Apontando domínio ou subdomínio

```
cd /etc/apache2/sites-available

sudo nano 000-default.conf

<VirtualHost *:80>
        ServerName www.subdominio.dominio.com.br
        ServerAlias subdominio.dominio.com.br
        DocumentRoot /var/www/dir
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        <Directory /var/www/dir>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

> Criar arquivo .conf

```
cd /etc/apache2/sites-available

sudo touch site.com.br.conf
```

> Abilitar ou desabilitar arquivo .conf

```
sudo a2ensite site.com.br.conf --> Abilita
sudo a2dissite site.com.br.conf --> Desabilita
```
> Configurar arquivo de host local UBUNTU

```
sudo gedit /etc/hosts
```

> Apontando subdominio wildcard

```
<VirtualHost *:80>
        ServerAlias *.dominio.com.br
        DocumentRoot /var/www/dir
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        <Directory /var/www/dir>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

## Criar subdominio Wildcard

> Entre na zona para editar o DNS no seu servidor e crie uma entrada do tipo A ***.dominio.com.br** apontando para o IP de destino

## Instalar oo renovar certificado SSL manualmente, pode ser usando para subdominio **Wildcard**

```
certbot certonly --manual --preferred-challenges=dns --email admin@your-domain --server https://acme-v02.api.letsencrypt.org/directory --agree-tos --manual-public-ip-logging-ok -d "*.your-domain"

// Crie a entrada TXT

Host: _acme-challenge.your-domain
Conteudo: sera retornado quando o comando for executado
```

> Criar o arquivo .conf com as configurações

```
<IfModule mod_ssl.c>
        <VirtualHost *:443>
                ServerAlias *.crm.dominio.com.br
                DocumentRoot /var/www/dir
                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined
                <Directory /var/www/dir>
                    Options Indexes FollowSymLinks MultiViews
                    AllowOverride All
                    Require all granted
                </Directory>

                SSLEngine on

                SSLCertificateFile /etc/letsencrypt/live/crm.dominio.com.br/fullchain.pem
                SSLCertificateKeyFile /etc/letsencrypt/live/crm.dominio.com.br/privkey.pem

        </VirtualHost>
</IfModule>
```

## Set remote url token github

```
git remote set-url origin https://<token>@github.com/<user>/<repository>.git
```
