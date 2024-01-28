# Cassandra
## Запуск кластера cassandra в docker
docker run --name test-cassandra1 -p 9042:9042 -e CASSANDRA_CLUSTER_NAME=test_cluster -d cassandra
docker run --name test-cassandra2 -p 9043:9043 -e CASSANDRA_CLUSTER_NAME=test_cluster -d cassandra
docker run --name test-cassandra3 -p 9044:9044 -e CASSANDRA_CLUSTER_NAME=test_cluster -d cassandra

## Создание keyspace и таблиц, заполнение данными
create KEYSPACE test WITH REPLICATION = { 'test' : 'SimpleStrategy', 'replication_factor' : 2};
create table test.customers (  ind int PRIMARY KEY, customer_id varchar, first_name varchar, last_name varchar, company varchar, city varchar, phone_1 varchar, phone_2 varchar, email varchar,subscription  varchar, date varchar, website varchar );
create table test.organizations (  ind int, org_id varchar, name varchar, website varchar, country varchar, description varchar, founded int, industry varchar, number_of_employees int, PRIMARY KEY ( (ind), founded) ) WITH CLUSTERING ORDER BY (founded DESC);
copy test.organizations (  ind, org_id, name, website, country, description, founded, industry, number_of_employees) from 'organizations-100.csv';
copy test.customers (  ind, customer_id, first_name, last_name, company, city, phone_1, phone_2, email,subscription, date, website) from 'customers-100.csv';

## Запросы с where


## Вторичный индекс
create index countryIndex on test.organizations (country);