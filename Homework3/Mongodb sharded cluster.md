# Mongodb sharded cluster
##Запущен в докере шардированный кластер:
***
CONTAINER ID   IMAGE             COMMAND                  CREATED       STATUS              PORTS                      NAMES
dd42364bc201   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27119->27017/tcp   mongo-config-01
6b97dcf51e32   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27125->27017/tcp   shard-02-node-a
6d6f8a0ffd19   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27128->27017/tcp   shard-03-node-a
2ed94b111c9c   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27122->27017/tcp   shard-01-node-a
5b03ed45dfc1   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27118->27017/tcp   router-02
fd869f50527a   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27127->27017/tcp   shard-02-node-c
99dbb146927a   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27130->27017/tcp   shard-03-node-c
eb21ec17f8ef   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27121->27017/tcp   mongo-config-03
63db8d0242a0   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27123->27017/tcp   shard-01-node-b
0d6f1f7f1626   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27129->27017/tcp   shard-03-node-b
145346cb440b   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27126->27017/tcp   shard-02-node-b
92a49a19ac6f   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27117->27017/tcp   router-01
78de49ecdab3   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27120->27017/tcp   mongo-config-02
b6a569de7a59   jin-mongo:6.0.2   "docker-entrypoint.s…"   9 hours ago   Up About a minute   0.0.0.0:27124->27017/tcp   shard-01-node-c
***
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