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

