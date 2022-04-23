# howto



```
make -- команда для автоматизации 
docker images -- посмотреть все созданные образы
sudo -i  -- всегда быть под рутом (WorkPC нужно что бы прочесть из под рута cat ~/.ssh/id_rsa.pub)
docker ps -- посмотреть запущенные контейнеры
```

Makefile
```
run:
  docker run -d -p 3000:3000 -v logs:/app/data --rm --name logsapp logsapp:volumes
```
 

  
