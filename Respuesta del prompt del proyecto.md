Esta es una guía estructurada para inicializar tu entorno de desarrollo y construir la aplicación de panadería utilizando la metodología de agentes globales.

---

## 1. Estructura del Agente Global `.agents`

Para que tu sistema reconozca las habilidades de diseño, código y scraping, organizamos el archivo maestro de la siguiente manera:

**Ruta sugerida:** `~/.agents/SKILL.md`

```markdown
# Global Agent Skill: Bakery Automation
## Estructura de Carpetas
- /scripts: Automatización de deploys y limpieza.
- /ejemplos: Templates de UI y modelos de datos.
- /resources: Assets (iconos de panadería, logos).

## Skills Activos
1. **Skill de Diseño**: Manejo de Material Design 3, paleta de colores café/trigo.
2. **Skill de Código**: Arquitectura limpia (Clean Architecture) en Dart.
3. **Skill de Scraping**: Extracción de precios de competencia para actualización de productos.
```

---

## 2. Prerrequisitos y Configuración del Entorno

### Verificación de Herramientas
Ejecuta estos comandos en tu terminal (VS Code o Antigravity):

```bash
# 1. Verificar Flutter
flutter --version

# 2. Instalar FlutterFire CLI (si no lo tienes)
dart pub global activate flutterfire_cli

# 3. Login en Firebase
firebase login

# 4. Configurar el proyecto con Firebase
# Corre esto dentro de tu carpeta 'proyectopanaderia'
flutterfire configure
```

### Configuración del `pubspec.yaml`
Asegúrate de tener estas dependencias para el CRUD y Firebase:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.x.x
  cloud_firestore: ^4.x.x
  cupertino_icons: ^1.0.2
```

---

## 3. Arquitectura del Proyecto: `proyectopanaderia`

### Estructura de Archivos
```text
proyectopanaderia/
├── lib/
│   ├── main.dart
│   ├── models/
│   │   └── producto_model.dart
│   ├── screens/
│   │   ├── home_screen.dart
│   │   ├── product_form_screen.dart
│   │   └── product_list_screen.dart
│   └── services/
│       └── firebase_service.dart
└── pubspec.yaml
```

### Código: Modelo de Producto (`lib/models/producto_model.dart`)
```dart
class Producto {
  String id;
  String nombre;
  double precio;
  int stock;

  Producto({required this.id, required this.nombre, required this.precio, required this.stock});

  Map<String, dynamic> toMap() => {
    "nombre": nombre,
    "precio": precio,
    "stock": stock,
  };
}
```

### Código: Servicio CRUD (`lib/services/firebase_service.dart`)

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

final FirebaseFirestore _db = FirebaseFirestore.instance;

// CREATE
Future<void> addProducto(String nombre, double precio, int stock) async {
  await _db.collection('productos').add({
    'nombre': nombre,
    'precio': precio,
    'stock': stock,
  });
}

// READ (Stream para tiempo real)
Stream<QuerySnapshot> getProductos() {
  return _db.collection('productos').snapshots();
}

// UPDATE
Future<void> updateProducto(String id, String nuevoNombre, double nuevoPrecio) async {
  await _db.collection('productos').doc(id).update({
    'nombre': nuevoNombre,
    'precio': nuevoPrecio,
  });
}

// DELETE
Future<void> deleteProducto(String id) async {
  await _db.collection('productos').doc(id).delete();
}
```

---

## 4. UI: Pantalla de Listado y Navegación (`lib/screens/product_list_screen.dart`)

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';

class ProductListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Inventario Panadería")),
      body: StreamBuilder(
        stream: getProductos(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return CircularProgressIndicator();
          var docs = snapshot.data!.docs;
          return ListView.builder(
            itemCount: docs.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(docs[index]['nombre']),
                subtitle: Text("\$${docs[index]['precio']}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => deleteProducto(docs[index].id),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => _mostrarFormulario(context),
        child: Icon(Icons.add),
      ),
    );
  }
}
```

---

## 5. Secuencia Lógica de Implementación (Flujo Agente)

1.  **Fase de Verificación**: El agente revisa que `flutter doctor` no tenga errores y que la cuenta de Firebase esté vinculada.
2.  **Fase de Datos**: Se crea la colección `productos` en el Console de Firestore (Modo de prueba).
3.  **Fase de Construcción**: Generación de los archivos `.dart` proporcionados arriba.
4.  **Fase de Pruebas**: Ejecución en emulador Android o dispositivo físico para verificar que el botón "Guardar" refleje los datos en la consola de Firebase inmediatamente.

> **Nota sobre el "Proyecto Veterinario"**: Mencionaste al final un proyecto veterinario. Si deseas cambiar el contexto de la panadería a una veterinaria, el agente simplemente debe mapear la colección `productos` a `mascotas` y los campos `precio/stock` a `raza/edad`. ¿Deseas que ajustemos los modelos para el entorno veterinario ahora mismo?
