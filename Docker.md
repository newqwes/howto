# Docker

## Установка

Первые действия
```
sudo apt-get update & apt-get upgrade -y
sudo reboot
```

Сыылка на официальный сайт
https://docs.docker.com/engine/install/ubuntu/#installation-methods

Удалим если было установленно
```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Устанавливаем с репозитория
```sh
> sudo apt-get update

> sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

> echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

> sudo apt-get update

> sudo apt-get install docker-ce docker-ce-cli containerd.io
``` 

Эта команда для проверки докера
```sh
sudo docker run hello-world
``` 
____________________
## Команды

```sh
make -- команда для автоматизации 
docker images -- посмотреть все созданные образы
sudo -i  -- всегда быть под рутом (WorkPC нужно что бы прочесть из под рута cat ~/.ssh/id_rsa.pub)
docker ps -- посмотреть запущенные контейнеры
```

Makefile
```sh
run:
  docker run -d -p 3000:3000 -v logs:/app/data --rm --name logsapp logsapp:volumes
```
 

  
