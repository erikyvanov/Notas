# HTML Golang

## Contenidos



1. [Mostar HTML](#mostrar-html)
2. [Manejo del template](#manejo-temp)
   - [Variables](#var)
   - [Condicionales](#conditional)
   - [Bucles](#bucles)
3. [Servidor HTML](#servidor)
4. [Servidor archivos estáticos](#server-estatic)

## Mostar HTML <a name="mostrar-html"></a>

```go
package main

import (
	"html/template"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        		w.Header().Set("Content-Type", "text/html")
        
		// Creamos la variable temp y verificamos si hay error
		temp, err := template.ParseFiles("../html/index.html")
		if err != nil {
			panic(err)
		}

		// Se muestra el template
		temp.Execute(w, nil)
	})

	http.ListenAndServe(":8080", nil)
}
```

Hay que indicar que se esta regresando html después de cada `func(w http.ResponseWriter, r *http.Request)`

```go
w.Header().Set("Content-Type", "text/html")
```



## Manejo del template <a name="manejo-temp"> </a>

### Variables <a name="var"></a>

Poner valor de variables en el html se usa {{.NombreDeVariable}}

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User</title>
</head>
<body>
    <h1>User</h1>
    <h3>Name: {{.Name}}</h3>
    <h3>Lastname: {{.Lastname}}</h3>
    <h3>Premium: {{.Premium}}</h3>
</body>
</html>
```

En la ruta del servidor se tiene que pasar una estructura
```go
http.HandleFunc("/user", func(w http.ResponseWriter, r *http.Request) {
		// Creamos la variable temp y verificamos si hay error
		temp, err := template.ParseFiles("../html/user.html")
		if err != nil {
			panic(err)
		}

		// Creamos el usuario a mostrar
		user := User{Name: "Erik", Lastname: "Ivanov", Premium: true}

		// Mostrar el template y pasar la escructura user
		temp.Execute(w, user)
	})
```

### Condicionales <a name="conditional"></a>

Para poner el condicional en el html

```html
	{{if .Premium}}
    <h3>Usuario premium</h3>
    {{else}}
    <h3>Us
    {{end}}
```

Para usar operadores se usa {{if <operador> <variale> <comparacion>}}

Lista de operadores

- Mayor que (gt)

- Mayor o igual que (ge)
- Menor que (lt)
- Menor o igual que (le) 
- Igual que (eq)
- Diferente de (ne)
- and (and)
- || (or)

Ejemplo usando operadores

```html
	{{if ge .Age 18}}
    <h1>Es mayor de edad</h1>
    {{else}}
    <h1>Es menor de edad</h1>
    {{end}}
```

```html
	{{if and (.Premium) (eq .Name "Erik")}}
    <h1>Holaa</h1>
    {{end}}
```

### Bucles <a name="bucles"></a>

For range

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bucles</title>
</head>
<body>
    <h1>Bucles</h1>
    {{range .Friends}}
        <h2>{{.}}</h2>
    {{end}}
</body>
</html>
```

En go se usa una estructura con un array

```go
http.HandleFunc("/bucles", func(w http.ResponseWriter, r *http.Request) {
		// Creamos la variable temp y verificamos si hay error
		temp, err := template.ParseFiles("../html/bucles.html")
		if err != nil {
			panic(err)
		}

		friends := []string{"Erik", "Ivanov", "Dominguez", "Rivera"}
		// Creamos el usuario a mostrar
		user := User{Name: "Erik", Lastname: "Ivanov", Premium: true, Age: 19, Friends: friends}

		// Mostrar el template y pasar la escructura user
		temp.Execute(w, user)
	})
```

## Servidor HTML<a name="servidor"></a>



Se necesita poner un define en todos los archivos html

```html
{{define "index"}}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Home</h1>
    <p>This is my page.</p>
</body>
</html>

{{end}}
```



Para crear un servidor de archivos html se necesitan cargar todos los templates al principio.

```go
// Cargar todos los templates al iniciar el server
var templateTODOS = template.Must(template.New("T").ParseFiles(
	"../html/serverHTML/index.html",
	"../html/serverHTML/user.html",
	"../html/serverHTML/bucles.html",
))
```

Usando el template en un HandleFunc

```go
http.HandleFunc("/user", func(w http.ResponseWriter, r *http.Request) {
		// Creamos el usuario a mostrar
		user := User{Name: "Erik", Lastname: "Ivanov", Premium: true, Age: 19}

		// Mostrar el template y pasar la escructura user
		templateTODOS.ExecuteTemplate(w, "user", user)
	})
```

Para cargar todos los archivos html de una carpeta se usa ParseGlob

```go
var templateTODOS = template.Must(template.New("T").ParseGlob("../html/serverHTML/*.html"))
```

Si esta dividido en subcarpetas

```go
var templateTODOS = template.Must(template.New("T").ParseGlob("../html/serverHTML/**/*.html"))
```

Servidor completo

```go
package main

import (
	"html/template"
	"net/http"
)

// User saves a user's data
type User struct {
	Name     string
	Lastname string
	Premium  bool
	Age      int
	Friends  []string
}

// Cargar todos los templates al iniciar el server
// var templateTODOS = template.Must(template.New("T").ParseFiles(
// 	"../html/serverHTML/index.html",
// 	"../html/serverHTML/user.html",
// 	"../html/serverHTML/bucles.html",
// ))
var templateTODOS = template.Must(template.New("T").ParseGlob("../html/serverHTML/*.html"))

func main() {

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/html")
		// Ejecutamos el template ya precargado
		templateTODOS.ExecuteTemplate(w, "index", nil)
	})

	http.HandleFunc("/user", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/html")
		// Creamos el usuario a mostrar
		user := User{Name: "Erik", Lastname: "Ivanov", Premium: true, Age: 19}

		// Mostrar el template y pasar la escructura user
		templateTODOS.ExecuteTemplate(w, "user", user)
	})

	http.HandleFunc("/bucles", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/html")
		friends := []string{"Erik", "Ivanov", "Dominguez", "Rivera"}
		// Creamos el usuario a mostrar
		user := User{Name: "Erik", Lastname: "Ivanov", Premium: true, Age: 19, Friends: friends}

		// Mostrar el template y pasar la escructura user
		templateTODOS.ExecuteTemplate(w, "bucles", user)
	})

	http.ListenAndServe(":8080", nil)
}

```



## Servidor archivos estáticos <a name="server-estatic"></a>

```go
package main

import (
	"html/template"
	"net/http"
)

func main() {
	// Cargamos los archivos estaticos
	files := http.FileServer(http.Dir("../static"))
	// Los almacenamos en un handle
	http.Handle("/static/", http.StripPrefix("/static/", files))

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/html")
		temp, err := template.ParseFiles("../html/index.html")
		if err != nil {
			panic(err)
		}

		// Se muestra el template
		temp.Execute(w, nil)
	})

	http.ListenAndServe(":8080", nil)
}

```

