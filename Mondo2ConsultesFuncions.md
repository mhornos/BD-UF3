# Exercici 2 CONSULTES AMB FUNCIONS


 1. Busca els estudiants de gènere masculí
```js
db.students.find({gender:"H"},{})
```

2. Digues quina és la quantitat d’estudiants de gènere femení.
```js
db.students.find({gender:"M"},{}).count()
```

3. Busca els estudiants nascuts l’any 1993
```js
db.students.find({birth_year:1993},{})
```

4. Busca els estudiants de gènere masculí i nascuts l’any 1993
```js
db.students.find({$and:[{gender:"H"},{birth_year:1993}]},{})
```

5. Busca els estudiants nascuts després de l’any 1990
```js
db.students.find({birth_year: {$lte:1990}},{})
```

6. Busca els estudiants nascuts abans o durant l’any 1990
```js
db.students.find({$or:[{birth_year:1990},{birth_year:{$lte:1990}}]},{})
```

7. Busca els estudiants nascuts a la dècada dels 90s
```js
db.students.find({$and: [{birth_year: {$gte:1990}},{birth_year:{$lte:1999}}]},{})
```

8. Busca els estudiants de gènere femani nascus a la dècada dels 90s
```js
db.students.find({$and: [{birth_year: {$gte:1990}},{birth_year:{$lte:1999}},{gender:"M"}]},{})
```
