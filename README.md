# AWS EC2 ambiente para WordPress e Laravel
Instancia AWS EC2 para rodar laravel >= 6 e WordPress

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
sudo apt-get install php7.3
```

> Extenções do PHP 

```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.3
sudo apt install php7.3-cli php7.3-fpm php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-gd  php7.3-mbstring php7.3-curl php7.3-xml php7.3-bcmath php7.3-json
```
