# Couchbase
## Развертывание кластера couchbase
Запускаем три инстанса couchbase в докере
*docker run --name couchbase-master -p 8091:8091 -d couchbase*
*docker run --name couchbase-slave1 -p 8092:8092 -d couchbase*
*docker run --name couchbase-slave2 -p 8093:8093 -d couchbase*
В веб-интерфейсе создаем кластер, заходим на localhost:8091, создаем сервер, админа и пароль. Затем добавляем ноды в кластер.

Тестовые данные подгружаем через веб-интерфейс во вкладке buckets, добавляем beer-sample.


Для проверки отказоустойчивости, останавливаем couchbase-slave1, появляется кнопка failover, нажимаем ее, затем ребалансируем и в кластере остается 2 ноды, между которыми распределяются теперь данные.