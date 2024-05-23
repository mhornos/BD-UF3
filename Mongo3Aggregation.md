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

