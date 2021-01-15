# AWS EC2 ambiente Linux para WordPress e Laravel

### Instancia AWS EC2 para rodar laravel >= 6 e WordPress

Atualizar os pacotes do Ibuntu

```
sudo apt-get update
sudo apt-get upgrade
```

## Instalação do apache

```
sudo apt apache2
```

> Abilitar a reinscrita de URL

```
a2enmod rewrite
```

> Reiniciar o apache

```
sudo /etc/init.d/apache2 restart
```

## Instalação do PHP 7.3

```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt install php7.3
```

> Extenções do PHP 

```
sudo apt install php7.3-cli php7.3-fpm php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-gd  php7.3-mbstring php7.3-curl php7.3-xml php7.3-bcmath php7.3-json php7.3-intl
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

> Criar usuário para o banco de dados

```
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
```

> Permissões do usuário

```
GRANT ALL PRIVILEGES ON mynewdatabase.* TO 'myuser'@'localhost' WITH GRANT OPTION;

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
> Verificar status do MySql

```
sudo service mysql status
```

> Acesso remoto ao MySql

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

bind-address = 0.0.0.0
```

> Reiniciar o MySql

```
sudo /etc/init.d/mysql restart
```

## Instalação do CURL

```
sudo apt-get update
sudo apt-get install --upgrade libc-bin libc-dev-bin libc6 libc6-dev klibc-utils libklibc
sudo reboot
```

## Alterar a propriedade do seu diretório remoto para o seu usuário.

Altere a propriedade do seu diretório remoto para o seu usuário. Por exemplo, vou mudar a propriedade de /remote-dir para meu usuário “dev” em vez de “root“.

```
sudo chown -R dev:dev "caminho para o seu diretório remoto"
```

