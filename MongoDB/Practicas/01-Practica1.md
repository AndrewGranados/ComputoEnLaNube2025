# Practica 1: Bases de Datos, colecciones e inserts

1. Conectarnos con mongosh a mongodb 
1. Crear una base de datos llamada curso
1. Comprobar que la base de datos no existe
1. Crear una colección que se llame facturas y mostrarla

```json
db.createCollection("Facturas")

show collection
```

5. Insertar un documento con los siguientes datos

| Codigo   | Valor   |
|-------------|-------------|
| cod_cactura | 10 |
| ciente | Frutas Ramirez |
| total | 223 |

| Codigo   | Valor   |
|-------------|-------------|
| cod_factura | 20 |
| ciente | Ferreteria Juan |
| total | 140 |


```json
db.Facturas.insertOne(
    {
        cod_factura: 10,
        cliente: "Frutas Ramirez",
        total: 223,
    }
)

db.Facturas.insertOne(
    {
        cod_factura: 20,
        cliente: "Ferreteria Juan",
        total: 140,
    }
)
```

6. Crear una nueva colección pero usando directamente el insert one.
Insertar un documento en la colección productos con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| cod_producto | 1 |
| nombre | Tornillo x1" |
| precio | 2 |
| unidades | 1500 |

```json
db.Productos.insertOne(
    {
        cod_producto: 10,
        nombre: 'Tornillo x1"',
        precio: 2,
        unidades: 1500
    }
)
```

7. Crear un nuevo documento de producto que contenga un array con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| cod_producto | 2 |
| nombre | Martillo |
| precio | 20 |
| unidades | 50 |
| fabricantes | fab1, fab2, fab3, fab4 |

```json
db.Productos.insertOne(
    {
        cod_producto: 2,
        nombre: "Martillo",
        precio: 20,
        unidades: 50,
        fabricantes: ["fab1", "fab2", "fab3", "fab4"]
    }
)
```

8. Borrar la colección facturas y comprobar que se borró

```json
db.Facturas.drop()

show collections
```

9. Insertar un documento en una colección denominada **fabricantes**, para probar los subdocumentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| nombre | fab1 |
| localidad | ciudad: buenos aires, pais: argentina, calle: pez, cod_postal: 2900 |

```json
db.Fabricantes.insertOne(
    {
        _id: 1,
        nombre: "fab1",
        localidad: {
            ciudad: "buenos aires",
            pais: "argentina",
            calle: "pez",
            cod_postal: 2900
        }
    }
)
```

10. Realizar una inserción de varios documentos en la colección Productos

| Codigo   | Valor   |
|-------------|-------------|
| cod_producto | 3 |
| nombre | Alicates |
| precio | 10 |
| unidades | 25 |
| fabricantes | fab1, fab2, fab5 |

| Codigo   | Valor   |
|-------------|-------------|
| cod_producto | 4 |
| nombre | Arandelas |
| precio | 1 |
| unidades | 500 |
| fabricantes | fab2, fab3, fab4 |

```json
db.Productos.insertMany(
[
    {
        cod_producto: 3,
        nombre: "Alicates",
        precio: 10,
        unidades: 25,
        fabricantes: ["fab1", "fab2", "fab5"]
    },
    {
        cod_producto: 4,
        nombre: "Arandelas",
        precio: 1,
        unidades: 500,
        fabricantes: ["fab2", "fab3", "fab4"]
    }
])
```


