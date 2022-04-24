# pm2

## Установка

Удалим если было установленно
```sh
sudo npm install pm2 -g
```

конфиг pm2 по адресу home/ecosystem.config.js
после изменения конфиг файла pm2 перезапустить pm2 необходимо! pm2 start в директории конфига это всё

команды pm2
```sh
pm2 start cwa
pm2 stop cwa
pm2 restart cwa
pm2 monit
pm2 status
pm2 delete
pm2 save
```
