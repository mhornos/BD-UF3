# Exercicis AGGREGATION FRAMEWORK (bd examen ACB)

1. Volem comparar l'alçada dels dos germans Gasol. El noms curts són "Pau Gasol" i
"Marc Gasol".
```js
[
  {
    $match: {
      nom_curt : {$in : ["Marc Gasol", "Pau Gasol"]}
    }
  },
  {
    $project: {
     nom_curt: 1, alcada : 1, _id : 0
    }
  }
]
```
