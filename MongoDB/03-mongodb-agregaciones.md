# Agregaciones en MongoBD (Framework)

## Métodos para realizar agregaciones simples 
- distinct(): Devuelve valores no duplicados 
- countDocuments(): Cuenta los documentos en una colección
- estimatedDocument(): Cuenta de manera aproximada durante un periodo del tiempo

## Una Aggregation pipeline
Consta de una o más etapas (stage) que procesan documentos
1. Cada etapa realiza una operación en los documentos de enterada, por ejemplo, una fase puede filtrar documentos, agregar documentos y calcular valores.

2. Los documentos que se generan en una fase pasan a la siguiente fase.

3. Puede devolver resultados para grupos de documentos como totales, máximos, mínimos, etc

### Se utiliza la clausula "aggregate" 
- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos: etapa, comparación, booleanos, aritméticos, de cadena, etc. 

## Métodos Simples: countDocuments() y distinct()
1. Contar los documentos de la colección libros
```json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial "terra"
```json
db.libros.countDocuments({editorial: {$eq:'Terra'}})
```

3. Seleccionar o mostrar todos los libros mostrando soñ

4. Mostrar todas las distintas editoriales
```json
db.libros.count()
```

[Documentación de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline básica

### Tienen funciones de etapa
```json
db.libros.aggregate(
    [
        {
        $match:{editorial:"Terra"}
        }
    ]
)
```

## $project. Incluir y renombrar campos

```json
db.libros.aggregate(
    [
        {
            $match:{editorial:'Terra'}
        },
        {
            $project:{
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial:"$editorial",
                editorial:1
            }
        }
    ]
)
```

# Crear campo calculado con project y match

```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total De Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## $sort. Ordenaciones 
1. Ordenar el ejercicio anterior de mayor a menor por el precio
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total De Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        precio: 1
      }
  }
]
```

## $group. Agrupaciones
[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

1. Cuantos documentos hay por cada una de las editoriales
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Número de Documentos": {
          $count: {}
        }
      }
  }
]
```

2. Cuantos documentos hay por cada una de las editoriales por numero de docuemntos de manera descendente
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        "Numero Documentos": -1
      }
  }
]
```

## $group. Agrupaciones ocupando mongo atlas y listingAndReviews
3. Agrupar por tipo de propiedad mostrando el numero de propiedades y el promedio de sus precios
- SELECT * property_type, count(*) as Numero avg(price) as Media FROM listingAndReviews
```json
[
  {
    $group:
    {
      _id: "property_type",
      Numero:{
        $count:{}
      },
      Media:
      {
        $avg: "$price"
      }
    }
  }
]
```

## Operadores $set
```json
[
  {
    $group:
      {
        _id: "property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "Media"
        }
      }
  }
]
```

## Operadores $unset
```json
[
  {
    $group:
      {
        _id: "property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
     $unset: 
   {
     'Media',
     'Media_Total'
   }
   }
]
```

1. Creando nuevas colecciones utilizando el operador $out
- Nota: debe ser el último en la agregación

```json
{
  db: "sample_airbnb",
  coll: "media_propiedades"
}
```


- Ejemplos con operadores de comparación y lógicos
```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        barato: {
          $lt: ["$price", 100]
        }
      }
  }
]
```

## $cond - Devuelva valores según una condición (es parecido a un operador ternario de un lenguaje de programación)
```json
[
  {
    $project: {
      _id: 0,
      price: 1,
      room_type: 1,
      caro: {
        $cond: [
          {
            $gt: ["$price", 300]
          },
          "Si",
          "No"
        ]
      },
      medio: {
        $cond: [
          {
            $and: [
              {
                $gte: ["$price", 100]
              },
              {
                $lte: ["$price", 300]
              }
            ]
          },
          "Si",
          "No"
        ]
      },
      barato: {
        $cond: [
          {
            $lt: ["$price", 100]
          },
          "Si",
          "No"
        ]
      }
    }
  }
]
```

- Views
```json
db.createView("ganancias_libros", 
"libros",
  [
  {
    $match:
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        "Total de Ganancias": -1
      }
  }
]
)
```


