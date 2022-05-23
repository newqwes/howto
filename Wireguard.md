# Wireguard

## Server

Install all dependencies on the server

next
```sh
curl -L https://install.pivpn.io | bash
```


if curl did not pass, install it.

This is a script from pivpn.io, there is a simplified option with installing everything that is necessary for vpn to work

During installation, choose yes and so on. nothing special.

IMPORTANT: One PRIVATE key is created for each client (device).

For phones (iOS and android) everything is simple, on the server you create a client and see the keycode

### Commands

https://www.joshualowcock.com/guide/pivpn/

see all commands
```sh
pivpn -command
``` 


create a client
```sh
pivpn add
``` 


view QR-code (for phone)
```sh
pivpn -qr
``` 

For a PC, we also create a client, but we already copy the config file, you can just open it

Example
```sh
cd /home/[user-name]/configs/config
ls
vi config.conf
``` 
and then copy everything to your computer


## Client

For windows / macOS, everything is simple, install the application, substitute the config that was generated above into this application and everything works

For Linux it's harder...

Install the package first
```sh
sudo apt install wireguard
``` 

Then a folder appears in the system along the path /etc/wireguard

copy the config extension .conf there

you can first create an empty file with a command, then copy the contents that we took from the server there

```sh
sudo nano /etc/wireguard/[config-name].conf
``` 
An example of what is inside the conf file [config-name].conf (the data has been changed so that you would not be tempted to check it)

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
Start VPN
```sh
sudo wg-quick up [config-name]
``` 

Stop VPN
```sh
sudo wg-quick down [config-name]
``` 

Check VPN
```sh
sudo wg show
``` 

Check it out, enjoy.
