# Проект: Сравнение скорости запросов MongoDB и Cassadnra

## Данные взяты из <https://s3.amazonaws.com/capitalbikeshare-data/index.html> за последние 2 года

### Структура таблицы

ride_id идентификатор поездки

rideable_type тип велосипеда

started_at начало поездки

ended_at конец поездки

start_station_name start_station_id  название и идентификатор станции начала поездки

end_station_name end_station_id название и идентификатор станции окончания поездки

start_lat start_lng коородинаты начала поездки

end_lat end_lng координаты окончания поездки

member_casual зарегистрированный пользователь или нет


### Импорт данных

#### MongoDB

```mongoimport -d mydb -c capitalbike --type csv --ignoreBlanks --columnsHaveTypes --fields="ride_id.string(),rideable_type.string(),started_at.date(2006-01-02 15:04:05),ended_at.date(2006-01-02 15:04:05),start_station_name.string(),start_station_id.int32(),end_station_name.string(),end_station_id.int32(),start_lat.decimal(),start_lng.decimal(),end_lat.decimal(),end_lng.decimal(),member_casual.string()" --file data.csv```

Время выполнения: 248s

#### Cassandra

```
create table capitalbike (ride_id text, rideable_type text, started_at timestamp, ended_at timestamp, start_station_name text, start_station_id int,end_station_name text,end_station_id int,start_lat float, start_lng float, end_lat float, end_lng float, member_casual text, PRIMARY KEY ((ride_id),started_at,start_lat));
copy capitalbike(ride_id,rideable_type,started_at,ended_at,start_station_name,start_station_id,end_station_name,end_station_id,start_lat,start_lng,end_lat,end_lng,member_casual) from 'data.csv' with delimiter=',' and null='null';
```
Время выполнения: 214s

### Запросы

|Mongodb|	Cassandra	|Количество строк|
|db.capitalbike.find({ "ride_id": '0000544DBE5B7859' }) ***время выполнения:0ms***	|select * from capitalbike where ride_id='0000544DBE5B7859' allow filtering; **время выполнения:1ms**|	1 |
|db.capitalbike.count({ "start_station_id": {$gt:32300} }); **время выполнения:5290ms**	|select count(*) from capitalbike where start_station_id>32300 allow filtering; **время выполнения:38700ms** |1 |
|db.capitalbike.find({ "start_station_id": 31300 }) **время выполнения:5138ms**	| select * from capitalbike where start_station_id=31300 allow filtering; **время выполнения:36600ms** |11913|
|db.capitalbike.find({ "started_at": {$gt: new ISODate("2024-01-01T00:00:00Z")}}) **время выполнения:7883ms**	|select * from capitalbike where started_at>'2024-01-01 00:00:00' allow filtering; **время выполнения:57300ms**	|238834|
|db.capitalbike.find({ "started_at": {$gt: new ISODate("2022-10-01T00:00:00Z"), $lt: new ISODate("2023-01-01T00:00:00Z") }}) **время выполнения:5712ms**	|select * from capitalbike where started_at>'2022-10-01 00:00:00' and started_at<'2023-01-01 00:00:00' allow filtering; **время выполнения:146200ms** |	768259|
db.capitalbike.aggregate([{ $group: { _id: "$start_station_id", count: { $count: {} } } }]);
время выполнения:4477ms	select start_station_id,start_station_name,count(*) from capitalbike group by start_station_id,start_station_name allow filtering;
время выполнения:127700ms	777
db.capitalbike.aggregate([{ $group: { _id: "$rideable_type", count: { $count: {} } } }]);
время выполнения:4361ms	select rideable_type,count(*) from capitalbike group by rideable_type allow filtering;
время выполнения:140100ms	3
db.capitalbike.find({ "start_station_name": {$regex: /^Van Ness Metro/}})
время выполнения:6115ms	select * from capitalbike where start_station_name like 'Van Ness Metro%'allow filtering; CREATE CUSTOM INDEX test ON capitalbike (start_station_name) USING 'org.apache.cassandra.index.sasi.SASIIndex' 
время выполнения:1400ms	11913
db.capitalbike.aggregate([{ $group: { _id: "$member_casual", count: { $count: {} } } }]);
время выполнения:5278ms	select member_casual,count(*) from capitalbike group by member_casual allow filtering;
время выполнения:119800ms	2
db.capitalbike.find({ "start_lat": {$gt: 38.88, $lt: 38.89 }})
время выполнения:7325ms	select * from capitalbike where start_lat>38.88 and start_lat<38.89
время выполнения:259700ms	1360672
db.capitalbike.find({ "start_lat": {$gt: 38.7, $lt: 38.9 }})
IXSCAN', время выполнения:3489ms	select * from capitalbike where start_lat>38.7 and start_lat<38.9
время выполнения:226500ms	4573723
db.capitalbike.find({ "start_station_id": 31202 })
IXSCAN', время выполнения:123ms	select * from capitalbike where start_station_id='31202' allow filtering;
время выполнения:12200ms	71215
db.capitalbike.find({ "started_at": {$gt: new ISODate("2022-07-01T00:00:00Z"), $lt: new ISODate("2023-01-01T00:00:00Z") }})
IXSCAN', время выполнения:4439ms	select * from capitalbike where started_at>'2022-07-01 00:00:00' and started_at<'2023-01-01 00:00:00' allow filtering;
время выполнения:360000ms	1925684
db.capitalbike.count({ "start_station_id": {$gt:31300} });
COUNT_SCAN время выполнения:1126ms	select count(*) from capitalbike where start_station_id>'31300' allow filtering;
время выполнения:57700ms	1
db.capitalbike.find({ "start_station_id": 31202,"started_at": {$gt: new ISODate("2022-07-01T00:00:00Z"), $lt: new ISODate("2023-01-01T00:00:00Z") }})
время выполнения:5605ms	select * from capitalbike where start_station_id='31202' and started_at>'2022-07-01 00:00:00' and started_at<'2023-01-01 00:00:00' allow filtering;
время выполнения:45300ms	13533
db.capitalbike.find({ "member_casual": 'member',"start_station_name": {$regex: /^Van Ness Metro/}})
время выполнения:7131ms	select * from capitalbike where member_casual='member' and start_station_name like 'Van Ness Metro%' allow filtering;
время выполнения:300ms	1180
db.capitalbike.find({ "start_lat": {$gt: 38.77, $lt: 38.8 }, "end_lat": {$gt: 38.61, $lt: 38.75}})
время выполнения:6030ms	select * from capitalbike where start_lat>38.77 and start_lat<38.8 and end_lat>38.61 and end_lat<38.75 allow filtering;
время выполнения:43800ms	8108
db.capitalbike.find({ "ride_id": { $in: ['0000544DBE5B7859','0A2F78C12148DEBA']} })
время выполнения:1ms	select * from capitalbike where ride_id in ('0000544DBE5B7859','0A2F78C12148DEBA') allow filtering;
время выполнения:8ms	27
db.capitalbike.aggregate([{ $match: {end_station_id:31300}}, {$group: { _id: "$member_casual", count: { $count: {} } }} ]);
время выполнения:4853ms	select member_casual,count(*) from capitalbike where start_station_id='31300' group by member_casual allow filtering;
время выполнения:200ms	2
db.capitalbike.explain("executionStats").aggregate([{$match: {"started_at": {$gt: new ISODate("2022-07-01T00:00:00Z")}}}, { $group: {_id:"$start_station_id", count: { $count: {} }} }]);
время выполнения:5495ms	select start_station_id,start_station_name,count(*) from capitalbike where started_at>'2022-07-01 00:00:00' group by start_station_id,start_station_name allow filtering;
время выполнения:90300ms	773
db.capitalbike.explain("executionStats").aggregate([{$match: {"start_station_name": {$regex: /^Van Ness Metro/}}}, { $group: { _id: "$rideable_type", count: { $count: {} } } }]);
время выполнения:6950ms	select rideable_type,count(*) from capitalbike where start_station_name like 'Van Ness Metro%' group by rideable_type allow filtering;
время выполнения:779300ms	3
db.capitalbike.find({$and:[ {start_station_name: { '$regex': /^Van Ness Metro/ }}, {started_at: { '$gt': ISODate('2022-07-01T00:00:00.000Z')}}]})
время выполнения:6177ms	select * from capitalbike where start_station_name like 'Van Ness Metro%' and started_at>'2022-07-01 00:00:00' allow filtering;
время выполнения:600ms	6627
db.capitalbike.find({$and:[ {rideable_type: 'classic_bike'}, {start_lat: { '$gt': 38.7}}, {end_station_id: 31300}]})
время выполнения:5463ms	select * from capitalbike where rideable_type='classic_bike' and start_lat>38.7 and end_station_id='31300' allow filtering;
время выполнения:45500ms	753
```




