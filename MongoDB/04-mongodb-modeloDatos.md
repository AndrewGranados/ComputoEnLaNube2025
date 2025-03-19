# Modelo de datos en mongodb

- Modelos
    - Embebidos
    - Referenciados
    
## Modelos Embebidos
**Uno a uno**
```json
db.departamentos.insertMany([
    {
        _id:1,
        nombre:"Tecnologias de informacion",
        responsable:{
            nombre:"Monico",
            apellidos:"Martinez Perez",
        },
        descripcion:"Ejemplo de departamento"
    },
    {
        _id:2,
        nombre:"Contabilidad",
        responsable:{
            nombre:"Raul",
            apellidos:"Lopez Hernandez",
        },
        descripcion:"Ejemplo de departamento"
    }    
])
```

- Hacer una proyeccion
```json
db.departamentos.find({}, {nombre:1, _id:0, "responsable.nombre":1})
```


## Modelos Referenciados
**Uno a uno**
```json
db.localidades.insertMany([
    {
        _id:"BA",
        ciudad:"Buenos Aires",
        pais:"Argentina",
        poblacion:"16 millones",
        turismo:["edificios", "tango", "gastronomia", "museos", "parques"],
        direccion:"Avenida Tortolos",
        cod_departamento:1
    },
    {
        _id:"SA",
        ciudad:"Santiago",
        pais:"Chile",
        poblacion:"20 millones",
        turismo:["iglesias", "vino", "gastronomia", "museos", "parques"],
        direccion:"Calle Soyla vaca del corral",
        cod_departamento:2
    }
])
```

- Hacer una consulta de look up
```json
db.departamentos.aggregate([
    {
        $lookup:{
            from:"localidades",
            localField:"_id",
            foreignField:"cod_departamento",
            as:"localidades"
        }
    }
])
```


1. Realizar otros insertMany y look up de categorias y productos
- Insert Categoria
```json
db.categoria.insertMany([
    {
        _id:1,
        categoria: 1,
        nombre:"Categoria_1", 
    },
    {
        _id:2,
        categoria: 2,
        nombre:"Categoria_2", 
    }    
])
```

- Insert Producto
```json
db.producto.insertMany([
    {
        _id:"AA",
        nombre:"producto_1",
        precio:20,
        existencia:20,
        cod_categoria:1
    },
    {
        _id:"BB",
        nombre:"producto_2",
        precio:40,
        existencia:10,
        cod_categoria:2
    }
])
```

- Look Up 
```json

db.categoria.aggregate([
  {
    $lookup: {
      from: "producto",
      localField: "_id",
      foreignField: "cod_categoria",
      as: "Productos"
    }
  },
  {
    $project: {
      _id: 0,
      nombre: 1
    }
  }
])
```