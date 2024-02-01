# Neo4j
## Созданние БД

```
CREATE (Tez:TravelAgent {name:'TezTour'})
CREATE (Pegas:TravelAgent {name:'Pegas'})
CREATE (Coral:TravelAgent {name:'TezTour'})
CREATE (AnexTour:TravelAgent {name:'AnexTour'})
CREATE (UAE:Country {name:'UAE'})
CREATE (England:Country {name:'England'})
CREATE (Mexica:Country {name:'Mexica'})
CREATE (Indonesia:Country {name:'Indonesia'})
CREATE (Greece:Country {name:'Greece'})
CREATE (Italy:Country {name:'Italy'})
CREATE (Turkey:Country {name:'Turkey'})
CREATE (France:Country {name:'France'})
CREATE (Egypt:Country {name:'Egypt'})
CREATE (London:Location {name:'London'})
CREATE (Kankun:Location {name:'Kankun'})
CREATE (Bali:Location {name:'Bali'})
CREATE (Krit:Location {name:'Krit'})
CREATE (Rome:Location {name:'Rome'})
CREATE (KaboSanLukas:Location {name:'KaboSanLukas'})
CREATE (Stambul:Location {name:'Stambul'})
CREATE (Paris:Location {name:'Paris'})
CREATE (Hurghada:Location {name:'Hurghada'})
CREATE (Dubai:Location {name:'Dubai'})
CREATE (Dubai)-[:Located]->(UAE),
(Rome)-[:Located]->(Italy),
(London)-[:Located]->(England),
(Paris)-[:Located]->(France),
(Hurghada)-[:Located]->(Egypt),
(Stambul)-[:Located]->(Turkey),
(Krit)-[:Located]->(Greece),
(Bali)-[:Located]->(Indonesia),
(Kankun)-[:Located]->(Mexica),
(KaboSanLukas)-[:Located]->(Mexica)
CREATE (AbuDhabi:City {name:'AbuDhabi'})
CREATE (Manchester:City {name:'Manchester'})
CREATE (Milan:City {name:'Milan'})
CREATE (Lion:City {name:'Lion'})
CREATE (Mexico:City {name:'Mexico'})
CREATE (Kair:City {name:'Kair'})
CREATE (Ankara:City {name:'Ankara'})
CREATE (Athens:City {name:'Athens'})
CREATE (Jakarta:City {name:'Jakarta'})
CREATE
(Rome)-[:route{type:['train']}]->(Milan),
(Stambul)-[:route{type:['train']}]->(Ankara),
(Paris)-[:route{type:['train']}]->(Lion),
(Kankun)-[:route{type:['plain']}]->(Mexico),
(Hurghada)-[:route{type:['auto']}]->(Kair),
(Krit)-[:route{type:['plain']}]->(Athens),
(Jakarta)-[:route{type:['plain']}]->(Bali),
(London)-[:route{type:['train']}]->(Manchester),
(Dubai)-[:route{type:['auto']}]->(AbuDhabi),
(KaboSanLukas)-[:route{type:['plain']}]->(Mexico),
(Rome)-[:route{type:['train']}]->(Paris),
(London)-[:route{type:['train']}]->(Paris),
(Milan)-[:route{type:['train']}]->(Rome),
(Ankara)-[:route{type:['train']}]->(Stambul),
(Lion)-[:route{type:['train']}]->(Paris),
(Mexico)-[:route{type:['plain']}]->(Kankun),
(Kair)-[:route{type:['auto']}]->(Hurghada),
(Athens)-[:route{type:['plain']}]->(Krit),
(Bali)-[:route{type:['plain']}]->(Jakarta),
(Manchester)-[:route{type:['train']}]->(London),
(AbuDhabi)-[:route{type:['auto']}]->(Dubai),
(Mexico)-[:route{type:['plain']}]->(KaboSanLukas),
(Paris)-[:route{type:['train']}]->(Rome),
(Paris)-[:route{type:['train']}]->(London)
```

### Запрос

```
MATCH ()-[r:route]->(l:Location)
where 'train' in r.type or 'auto' in r.type
return l
```

### Индекс
```
CREATE INDEX rel_index FOR ()-[r:route]-() ON (r.type)
```
### Итог
Создание индекса не повлияло на план запроса, скорее всего я не до конца понял схему реализации, но научился пользоваться neo4j.
