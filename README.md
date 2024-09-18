## mostrar bases de datos existentes
```
show databases;
```

## crear/usar una base de datos
```
use <name-database>;
```

## crear una coleccion
```
db.createCollection("nombre_de_la_colección", opciones);
```

## mostar colecciones
```
show collections;
```

## insertar un registro
```
db.<collection_name>.insertOne(
	{
		"llave":"valor",
		"llave:":"valor"
	}
)
```

## insertar varios registros en una coleccion
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

## consultar los registros de una coleccion
1) podemos utilizar .pretty() para que regrese los datos con un formato identando
```
db.<collection_name>.find()

db.<collection_name>.find().pretty()
```

## consultar los registros de una coleccion con filtros
- cuando se agrega un objeto dentro del .find() se interpreta como un filtro
```
db.<collection_name>.find({"llave":"valor"})
```
- utilizando operadores especiales para filtros mas complejos
	- gt: Greater Than -> mayor que
	- 

```
db.<collection_name>.find({"llave": {$<operador>:"valor"} })

//ejemplo, obtenemos todos los registros de la coleccion de usuarios donde la edad sea mayor a 25 años
db.usuarios.find({edad: {$gt:25} })
```
- Nota: si se utilizan los filtros con el .findOne() regresa siempre el primer registro que encuentre y cumpla con la condicion

## actualizar un registro
```
db.<collection_name>.updateOne({<where>}, {$set:{<argumentos>}})

db.<collection_name>.updateOne({"llave":"valor"}, {$set:{"llave":"valor"}})
```

## actualizar varios registros
```
// se puede colocar el primer parametros en vacio para que se aplique a todos los registros
db.<collection_name>.updateMany({}, {$set:{<argumentos>}})

db.<collection_name>.updateMany({}, {$set:{"llave":"valor"}})
```

## eliminar un registro
```
db.<collection_name>.deleteOne(<opciones>)

// aqui se estipula el filtro por el cual se desea borrar el registro
db.<collection_name>.deleteOne({"llave":"valor"})

// ejemplo
db.usuarios.deleteOne({"nombre":"chanchito"})
```

## eliminar varios registros
1) se eliminan varios registros cuando se tenga algo en comun 
```
db.<collection_name>.deleteMany(<opciones>)

db.<collection_name>.deleteMany({"llave":"valor"})
```
2) eliminar todos los registros
```
db.<collection_name>.deleteMany({})
```


