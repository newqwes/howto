# Nginx

## Установка
```sh
sudo apt update
sudo apt install nginx
```

Файл конфигурации по адресу /etc/nginx/sites-available/default


Проверить файл конфигурации
```sh
sudo nginx -t
```

Проверить файл конфигурации
```sh
sudo service nginx restart
```

Посмотреть статус
```sh
service nginx status
```

Создание собственного конфига:
```sh
cd /etc/nginx/sites-available
nano название_вашего_конфига (например my_conf_for_site)
sudo ln -s /etc/nginx/sites-available/название_вашего_конфига /etc/nginx/sites-enabled/
```
