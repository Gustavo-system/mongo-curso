# Base de datos MONGO

#### Mostrar bases de datos
```
show databases
```

### Eliminar la bases de datos
```
db.dropDatabase()
```

# Colecciones

#### Crear / usar una base de datos
- cuando se usa el comando **use** se crea la base de datos pero no se mostrara con el comando anterior hasta que se cree una coleccion.
```
use <name-database>
```

#### Crear una coleccion
```
db.createCollection("nombre_de_la_colección", opciones);
```

#### Mostar colecciones
```
show collections
```

### Eliminar colecciones
```
db.<collection_name>.drop()
```

# Registros

### Insertar un registro
```
db.<collection_name>.insertOne(
	{
		"llave":"valor",
		"llave:":"valor"
	}
)
```

### Insertar varios registros en una coleccion
- Se debe tener en consideracion que se deben agregar por medio de una matriz utilizando "[ ]"
```
db.<collection_name>.insertMany([
	{
		"llave":"valor",
		"llave:":"valor"
	},
	{
		"llave":"valor",
		"llave:":"valor"
	}
])
```

### Consultar todos los registros de una coleccion
- podemos utilizar .pretty() para que regrese los datos con un formato identando
```
db.<collection_name>.find()

db.<collection_name>.find().pretty()
```

- NOTA: en la shell solo podemos visualizar de 20 en 20 registros, podemos usar alternativas como:
	- .toArray() muestra todos los registros en una matriz
	- .forEach((valor) => {printjson(valor)}) esto nos permite recorrer uno por uno de los registros y mostrar todos

### Consultar los registros de una coleccion con filtros
- cuando se agrega un objeto dentro del .find() se interpreta como un filtro
```
db.<collection_name>.find({"llave":"valor"})
```
- utilizando operadores especiales para filtros mas complejos
	- gt: Greater Than -> mayor que

```
db.<collection_name>.find({"llave": {$<operador>:"valor"} })

//ejemplo: obtenemos todos los registros de la coleccion de usuarios donde la edad sea mayor a 25 años
db.usuarios.find({edad: {$gt:25} })
```
- Nota: si se utilizan los filtros con el .findOne() regresa siempre el primer registro que encuentre y cumpla con la condicion

### Consultar los registros con proyeccion
- se utliza para mostrar solo la informacion que se desee
- el primer valor siempre se coloca en "{ }" ahi se indica que se busque en todos los registros
- el "_id" siempre se incluye a expecion que se indique de forma explicita "{ _id:0 } "
- se utliza por medio de 1 y 0
	- 1: si se muestra
	- 0: no se muestra
```
db.<collection_name>.find({}, { "llave": 1 })
```

### Actualizar un registro
- para utilizar el updateOne() se debe proporcionar un valor por el cual se realizara el filtrado
```
// sintaxis
db.<collection_name>.updateOne({<where>}, {$set:{<argumentos>}})
db.<collection_name>.updateOne({"llave":"valor"}, {$set:{"llave":"valor"}})

// ejemplo: actualizar la edad de un usuario
db.usuarios.updateOne({_id:"12345"}, {$set: { edad:30 } })
```
- podemos utilizar .update() pero de igual forma especificar un valor por el cual podamos filtrar el registro
```
db.usuarios.update({_id:"12345"}, {$set: { edad:30 } })
```
- NOTA: si se utiliza el update sin el operador "$set" se eliminaran los datos de la coleccion y persistira la llave-valor indicada
```
db.usuarios.update({_id:"12345"}, { edad:30 })
```
- NOTA: si se quiere remplazar todos los datos de un registro podemos usar .replaceOne()
```
db.usuarios.replaceOne({_id:"12345"}, { "nombre":"chanchito", edad:30, "esSocio": "true"})
```

### Actualizar varios registros
```
// se puede colocar el primer parametros en vacio para que se aplique a todos los registros
db.<collection_name>.updateMany({}, {$set:{<argumentos>}})

db.<collection_name>.updateMany({}, {$set:{"llave":"valor"}})
```

### Eliminar un registro
```
db.<collection_name>.deleteOne(<opciones>)

// aqui se estipula el filtro por el cual se desea borrar el registro
db.<collection_name>.deleteOne({"llave":"valor"})

// ejemplo
db.usuarios.deleteOne({"nombre":"chanchito"})
```

### Eliminar varios registros
1) se eliminan varios registros cuando se tenga algo en comun 
```
db.<collection_name>.deleteMany(<opciones>)

db.<collection_name>.deleteMany({"llave":"valor"})
```
2) eliminar todos los registros
```
db.<collection_name>.deleteMany({})
```

# Objetos y Matrices dentro de documentos

### Objetos

- Los registros pueden tener objetos como registros

```
{
    "_id": ObjectId("6478db6f8d9e8f271d4718e1"),
    "nombre": "Teléfono Móvil X",
    "precio": 199.99,
    "descripcion": "El mejor teléfono del mercado",
    "detalles": {
        "marca": "XYZ",
        "modelo": "X10",
        "color": "Negro"
        "autor": {
            "nombre": "chanchito"
        }
    }
}
```

### Matrices

- Los registros pueden tener matrices como registros

```
{
    "_id": ObjectId("6478db6f8d9e8f271d4718e1"),
    "nombre":"Teléfono Móvil X",
    "precio":199.99,
    "descripcion":"El mejor teléfono del mercado",
    "detalles":{
        "marca":"XYZ",
        "modelo":"X10",
        "color":"Negro"
    },
    "comentarios":[
        {
            "usuario":"Juan Pérez",
            "fecha":"2024-05-20T10:23:34Z",
            "texto":"Excelente producto"
        },
        {
            "usuario":"Ana García",
            "fecha":"2024-05-21T15:45:12Z",
            "texto":"La batería dura poco"
        }
    ],
    "numeros":[
        "2222222222",
        "0000000000"
    ]
}
```

### Accediendo a los datos estructurados
1) Nos retorna solo una parte de la coleccion
```
// sintaxis
db.<collection_name>.findOne({"llave":"valor"}).<nodo>

// ejemplo
db.usuarios.findOne({_id:ObjectId("6478db6f8d9e8f271d4718e1")}).numeros

// resultado del ejemplo
[ "2222222222", "0000000000" ]
```

2) filtrando por medio de un campo dentro de la estructura
- Se debe tomar en cuenta que si es necesario agregar **""** en el filtro para que se pueda utilizar el **( . )** y acceder a los nodos
```
// sintaxis
db.<collection_name>.findOne({"llave.llave.llave":"valor"})

// ejemplo
db.usuarios.findOne({"detalles.color":"Negro"})
```

# Tipos de datos Mongo
- Texto: "Juan"
- Booelan: true
- Numero
	- NumberInt (int32): 25
	- NumberLong (Int64): 2090845886852
	- NumberDecimal (Int128): 1000.55
- ObjectId: ObjectId("123")
- Fecha
	- ISODate: ISODate("2022-10-10)
	- Timestamp: Timestamp(123213123, 1)
- Documento Incrustado: {"name": "valor"}
- Array (Matriz): {"name": [ "valor ]}

Obtener el tipo de dato de un valor 
```
typeof db.<nameColection>.find().<llave>

typeof db.usuarios.find().edad
-> number
```

# Relaciones

### Relacion uno a uno - incrustado
La relacion se realiza dentro del documento en este caso el objeto historial tiene la relacion dentro del documento
```
{
	"_id" : ObjectId("5fb31271267317831h123"),
	"nombre" : "Juan",
	"edad" : 25,
	"historial" : {
		"enfermedades" : [
			"gripe",
			"tos"
		]
	}
}
```

### Relacion uno a uno - referencia
La relacion se hara por medio del _id que se encuentra en otro documento, este _id puede ser personalizado o por medio del ObjectId

Cuando la relacion es por referencia, al modificar los datos de un documento que se usa como relacion se modifica el valor a todos los que tengan ensertado ese registro

```
Coleccion de vehiculos
{
	"_id" : "VW1",
	"modelo" : "Jetta",
	"marca" : "VW",
	"precio" : 2500
}

Coleccion de personas
{
	"_id" : ObjectId("5fb31271267317831h123"),
	"nombre" : "Juan",
	"edad" : 25,
	"vehiculo" : "VW1"
},
{
	"_id" : ObjectId("5fb3123123u53831h123"),
	"nombre" : "Pedro",
	"edad" : 40,
	"vehiculo" : "VW1"
}
```

### Relacion uno a muchos - incrustado

### Relacion uno a muchos - referencia

### Relacion muchos a muchos - incrustado

### Relacion muchos a muchos - referencia