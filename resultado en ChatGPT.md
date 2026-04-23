# 🟣📘 PROYECTO: CRUD PAPELERÍA EN FLUTTER + FIREBASE

Guía completa paso a paso (modo práctica para estudiantes)

---

# 1. 📁 ESTRUCTURA DE CARPETAS

Crear manualmente:

```
XflutterAlvarado_0456/
 └── crudpapeleria/
```

Luego dentro ejecutas:

```
flutter create crudpapeleria
```

Estructura final:

```
crudpapeleria/
 ├── lib/
 │   ├── main.dart
 │   ├── screens/
 │   │   ├── home_screen.dart
 │   │   ├── add_product.dart
 │   │   └── edit_product.dart
 │   ├── services/
 │   │   └── firestore_service.dart
 │   └── widgets/
 │       └── product_card.dart
 ├── pubspec.yaml
```

---

# 2. ⚡ TRABAJAR CON ANTIGRAVITY

Antigravity se usa como apoyo para:

* Generar interfaces
* Optimizar código
* Crear componentes UI

Prompt sugerido:

"""
Crea una app Flutter con Firebase Firestore tipo CRUD para una papelería.
Usa colores morados y azules, diseño moderno, tarjetas elegantes.
Colección: productos (nombre, precio, stock).
Pantalla principal con lista, botón para agregar, editar y eliminar.
"""

---

# 3. 🔥 CONFIGURAR FIREBASE

1. Ir a: [https://console.firebase.google.com/](https://console.firebase.google.com/)
2. Crear proyecto
3. Agregar app Android
4. Descargar google-services.json
5. Colocarlo en:

```
android/app/
```

---

# 4. 📦 LIBRERÍAS NECESARIAS

Agregar en pubspec.yaml:

```
dependencies:
  flutter:
    sdk: flutter

  firebase_core: ^2.30.0
  cloud_firestore: ^4.15.0
```

---

# 5. ⚙️ CONFIGURAR pubspec.yaml

Ejemplo completo:

```
name: crudpapeleria

dependencies:
  flutter:
    sdk: flutter

  firebase_core: ^2.30.0
  cloud_firestore: ^4.15.0
```

Luego ejecutar:

```
flutter pub get
```

---

# 6. 🧠 INICIALIZAR FIREBASE

### main.dart

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Papelería Cometa',
      theme: ThemeData(
        primarySwatch: Colors.deepPurple,
      ),
      home: HomeScreen(),
    );
  }
}
```

---

# 7. 🔥 SERVICIO FIRESTORE (CRUD)

### firestore_service.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirestoreService {
  final CollectionReference productos =
      FirebaseFirestore.instance.collection('productos');

  // CREATE
  Future addProducto(String nombre, double precio, int stock) {
    return productos.add({
      'nombre': nombre,
      'precio': precio,
      'stock': stock,
    });
  }

  // READ
  Stream<QuerySnapshot> getProductos() {
    return productos.snapshots();
  }

  // UPDATE
  Future updateProducto(String id, String nombre, double precio, int stock) {
    return productos.doc(id).update({
      'nombre': nombre,
      'precio': precio,
      'stock': stock,
    });
  }

  // DELETE
  Future deleteProducto(String id) {
    return productos.doc(id).delete();
  }
}
```

---

# 8. 🏠 HOME SCREEN (LISTAR)

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import 'add_product.dart';
import 'edit_product.dart';

class HomeScreen extends StatelessWidget {
  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Papelería Cometa')),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.deepPurple,
        child: Icon(Icons.add),
        onPressed: () {
          Navigator.push(context,
              MaterialPageRoute(builder: (_) => AddProduct()));
        },
      ),
      body: StreamBuilder(
        stream: service.getProductos(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return CircularProgressIndicator();

          var docs = snapshot.data!.docs;

          return ListView.builder(
            itemCount: docs.length,
            itemBuilder: (context, index) {
              var data = docs[index];

              return Card(
                color: Colors.blue.shade100,
                child: ListTile(
                  title: Text(data['nombre']),
                  subtitle: Text(
                      'Precio: ${data['precio']} | Stock: ${data['stock']}'),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(
                        icon: Icon(Icons.edit, color: Colors.purple),
                        onPressed: () {
                          Navigator.push(
                            context,
                            MaterialPageRoute(
                              builder: (_) => EditProduct(doc: data),
                            ),
                          );
                        },
                      ),
                      IconButton(
                        icon: Icon(Icons.delete, color: Colors.red),
                        onPressed: () {
                          service.deleteProducto(data.id);
                        },
                      ),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

# 9. ➕ AGREGAR PRODUCTO

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';

class AddProduct extends StatelessWidget {
  final _nombre = TextEditingController();
  final _precio = TextEditingController();
  final _stock = TextEditingController();

  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Agregar Producto')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(controller: _nombre, decoration: InputDecoration(labelText: 'Nombre')),
            TextField(controller: _precio, decoration: InputDecoration(labelText: 'Precio')),
            TextField(controller: _stock, decoration: InputDecoration(labelText: 'Stock')),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                service.addProducto(
                  _nombre.text,
                  double.parse(_precio.text),
                  int.parse(_stock.text),
                );
                Navigator.pop(context);
              },
              child: Text('Guardar'),
            )
          ],
        ),
      ),
    );
  }
}
```

---

# 10. ✏️ EDITAR PRODUCTO

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';

class EditProduct extends StatelessWidget {
  final doc;

  EditProduct({required this.doc});

  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    TextEditingController nombre = TextEditingController(text: doc['nombre']);
    TextEditingController precio = TextEditingController(text: doc['precio'].toString());
    TextEditingController stock = TextEditingController(text: doc['stock'].toString());

    return Scaffold(
      appBar: AppBar(title: Text('Editar Producto')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(controller: nombre),
            TextField(controller: precio),
            TextField(controller: stock),
            ElevatedButton(
              onPressed: () {
                service.updateProducto(
                  doc.id,
                  nombre.text,
                  double.parse(precio.text),
                  int.parse(stock.text),
                );
                Navigator.pop(context);
              },
              child: Text('Actualizar'),
            )
          ],
        ),
      ),
    );
  }
}
```

---

# 11. 🧩 METODOLOGÍA (AGENTES - ROLES - SKILLS)

### 🧠 AGENTES

* UI Agent → Diseña interfaces
* Backend Agent → Maneja Firebase
* QA Agent → Pruebas

### 🎭 ROLES

* Desarrollador Flutter
* Diseñador UI
* Tester

### 🛠 SKILLS

* Flutter widgets
* Firebase Firestore
* Navegación
* CRUD

### 🔄 FLUJO

1. Diseñar UI
2. Conectar Firebase
3. Crear CRUD
4. Probar
5. Mejorar UI

---

# 12. 🎨 ESTILO VISUAL

Colores sugeridos:

* Morado: #6A1B9A
* Azul: #0D47A1
* Fondo claro
* Cards suaves con sombras

---

# ✅ RESULTADO FINAL

✔ App funcional
✔ CRUD completo
✔ Conexión a Firebase
✔ Diseño moderno

---

Si quieres, en el siguiente paso puedo:
👉 Convertir esto en una práctica tipo guía de laboratorio (con preguntas y evaluación)
👉 O mejorar el diseño tipo app profesional como Webtoon/tienda real
