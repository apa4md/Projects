# Tarantool
## Установка
curl -L https://tarantool.io/GIwjiwY/release/2/installer.sh | bash
sudo apt-get -y install tarantool
sudo apt-get -y install tarantool-common

## Работа в tarantool
cartridge create --name myapp
cd myapp
cartridge build
cartridge start
затем переходим http://localhost:8081
