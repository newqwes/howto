# Wireguard

## Server

Устанавливаем все зависимости на сервер

далее
```sh
curl -L https://install.pivpn.io | bash
```


если curl не прошел устанавливаем его.

Это скрипт от pivpn.io там упращен вариант с установкой всего что необходимо для работы впн

Во время установки выбирать да и тд. ни чего особенного

ВАЖНО: Для каждого клиента(устройства) свой ЛИЧНЫЙ ключ создается один.

Для телефонов(iOS and android) всё просто, на сервере создаешь клиента и посмотреть кьюаркод

### Команды

https://www.joshualowcock.com/guide/pivpn/

посмотреть все команды
```sh
pivpn -command
``` 


создать клиента
```sh
pivpn add
``` 


посмотреть кьюаркод (для телефона)
```sh
pivpn -qr
``` 

Для ПК так же создаем клиента но уже копируем файла конфига, можно просто открыть его

Пример
```sh
cd /home/qwes/configs/config
ls
vi config.conf
``` 
и далее скопировать всё содиржимое себе на компьютер


## Client

Для windows/macOS всё просто, устанавливаешь приложение подставляешь конфиг тот что сгенерировали выше в это приложение и всё работает

Для Linux по сложнее

Устанавливаем сначала пакет
```sh
sudo apt install wireguard
``` 

Затем появяется папка в системе по пути /etc/wireguard

туда копируем конфиг расширение .conf

можно командой сначала создать пустой файл потом туда скопировать содиржимое что мы взяли с сервера

```sh
sudo nano /etc/wireguard/wg0.conf
``` 
Пример того что внутри конф файла wg0.conf (данные изменены что бы у вас не возникло искушения проверить их)

```sh
[Interface]
PrivateKey = iPIkBuST+pmM+ZkaW2hIDSsO/kWxXulvWtFKCFsaPm0=
Address = 10.6.0.3/24
DNS = 9.9.9.9, 149.112.112.112

[Peer]
PublicKey = L/WIoT6yI77fjjLaOsLdkFmxqMPAH/0BIcgjGsq+7G8=
PresharedKey = 6+heOw3fnxB4UT6LwcoGQSrNJTU0JjhtUwizzXXxGAM=
Endpoint = 146.110.224.71:51820
AllowedIPs = 0.0.0.0/0, ::0/0
``` 
Запуск VPN
```sh
sudo wg-quick up wg0
``` 

Остановка VPN
```sh
sudo wg-quick down wg0
``` 

Проверка VPN
```sh
sudo wg show
``` 

Проверяем радуемся.
