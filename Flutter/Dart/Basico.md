# Dart

Conceptos básicos

```dart
// Este es un comentario

// Darte necesita un main
// Las funciones inician con el tipo de dato que devuelven, en este caso no regresa nada
void main() {
    print('Hola '); // Se requiere un ;
}
```

Imprimir una variable

```dart
void main() {
	var nombre = 'Erik';
	
	print('Hola $nombre');
}
```

Declarar variable tipo String

```dart
void main() {
	String nombre;
	nombre = 'Erik';
	
	print('$nombre');
}
```

### Tipos de datos

- int
- double
- String (Es una clase)
- bool (true/false)



Los double necesita precisión

```dart
void main() {
	// double n = 1; // Incorrecto
    double n = 1.0; // Correcto
    print(double);
}
```

 Index de strings

```dart
void main() {
	String nombre = 'Erik';
	print(nombre);
	// Obtener primera letra
	print(nombre[0]);
	// Obtener la ultima letra
	print(nombre[nombre.length - 1]);
}
```

! negar

```dart
bool b = true;
print(!b); // False
```

### Listas

```dart
void main() {
    // Crea una coleccion de datos dinamicos
	List numeros = [1,23,3,4,5];
  	print(numeros);
    // Agregar dato
    numeros.add(6);
    print(numeros);
}
```

Lista de tipo definido

se pone List<tipo>

```dart
void main() {
	List<int> numeros = [1,23,3,4,5];
  print(numeros);
  // Agregar dato
  numeros.add(6);
  print(numeros);
}
```

Listas de tamaño definido 

```dart
void main() {
    List masNumeros = List(10);
    // masNumeros NO ESTA PERMITIDO
    print(masNumeros);
}
```

### Mapas

Estructura de datos de clave valor

```dart
void main() {
    Map<String, dynamic> p = {
      'nombre': 'Erik',
      'edad': 19,
      'soltero': true
    };
  
  print(p);
    
    //Acceder a los valores
    print(p['edad']);
}
```

### Funciones

tipo nombre(parámetros) {

​	Cuerpo

}

```dart
String saludar(String nombre) {
	return 'Hola $nombre';
}
```



```dart
String saludo3({String nombre, String saludo}) {
  return '$saludo $nombre';
}

// ASi se llama
print(saludo3(nombre: 'Erik', saludo:'Hola'));
```

Hay funciones parecidas al las funciones de flecha de javascript.  

```dart
void main() {
  print(saludar('Erik'));
}

String saludar(String nombre) => 'Hola $nombre';
```

### Clases

```dart
void main() {
  // Se acostumbra usar final
  final p1 = new Persona(nombre: 'Erik', apellido: 'Ivanov');
  print(p1);
  // Imprimir propiedades
  print(p1.nombre);
}

class Persona {
  String nombre;
  String apellido;

  // Construnctor con argumentos no posicionales
  Persona({String nombre, String apellido}) {
    this.nombre = nombre;
    this.apellido = apellido;
  }

  // Metodo para llamar a imprimir
  String toString() => '${this.nombre} ${this.apellido}';
  
}
}
```



```dart
void main() {
  // Se acostumbra usar final
  final p1 = new Persona(nombre: 'Erik', apellido: 'Ivanov');
  print(p1);
  // Imprimir propiedades
  print(p1.nombre);
}

class Persona {
  String nombre;
  String apellido;

  // Construnctor con argumentos no posicionales
//   Persona({String nombre, String apellido}) {
//     this.nombre = nombre;
//     this.apellido = apellido;
//   }
  
  // Otra forma de definir el constructor
  Persona({this.nombre, this.apellido});

  // Metodo para llamar a imprimir
  String toString() => '${this.nombre} ${this.apellido}';
  
}

```

Constructores con nombre

```dart
import 'dart:convert';

void main() {
  // JSON string
  final rawJSON = '{"nombre": "Erik", "apellido": "Ivanov"}';
  Map parseJSON = json.decode(rawJSON);

  print(parseJSON);
  
  // Crear instancia con el nuevo constructor
  final p = new Persona.fromJSON(parseJSON);
  print(p);
}

class Persona {
  String nombre;
  String apellido;

  Persona({this.nombre, this.apellido});
  Persona.fromJSON(Map parsedJSON) {
    nombre = parsedJSON['nombre'];
    apellido = parsedJSON['apellido'];
  }

  // Metodo para llamar a imprimir
  String toString() => '${this.nombre} ${this.apellido}';
}

```

Getters y Setters

```dart
void main() {
  final c = new Cuadrado();
  
  // Set
  c.lado = 10;
  // Get
  print(c.area);
}

class Cuadrado {
  double _lado; // Propiedad Privada
  
  set lado(double valor) {
    if (valor <= 0) {
      throw('El lado el menor o igual a 0');
    }
    
    _lado = valor;
  }
  
  //double get area {
  //  return _lado * _lado;
  //}
  double get area => _lado * _lado;
}
```

### Clases abstractas 

Obliga a seguir un patrón de diseño

```dart
void main() {
  final p = new Perro();
  
}


abstract class Animal {
  int patas;
  void emitirSonido();
}

// Obliga a implementar lo que marca animal
class Perro implements Animal {
  int patas;
  
  void emitirSonido() {
    print('Wow');
  }
}
```

### extends

Es para herencia

Importante: si queremos que no se puedan crear instancias usar abstract class

```dart
void main() {
  final a1 = new Estudiante();

  print(a1);
}

// No se puede crear una instancia de persona
abstract class Persona {
  String nombre;
  String apellido;
}

class Estudiante extends Persona {
  int numeroMaterias;
}

class Profesor extends Persona {
  int numeroAlumnos;
}

```

### mixing

Es para combinar varias clases.

```dart
abstract class Animal { }

abstract class Mamifero extends Animal {}

abstract class Ave extends Animal {}

abstract class Pez extends Animal {}

abstract class Volador {
  void volar() => print('Estoy volando.');
}

abstract class Caminante {
  void volar() => print('Estoy caminando.');
}

abstract class Nadador {
  void volar() => print('Estoy nadando.');
}


// Podemos cumbinar varias clases
class Gato extends Mamifero with Caminante {}

```

### Features

Es una tarea asíncrona que se realiza en un hilo aparte del principal.

Es como las promesas de Javascript.

```dart
void main() {
  print('Sumulacion de una peticion http');
  // con then resuelvo el feature
  httpGet('xD').then((data) {
    print(data);
  });
    print('Otra linea');
}

Future<String> httpGet(String url) {
  return Future.delayed(new Duration(seconds: 3), () {
    return 'Hola';
  });
}

```

### async await

Las funciones async son funciones que resuelven features

```dart
void main() async {
  print('Sumulacion de una peticion http');
  // con then resuelvo el feature
  String data = await httpGet('xD');
  print(data);

  print('Ultima linea');
}

Future<String> httpGet(String url) {
  return Future.delayed(new Duration(seconds: 3), () {
    return 'Hola';
  });
}

```

