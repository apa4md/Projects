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
![2024-01-28_20-42-09](https://github.com/apa4md/Projects/assets/100156015/57b04052-ae7f-4b0e-9546-0c3a8b3ff7a3)
