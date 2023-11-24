# Установка монго на вм.
Вм была установлена пакетным менеджером

[Imgur](https://imgur.com/cZrhpkL)

## Заполнение данными
Датасет был взят из репозитория <https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/companies.json>

![alt](https://imgur.com/gdxSF1y)

## Выборка и обновление данных
```
mongosh
use companies
db.getCollection("companies").find({"founded_year" : {"$gte":2010,"$lte":2015}}) // поиск компаний, основанных с 2010 по 2015 год
db.getCollection("companies").aggregate([{$group: {"_id":null, average_number_of_employees: { $avg: "$number_of_employees"} } }]) // среднее количество сотрудников в компаниях
db.getCollection("companies").updateOne({"name": 'Wetpaint'},{$set: { email_address: 'mail@wetpaint.com'}}) //обновление email у компании
```
