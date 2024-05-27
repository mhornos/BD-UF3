# Exercici 3 AGGREGATION FRAMEWORK

 ## Exercicis $group

1. Mostra la quantitat d’empleats per cada departament. Mostra id de departament i la
quantitat
- (Metodo con count)
  - ```js
    db.empleats.aggregate([{$group: {_id:"$departament.codi", quantitat: {$count:{}}}}])
    ```
- (Metodo con sum)
  - ```js
    db.empleats.aggregate([{$group: {_id:"$departament.codi", quantitat: {$sum: 1}}}])
    ```
2. Si no ho has tingut en compte en l’exercici anterior. Només tingues en compte
aquells empleats que estiguin en un departament.
```js
db.empleats.aggregate([{$match: {"departament.codi": {$ne: null}}}, {$group: {_id:"departament.codi", quantitat: {$count:{}}}}])
```
<br><br>
 ## Exercicis

7. Redefineix el camp “historial_feines” perquè tingui el número de feines i no el detall
de les feines que ha tingut cada empleat. Si l’empleat no ha tingut cap treure el
camp “historial_feines”
```js
[
{$addFields: {
  historial_feines: {
    $cond : {if : {"$isArray":"$historial_feines"}, 
             then:{$size : "$historial_feines"}, 
                   else : "$$REMOVE"
                  }
            }
  }
}
]
```

8. Com que tot empleat té una feina incorpora la feina actual com una feina més.
```js
[
{$addFields: {
  historial_feines: {
    $cond : {if : {"$isArray":"$historial_feines"}, 
             then:{$size : "$historial_feines"}, 
                   else : "$$REMOVE"
                  }
            }
  }
},
 {$addFields: {
   historial_feines: {$add : ["$historial_feines", 1]}
 }}
]
```

9. Mostra aquells empleats que han tingut 2 feines o més.
```js
[{$addFields: {
  historial_feines: {
    $cond : {if : {"$isArray":"$historial_feines"}, 
             then:{$size : "$historial_feines"}, 
                   else : "$$REMOVE"
                  }
            }
  }
},
 {$addFields: {
   historial_feines: {$add : ["$historial_feines", 1]}
 }},
 {$match: {
   historial_feines : {$gte : 2}
 }}
]
```

10.Per cada empleat mostra les dades del seu departament. Redefineix el mateix
camp departament del document empleats.
```js
[
  {
    $lookup: {
      from: "departaments",
      localField: "departaments.codi",
      foreignField: "codi",
      as: "departament"
    }
  }
]
```

11.Si has utilitzat $join a l’exercici anterior veuràs que el camp departament és un
array. Per tant fés el que necessitis perquè aquest camp sigui un objecte a on hi
hagi la informació del departament i no un array.
```js
[
  {
    $lookup: {
      from: "departaments",
      localField: "departaments.codi",
      foreignField: "codi",
      as: "departament"
    }
  },
  {
    $addFields: {
      departament: {
        $arrayElemAt: ["$departament", 0]
      }
    }
  }
]
```
