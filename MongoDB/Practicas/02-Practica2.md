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
