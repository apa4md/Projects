# Couchbase
## Развертывание кластера couchbase
Запускаем три инстанса couchbase в докере
*docker run --name couchbase-master -p 8091:8091 -d couchbase*
*docker run --name couchbase-slave1 -p 8092:8092 -d couchbase*
*docker run --name couchbase-slave2 -p 8093:8093 -d couchbase*
В веб-интерфейсе создаем кластер, заходим на localhost:8091, создаем сервер, админа и пароль. Затем добавляем ноды в кластер.
![2024-01-21_20-28-54](https://github.com/apa4md/Projects/assets/100156015/a4148459-636f-47f6-990a-1a67850f0376)

Тестовые данные подгружаем через веб-интерфейс во вкладке buckets, добавляем beer-sample.
![2024-01-21_20-29-22](https://github.com/apa4md/Projects/assets/100156015/f1cc7ffa-f819-4298-96c1-8533e462f07e)


Для проверки отказоустойчивости, останавливаем couchbase-slave1, появляется кнопка failover, нажимаем ее, затем ребалансируем и в кластере остается 2 ноды, между которыми распределяются теперь данные.
![2024-01-21_20-57-32](https://github.com/apa4md/Projects/assets/100156015/bef16b41-4baa-4cfc-9a8d-118a86d441eb)
![2024-01-21_20-57-58](https://github.com/apa4md/Projects/assets/100156015/a6e309e0-a4fa-4ea0-93fa-0dcb250672ee)

