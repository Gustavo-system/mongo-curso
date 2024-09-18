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

## consultar los registros de una coleccion
1) podemos utilizar .pretty() para que regrese los datos con un formato identando
```
db.<collection_name>.find()

db.<collection_name>.find().pretty()
```

## eliminar un registro
```
db.<collection_name>.deleteOne(<opciones>)

// aqui se estipula el filtro por el cual se desea borrar el registro
db.<collection_name>.deleteOne({"llave":"valor"})

// ejemplo
db.usuarios.deleteOne({"nombre":"chanchito"})
```


