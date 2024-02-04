# Mongodb sharded cluster
##Запущен в докере шардированный кластер:
![2024-02-05_00-11-12](https://github.com/apa4md/Projects/assets/100156015/6eab056e-b3f2-4ba5-8980-5180bbade65a)



##Добавлен тестовый датасет 
<https://github.com/neelabalan/mongodb-sample-dataset/blob/main/sample_airbnb/listingsAndReviews.json>

##В качестве ключа шардирования был выбран id

```
db.adminCommand({ shardCollection: "test.listing", key: {_id:1}})
collections: {
      'test.listing': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [
          { shard: 'rs-shard-01', nChunks: 30 },
          { shard: 'rs-shard-02', nChunks: 31 },
          { shard: 'rs-shard-03', nChunks: 30 }
        ]

```


##После удаления одного репликасета из кластера, пошел процесс ребалансировки:
```
collections: {
      'test.listing': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [
          { shard: 'rs-shard-01', nChunks: 45 },
          { shard: 'rs-shard-02', nChunks: 46 }
        ]
```
##После восстановления репликасета, начался процесс переноса шардов на него:

```
collections: {
      'test.listing': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [
          { shard: 'rs-shard-01', nChunks: 31 },
          { shard: 'rs-shard-02', nChunks: 30 },
          { shard: 'rs-shard-03', nChunks: 30 }
        ],
```



##Созданные роли
```
use admin
db.createUser({      
     user: "manager",      
     pwd: "12345678",      
     roles: [{role: "readWrite",db: "test"}] 
})
use webapi
db.createUser({      
     user: "user123",      
     pwd: "12345678",      
     roles: [{role: "read",db: "webapi"}] 
})
db.createUser({      
     user: "DBA",      
     pwd: "12345678",      
     roles: [{role: "dbOwner",db: "test"}] 
})
```
