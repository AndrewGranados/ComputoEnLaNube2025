# Practica 3. Updates y Deletes

1. Cambiar el salario del empleado 'Imogene Nolan'. y asiganarle 8000
```json
db.Empleados.updateOne(
    {nombre: "Imogene"},
    {$set:{salario:8000}}
)

db.Empleados.updateOne({
    $and:[
        {nombre:'Imogene'},
        {apellidos:'Nolan'}
    ]
},
{$set:{salario:8000}}
)
```

2. Cambiar "Belgium" por "Bélgica" en los empleados (debe haber dos)
```json
db.Empleados.updateMany(
    {pais: "Belgium"},
    {$set:{pais:"Bélgica"}}
)
```

3. Incrementar el salario de todos los empleados de google en 1000
```json
db.Empleados.updateMany(
    {empresa: "Google"},
    {$inc:{salario:1000}}
)
```

4. Reemplazar el empleado omas gentry por el siguiente documento:
```json
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
```
```json
db.Empleados.replaceOne(
    {nombre:"Omar"},
    {
        nombre: "Omar",
        apellidos: "Gentry",
        correo: "sin correo",
        direccion: "Sin calle",
        region: "Sin region",
        pais: "Sin pais",
        empresa: "Sin empresa",
        ventas: 0,
        salario: 0,
        departamentos: "Este empleado ha sido anulado"
    }
)
```

5. Comprobar que el empleado ha sido modificado
```json
db.Empleados.find({nombre:"Omar"})
```

6. Borrar todos los empleados que ganen más de 8500, nota: deben ser borrados 3 documnetos
```json
db.Empleados.deleteMany({salario:{$gt:8500}})
```

7. Visualizar con una expresión regular todos los empleados que comiencen con R. 
```json
db.Empleados.find({nombre:/^R/})
```

8. Buscar todas las regiones que contengan una "v".
Hacerlo con el operador regex y que distinga mayus ni mininusculas (deben salir 2)
```json
db.Empleados.find({region:{$regex:/v/, $options: 'i'}})
```

9. Visualizar los apellidos de los empleados ordenados por el propio apellido
```json
db.Empleados.find(
    {},
    {
        _id:0,
        apellidos:1
    }
).sort({apellidos:1})
```

10. Indicar el número de empleados que trabajen en google
```json
db.Empleados.find({empresa:"Google"}).size()
```

11. Borrar la colección empleados y la base de datos
```json
db.Empleados.drop()

db.dropDatabase()
```