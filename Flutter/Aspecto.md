## Cambiar la barra superior a transparente en Android

```dart
import 'package:flutter/services.dart';


// Dentro del build
SystemChrome.setSystemUIOverlayStyle(SystemUiOverlayStyle.light
        .copyWith(statusBarColor: Colors.transparent));
```

## Helper para cambiar el color de los iconos de la barra de notificaciones 

```dart
import 'package:flutter/services.dart' as services;

void cambiarStatusBarLight() {
  services.SystemChrome.setSystemUIOverlayStyle(
      services.SystemUiOverlayStyle.light);
}

void cambiarStatusBarDark() {
  services.SystemChrome.setSystemUIOverlayStyle(
      services.SystemUiOverlayStyle.dark);
}
```

