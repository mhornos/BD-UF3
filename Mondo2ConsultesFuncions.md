# Exercici 2 CONSULTES AMB FUNCIONS


1)
db.students.find({gender:"H"},{})

2)
db.students.find({gender:"M"},{}).count()

3)
db.students.find({birth_year:1993},{})

4)
db.students.find({$and:[{gender:"H"},{birth_year:1993}]},{})

5)
db.students.find({birth_year: {$lte:1990}},{})

6)
db.students.find({$or:[{birth_year:1990},{birth_year:{$lte:1990}}]},{})

7)
db.students.find({$and: [{birth_year: {$gte:1990}},{birth_year:{$lte:1999}}]},{})

8)
db.students.find({$and: [{birth_year: {$gte:1990}},{birth_year:{$lte:1999}},{gender:"M"}]},{})

9
