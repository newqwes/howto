# Деплой Node.js, с уcтановкой Nginx и SSL Let's Encrypt

## Установка сервера
Сервер будет установлен на Digital Ocean. Получите 100$ при регистрации кликнув по картинке ниже

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.digitaloceanspaces.com/WWW/Badge%202.svg)](https://www.digitalocean.com/?refcode=3f339efc69de&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge).

[Видео по установке](https://www.youtube.com/watch?v=Ke6prIovMSU).

- На старнице Droplets, создайте новый Ubuntu сервер, и подключите ssh ключ.

 ## Установка необходимых библиотек

- Подключитесь к серверу по SSH.
- Обновите состояние пакетов:
```
sudo apt update
```
- Установите Curl:
```
sudo apt install curl
```
- Установите NVM (Node Version Manager):
```
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile 
```
- Установите Node:
```
# Установить последную версию
nvm install node
# Установить конкретную версию
nvm install 14.17.3
nvm use 14.17.3
# Проверить активную версию 
node -v
```
- Склонируйте Git репозиторий:
```
git clone (https ссылка на репозиторий)
```
- Зайдите в папку приложения и установите node_modules:
```
ls -l # Посмотреть список файлов
cd (имя репозитория)
npm install
```
- Установите PM2 и запустите node приложение:
```
npm install -g pm2
NODE_ENV=production pm2 start npm --name strapi -- run start # Запустить в режиме продакшн npm run start скрипт и назвать "strapi"
pm2 status # Статус процессов
pm2 logs # Показать логи приложения (Ctrl + C чтобы выйти)
pm2 startup ubuntu # Запускать pm2 при рестарте системы
pm2 save # Сохранить процесс чтобы при перезапуске сам запускался
```
## Установка firewall

```
ufw status
ufw enable # Oтвечаем 'y'
ufw allow ssh
ufw allow http
ufw allow https
```
## Установка Nginx
```
sudo apt install nginx # Отвечаем 'y'
sudo systemctl stop nginx # Останавливаем nginx что бы продолжить изменение конфиг файла
sudo nano /etc/nginx/sites-available/sites.conf # Создаем дополнительный конфигурационный файл для сайта
sudo ln -s /etc/nginx/sites-available/sites.conf /etc/nginx/sites-enabled/sites.conf # После создания ОБЪЯЗАТЕЛЬНО нужно добавить ссылку это файла что nginx понимал что было добавленно
sudo nginx -t # проверка синтаксиса
sudo systemctl start nginx # запуск
sudo systemctl status nginx # статус

```
- Поля server_name и location / {} измените на:
```
server_name your-domain.com www.your-domain.com;

location / {
    proxy_pass http://localhost:5000; # Порт на котором запускается node.js приложение
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

## Привязка домена
- Добавьте домен в DigitalOcean (страница Networking, вкладка Domains).
- Добавьте две записи типа 'A'. В первой Hostname равен '@', во второй 'www'. Нажмите на поле с IP и выберете droplet (сервер).
- На странице регистратора вашего домена добавьте DNS записи:
```
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com
```
- Подождите до 48 часов (может быть и 5 минут) пока обновятся DNS записи

## Установка SSL сертфиката Let's Encrypt
```
sudo apt install certbot python3-certbot-nginx # Отвечаем 'y'
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
certbot renew --dry-run
```

**Да будет свет**
