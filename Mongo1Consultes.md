- Crea la BD rrhh
- Importa el fitxer departaments.json → col·lecció departaments
- Importa el fitxer empleats.json → col·lecció empleats

  
# Exercici 1 CONSULTES
1. Obtenir de tots els empleats, el nom, cognoms i salari. Mostrar només 4 registres
   ```js
   db.empleats.find({},{nom:1, cognoms:1, salari:1, _id:0}).limit(4)
   ```
2. Mostra la quantitat de departaments que hi ha
   ```js
   db.departaments.find({},{}).count()
   ```
3. Recupera l’empleat “emplat_id=100”
   ```js
   db.empleats.find({empleat_id:"100"},{})
   ```
4. Recupera els empleats amb el càrrec de “President”
   ```js
   db.empleats.find({"feina.nom":"President"},{})
   ```
5. Recupera els empleats que treballen al departament de “IT”
    ```js
   db.empleats.find({"departament.nom":"IT"},{})
   ```
6. Recupera els empleats que van ser contractats a partir de l’1 de gener de 1985
  ```js
   db.empleats.find({data_contractacio:{$gte:new Date("1985-01-01")}},{})
   ```
7. Recupera els empleats amb un salari superior a 2000
  ```js
   db.empleats.find({salari:{$gt:2000}},{})
   ```
8. Recupera els empleats amb un salari entre 2000 i 6000
  ```js
   db.empleats.find({$and:[{salari:{$gte:2000}}, {salari:{$lte:6000}}]},{})
   ```
9. Recupera els empleats que el seu número de telèfon comença per 515
```js
   db.empleats.find({"telefon":{$regex: /^515/}},{})
   ```
10. Recupera els empleats que no treballin de Vice President. Utilitza el codi de la feina
"AD_VP"
```js
  db.empleats.find({"feina.codi": {$ne:"AD_VP"}},{})
   ```
11. Recupera els empleats que tenen pct_comissio.
```js
   
   ```
12. Recupera els empleats que tenen pct_comissio i hagi treballat o treballin
actualment de "Cap de Vendes" . Utilitza el codi de feina "SA_MAN".
```js
   
   ```
13. Recupera els empleats que han tingut 2 feines. No tinguis en compte la feina
actual.
```js
   
   ```
