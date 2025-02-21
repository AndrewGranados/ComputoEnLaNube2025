# Consultas
1. Cargar el archivo empleados.json

- En local:
    comando:
        mongoimport --db curso --collection empleados --file empleados.json

[Empleados.json](./Data/empleados.json)

2. Utilizar la base de datos curso
```json
use curso
```

3. Buscar todos los empleados que trabajan en google
```json
db.Empleados.find({empresa:'Google'})
```

4. Empleados que vivan en peru
```json
db.Empleados.find({pais:'Peru'})
```

5. Empleados que ganen más de 8 mil dolares
```json
db.Empleados.find({salario:{$gt:8000}})
```

6. Empleados con ventas inferiores a 10 mil
```json
db.Empleados.find({ventas:{$lt:10000}})
```

7. Realizar la consulta anterior pero devolviendo una sola fila
```json
db.Empleados.findOne({ventas: {$lt: 10000}})
```

8. Empleados que trabajan en google o en yahoo con el operador $in
```json
db.Empleados.find({
    $or:[{empresa:'Google'},
        {empresa:'Yahoo'}]
})
```

9. Empleados de amazon que ganen más de 9000 dolares 
```json
db.Empleados.find({
    $and:[{empresa:'Amazon'},
        {salario:{$gt:9000}}]
})
```

10. Empleados que trabajen en yahoo que ganen más de 6000 o empleados que trabajen en google que tengan ventas inferiores a 20000
```json
db.Empleados.find({
    $or:[
        {$and:[{empresa:"Yahoo"},{salario:{$gt:6000}}]},
        {$and:[{empresa:"Google"},{ventas:{$lt:20000}}]}
    ]
})
```

11. Visualizar el nombre, apellido y el pais de cada empleado
```json
db.Empleados.find({},{_id:0, nombre:1, apellidos:1, pais:1})
```

# Modificando Documentos
## Comandos importantes

1. updateOne -> Modificar un solo documento
2. updateMany -> Modificar multiples documentos
3. replaceOne -> Sustituir el contenido completo de un documento

Tiene el siguiente formato

```json
db.collection.updateOne(
    {filtro},
    {operador: }
)
```

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Operador Set

1. Modificar un documento 
```json
db.libros.updateOne(
    {titulo:'Python para todos'},
    {$set:{titulo:'Java para todos'}}
)
```

```json
db.libros.updateOne({_id:2}, {$set:{precio:56, existencia:10}})
```

2. Actualizar el precio a 100 y la cantidad a 50 para el _id:10 
```json
db.libros.updateOne(
    {_id:10},
    {$set:{precio:100, cantidad:50}}
)
```

#### Modificar multiples Documentos

1. Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
```json
db.libros.updateMany(
    {precio:{$gt:100}},
    {$set:{precio:150}}
)
```

### Operador $inc y $mul
1. Actualizar con un incremento de $5 todos los documentos 
```json
db.libros.updateMany(
    {},
    {$inc:{precio:5}}
)
```

2. Actualizar con multiplicación de 2 todos los documentos que sean mayores a 20
```json
db.libros.updateMany(
    {cantidad:{$gt:20}},
    {$mul:{cantidad:2}}
    )
```

3. Actualizar todos los documentos donde el precio sea mayor a $20 y se multimpliquen por 2 la cantidad y el precio 
```json
db.libros.updateMany(
    {precio:{$gt:20}},
    {$mul:{cantidad:2, precio:2}}
)
```

### Reemplazar documentos (replaceOne)
```json
db.libros.replaceOne(
    {_id:2},
    {
        titulo:'De la tierra a la luna',
        autor:'Julio Verne',
        precio:500}
)
```

### Borrar Documentos
1. deleteOne -> Elimina un solo documento
2. deleteMany -> Elimina multiples documentos


1. Eliminar el documento con id 2
```json
db.libros.deleteOne({_id:2})
```

2. Eliminar los documentos donde la cantidad sea mayor o igual a 150
```json
db.libros.deleteMany({cantidad:{$gte:150}})
```

### Expresiones regulares
1. Buscar los libros donde el titluo contengan la letra t
```json
db.libros.find({titulo:/t/})
```

2. Buscar los libros que en el titulo contengan la palabra 'Json'
```json
db.libros.find({titulo:/JSON/})
```

3. Todos los documentos que en el titulo terminen en tos
```json
db.libros.find({titulo:/tos$/})
```

4. Todos los documentos que en el titulo comiencen con J
```json
db.libros.find({titulo:/^J/})
```

### Operador $regex
[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

1. Seleccionar los libros que contengan con la palabra 'para' en titulo
```json
db.libros.find({titulo:{$regex:'para'}})
```

2. Seleccionar los libros que contengan con la palabra 'JSON' en titulo
```json
db.libros.find({titulo:{$regex:'JSON'}})
```
```json
db.libros.find({titulo:{$regex:/JSON/}})
```

3. Distinguir entre mayusculas y minusculas 
```json
db.libros.find({titulo:{$regex:/json/}}) 
```
-> no distingue entre mayusculas y minusculas
```json
db.libros.find({titulo:{$regex:/json/, $options:'i' }}) 
```
-> distingue entre mayusculas y minusculas

4. Seleccionar todos los libros que comiencen con J o j
```json
db.libros.find({titulo:{$regex:'^J', $options:'i'}})
```

5. Seleccionar todos los libros que terminen con 'es'
```json
db.libros.find({titulo:{$regex:'es$', $options:'i'}})
```

### Metodo Sort (ordenar documentos)
1. Ordenar los libros de manera ascendente por el precio
```json
db.libros.find({},{titulo:1, precio:1, _id:0}).sort({precio:1})
```

2. Ordenar los libros de manera descendente por el precio 
```json
db.libros.find({},{titulo:1, precio:1, _id:0}).sort({precio:-1})
```

3. Ordenar los libros de manera ascendente por el editorial y de manera descendente por el precio, mostrando el titulo, precio y editorial
```json
db.libros.find(
    {},
    {titulo:1, precio:1, editorial:1, _id:0}).sort({editorial:1, precio:-1}) 
```

### Otros metodos skip, limit, size
```json
db.libros.find({}).size()
```

```json
db.libros.find({editorial:{$regex:/java/i}}).size()
```

```json
db.libros.find({titulo:{$regex:/java/i}}).size()
```

1. Buscar todos los libros pero mostrando los dos primeros
```json
db.libros.find({},{titulo:1, editorial:1, precio:1, _id:0}).limit(2)
```

2. Mostrar los 3 ultimos libros
```json
db.libros.find({},{titulo:1, precio:1, editorial:1}).skip(2)
```

3. seleccionar toodos los libros de forma descendente, saltando los 2 primeros y mostrar el tamaño
```json
db.libros.find({}).sort({titulo:-1}).skip(2).size()
```

### Borrar colecciones y bases de datos
```json
use db5

db.createCollection('ejemplo')

db.ejemplo.insertOne({
    nombre: 'Petroleo'
})

db.dropDatabase()
```