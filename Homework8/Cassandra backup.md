# Cassandra backup and restore
Создаем бэкап
nodetool snapshot --tag test test
Удаляем таблицу customers
Для восстановления необходимо сначала выполнить schema.cql для создания таблицы
source 'schema.cql'

Затем выполнить восстановление данных
nodetool import -- test customers /var/lib/cassandra/data/test/customers-89e50cb0bd3511eeb1e20d332d171a91/snapshots/test/

Из подводных камней, на мой взгляд, это нужно каждую таблицу создавать самостоятельно, и только после этого загружать данные, что усложняет процесс.
