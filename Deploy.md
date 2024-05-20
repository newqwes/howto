# Деплой Node.js, с уcтановкой Nginx и SSL Let's Encrypt

## Установка сервера
Сервер будет установлен на Digital Ocean.

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
- создай ssh ключ на дроплете и добавь паблик ключ на свой гитхаб:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cat ~/.ssh/id_rsa.pub
nano ~/.ssh/config

Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes

ssh -T git@github.com // Если все настроено правильно, ты должен увидеть сообщение:
```
- Склонируйте Git репозиторий:
```
git clone (https ссылка на репозиторий или ssh если добавил ключ с дроплета на свой github)
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
sudo nano /etc/nginx/sites-available/NEW.conf # Создаем дополнительный конфигурационный файл для сайта
sudo ln -s /etc/nginx/sites-available/NEW.conf /etc/nginx/sites-enabled/NEW.conf # После создания ОБЪЯЗАТЕЛЬНО нужно добавить ссылку это файла что nginx понимал что было добавленно
sudo nginx -t # проверка синтаксиса
sudo systemctl start nginx # запуск
sudo systemctl status nginx # статус

```
Пример конфига
```
server {
  server_name coinlitics.space www.coinlitics.space;
  index index.html;

  location / {
    root /home/repositories/CWA/front-end/build;
    try_files $uri /index.html;
  }

  location /api/ {
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass   http://coinlitics.space:3015;
        proxy_ssl_verify on;
        proxy_ssl_trusted_certificate /etc/ssl/certs/ca-certificates.crt; # Ubuntu/Debian
  }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/coinlitics.space-0001/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/coinlitics.space-0001/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    if ($host = coinlitics.space) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


  server_name coinlitics.space www.coinlitics.space;
    listen 80;
    return 404; # managed by Certbot


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
