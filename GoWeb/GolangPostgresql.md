# Golang Postgresql

## Contenido

1. [Driver](#driver)
2. [Conexión](#conection)
    - [Consulta](#consulta)
    - [GetConection](#get)



<a name="driver"></a>
## Driver

Usamos el driver:

[https://github.com/lib/pq]

<a name="conection"></a>

## Conexión

Hacemos los imports

```go
import (
	"database/sql"
	"fmt"

	_ "github.com/lib/pq"
)
```

Definimos constantes para la conexión 

```go
const (
	host     = "localhost"
	port     = "5432" // Es el puerto por defectp
	user     = "postgres"
	password = "contrasena"
	dbname   = "db"
)
```

Main con conexión 

```go
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/lib/pq"
)

const (
	host     = "localhost"
	port     = "5432"
	user     = "postgres"
	password = "cdecohetey"
	dbname   = "dbsupermercado"
)

func main() {
	// Creamos el string de conexion
	conection := fmt.Sprintf("host=%s port=%s user=%s password=%s dbname=%s sslmode=disable",
		host, port, user, password, dbname)

	// Hacemos la conexion y especificamos que es a postgres
	db, err := sql.Open("postgres", conection)
	// Verificamos la conexion
	if err != nil {
		panic(err)
	}
	// Cerramos la conexion
	defer db.Close()
	fmt.Println("Conexion correcta a Postgres")
}

```

<a name="consulta"></a>

Hacer una consulta

```go
/*
Ya tiene que estar abierta la conexion
*/

// Definimos el query
	query := `select idcategoria, nombre, descripcion
	from public.categoria`

	// Hacemos la consulta
	rows, err := db.Query(query)
	if err != nil {
		panic(err)
	}

	// Iteramos sobre rows
	oLista := []categoria{}
	for rows.Next() {
		oCategoria := categoria{}
		rows.Scan(&oCategoria.idcategoria, &oCategoria.nombre, &oCategoria.descripcion)

		oLista = append(oLista, oCategoria)
	}

	fmt.Println(oLista)
```

Pasar parámetro

```go
// Definimos el query con parametro
	query := `select idcategoria, nombre, descripcion
	from public.categoria
	where idcategoria = $1`

	// Hacemos la consulta con parametro 2
	rows, err := db.Query(query, 2)
	if err != nil {
		panic(err)
	}
```

<a name="get"></a>

Obtener conexión asíncrona en singleton

```go
var db *sql.DB // singleton
var once sync.Once

// GetConectionPostgres resgresa una conexion a postgres
func GetConectionPostgres() *sql.DB {
	once.Do(func() {
		var err error
		psqlInfo := fmt.Sprintf("host=%s port=%s user=%s password=%s dbname=%s sslmode=disable",
			host, port, user, password, dbname)
		db, err = sql.Open("postgres", psqlInfo)
		if err != nil {
			panic(err)
		}
	})

	return db
}
```

