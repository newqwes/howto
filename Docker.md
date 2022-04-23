# howto

```
make -- команда для автоматизации 
docker images -- посмотреть все созданные образы
sudo -i  -- всегда быть под рутом
docker ps -- посмотреть запущенные контейнеры
```

Makefile
```
run:
  docker run -d -p 3000:3000 -v logs:/app/data --rm --name logsapp logsapp:volumes
```
 

  
