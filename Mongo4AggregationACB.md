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

2. L’entrenador “Pedro Martínez” és un dels entrenadors més veterans. Quants partits
ha participat com a entrenador. Independentment de si ho ha fet com a local o com
a visitant. Restringeix la consulta als partits de lla Lliga Regular de la temporada
2023-2024.
```js
[
  {
    $match: {
      temporada: "2023-2024",
      entrenador: "Pedro Martínez",
      competicio: "Lliga Regular"
    }
  }
]
```
   
3. La llicència "JFL" indica que és un jugador de formació. Quants jugadors tenim amb
aquesta llicència?.
```js
db.jugadors.find({ llicencia: "JFL" }).count()
```

4. Passa a Decimal el camp alcada de la col·lecció jugadors.
(https://www.mongodb.com/docs/manual/reference/operator/aggregation/convert/)
```js
[
  {
    "$addFields": {
      "alcada_decimal": { "$convert": { "input": "$alcada", "to": "decimal" } }
    }
  }
]

```

5. Quins jugadors tenim el compte d'Instagram? Mostra el nom_curt del jugador i
l'usuari d'Instragram.
```js
[
  {
    $match: {
      "xarxes_socials.nom": "instagram",
      "xarxes_socials.usuari": { $exists: true, $ne: "" }
    }
  },
  {
    $project: {
      _id: 0, nom_curt: 1,"usuario_instagram": {
        $arrayElemAt: [
          "$xarxes_socials.usuari", { $indexOfArray: ["$xarxes_socials.nom", "instagram"] }
        ]
      }
    }
  }
]
```

6. Dona el número total de punts de l'equip local del partit amb codi_acb :"103778"
```js
[
  { $match: { codi_acb: "103778" } },
  { $group: { _id: null, punts_local: { $sum: "$punts_local" } } },
  { $project: { _id: 0, punts_local: 1 } }
]
```
    
7. Obtenir els punts de cada equip (local i visitant) del partit amb codi_acb: "103778"
```js
[
  { "$match": { "codi_acb": "103778" } },
  { "$project": { "_id": 0, "puntos_local": "$punts_local", "puntos_visitante": "$punts_visitant" } }
]
```
