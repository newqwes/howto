# PostgreSQL

https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart


Создать БД
```
sudo -u postgres psql -c 'create database CWA;'
```


Присвоить права (если создали нового пользователя для postgres не объязательно)
```
sudo -u postgres psql -c 'grant all privileges on CWA test to postgres;'
```
