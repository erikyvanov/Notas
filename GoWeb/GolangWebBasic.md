# Golang Web

## Contenidos

1. [Primer Servidor](#primer-servidor)
2. [URL y parametros](#url)
3. [Llamadas a Servidor](#llamada-servidor)



Para crear un servidor se necesita importar la librería net/http

``````go
import "net/http"
``````

Para recargar el servidor automáticamente hay que instalar

https://github.com/gravityblast/fresh

Usar el comando `fresh` dentro de la carpeta con el main.go

## Primer Servidor <a name="primer-servidor"> </a>



```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	fmt.Println("Primer servidor")

	// Agregar una ruta
	// Requiere el nombre de la ruta y una funcion con la firma
	// func(w http.ResponseWriter, r *http.Request)
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprint(w, "Home")
	})

	// Correr el servidor en el puerto 8080
	http.ListenAndServe(":8080", nil)
}
```

### URL y parámetros <a name="url"> </a>

````go
http.HandleFunc("/p", func(w http.ResponseWriter, r *http.Request) {
	// Imprime la URL en consola
	fmt.Println(r.URL)
})
````

Para obtener los parámetros  (Raw Query)

```go
http.HandleFunc("/p", func(w http.ResponseWriter, r *http.Request) {
	// Imprime la URL en consola
	fmt.Println("URL: ", r.URL)
	// Sacar los parametros
	fmt.Println("Raw Query: ", r.URL.RawQuery)
})

http.HandleFunc("/params", func(w http.ResponseWriter, r *http.Request) {
	// Con Query().Get("NombreParametro") podemos obtener el valor de un parametro
	fmt.Println("ID: ", r.URL.Query().Get("id"))
    fmt.Println("ID: ", r.URL.Query().Get("name"))
})
```

Agregar Parámetros

```go
http.HandleFunc("/AddParams", func(w http.ResponseWriter, r *http.Request) {
	// Agregar parametros
    mapa := r.URL.Query()
    mapa.Add("id", "0")
    mapa.Add("name", "Go")
    
	// Imprime la URL en consola
	fmt.Println("URL: ", r.URL)
    fmt.Println("Params: ", mapa.Encode())

})
```

Generar una URL

```go
package main

import (
	"fmt"
	"net/url"
)

func main() {
	params := make(map[string]string)
	params["id"] = "0"
	params["name"] = "Go"
	fmt.Println(GenerateURL("localhost:8080", "http", "/p", params))
}

// GenerateURL genera una URL
func GenerateURL(host string, scheme string, uri string, params map[string]string) string {
	// Crear URL
	u, err := url.Parse(uri)
	if err != nil {
		panic(err)
	}
	u.Host = host
	u.Scheme = scheme

	// Agregar parametros
	mapa := u.Query()
	for key, value := range params {
		mapa.Add(key, value)
	}
	u.RawQuery = mapa.Encode()

	return u.String()
}
```

### Llamada a servidor <a name="llamada-servidor"> </a>

Llamada a el primer servidor

````go
package main

import (
	"GoWebUdemy/utils"
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	url := utils.GenerateURL("localhost:8080", "http", "/", nil)
	fmt.Println(url)

	// Obtener el objeto *http.Request
	r, err := http.NewRequest("GET", url, nil)
	if err != nil {
		panic(err)
	}

	client := &http.Client{}
	// Hacemos la llamada
	response, err := client.Do(r)
	if err != nil {
		panic(err)
	}

	// Decodificar el body
	bytes, err := ioutil.ReadAll(response.Body)
	if err != nil {
		panic(err)
	}

	fmt.Println("Status:", response.Status)
	fmt.Println("Body:", string(bytes))
}
````

