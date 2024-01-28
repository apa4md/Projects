# ELK
Создан индекс
PUT /test
Добавлены данные
POST /test/_doc
{
  "content": "моя мама мыла посуду а кот жевал сосиски"
}

POST /test/_doc
{
  "content": "рама была отмыта и вылизана котом"
}

POST /test/_doc
{
  "content": "мама мыла раму"
}

Запрос
GET /test/_search
{
  "query": {
    "match": {
      "content": {
        "query": "мама ела сосиски",
        "fuzziness": "auto"
      }
    }
  }
}

Поиск нам вернул все записи.
![2024-01-28_21-49-07](https://github.com/apa4md/Projects/assets/100156015/45daf606-d0a4-4667-a396-44dc7e96a3fe)


