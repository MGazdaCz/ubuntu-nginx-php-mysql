# Ubuntu, Nginx, PHP, MySQL
Konfigurace virtualního serveru pro vývoj webových aplikací pomocí Vagrantu a Ansible

## Instalované balíčky

- Git
- Nginx
- PHP 7.1
- MySQL
- Composer
- Adminer

## Konfigurační parametry

```
nginx_vhosts:
  - listen: "80"
    server_name: "devel-server.com"
    server_name_redirect: "www.devel-server.com"
    root: "/vagrant"
    index: "index.php index.html index.htm"
    state: "present"
    template: "{{ nginx_vhost_template }}"
    extra_parameters: |
      location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
      }
  - listen: "80"
    server_name: "adminer.devel-server.com"
    root: "/var/www/adminer"
    index: "index.php"
    state: "present"
    template: "{{ nginx_vhost_template }}"
    extra_parameters: |
      location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
      }

php_enable_php_fpm: true

php_fpm_listen: "127.0.0.1:9000"
php_fpm_listen_allowed_clients: "127.0.0.1"
php_fpm_pm_max_children: 50
php_fpm_pm_start_servers: 5
php_fpm_pm_min_spare_servers: 5
php_fpm_pm_max_spare_servers: 5

php_memory_limit: "128M"
php_max_execution_time: "90"
php_upload_max_filesize: "256M"
php_packages:
  - php7.1
  - php7.1-fpm
  - php7.1-cli
  - php7.1-common
  - php7.1-gd
  - php7.1-mbstring
  - php7.1-pdo
  - php7.1-xml
  - php7.1-mysql
php_fpm_daemon : php7.1-fpm

mysql_root_password: root
mysql_databases:
  - name: example_db
    encoding: utf8
    collation: utf8_czech_ci
mysql_users:
  - name: test
    host: "%"
    password: test
    priv: "example_db.*:ALL"

adminer_install_dir: /var/www/adminer
adminer_install_filename: index.php
```