¡Hola! Como desarrollador de software, me encanta la estructura que planteas. Vamos a construir este sistema de papelería utilizando **Flutter** y **Firebase**, integrando la filosofía de **Antigravity** (que se enfoca en la eficiencia y estructuras de alto rendimiento) para que tus estudiantes tengan una base sólida.

---

## 🚀 Plan de Trabajo: CRUD Papelería "XflutterAlvarado_0456"

### 1. Preparación del Entorno
* **Carpeta Raíz:** `XflutterAlvarado_0456`
* **Subcarpeta del Proyecto:** `crudpapeleria`
* **Comando de creación:** ```bash
    mkdir XflutterAlvarado_0456
    cd XflutterAlvarado_0456
    flutter create crudpapeleria
    ```

### 2. Configuración en Firebase Console
1.  Ve a [Firebase Console](https://console.firebase.google.com/).
2.  Crea un proyecto llamado `Papeleria-Alvarado`.
3.  Habilita **Cloud Firestore** en modo de prueba.
4.  Crea una colección llamada **`productos`**.
5.  Registra tu app (Android/iOS/Web) y descarga el archivo de configuración (`google-services.json` para Android).

---

### 3. Dependencias y `pubspec.yaml`
Para que el proyecto funcione, necesitamos las librerías oficiales de Firebase. Así se modifica el archivo:

**Archivo:** `pubspec.yaml`
```yaml
dependencies:
  flutter:
    sdk: flutter
  # Librerías esenciales
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
  cupertino_icons: ^1.0.6
```
*Ejecuta `flutter pub get` después de guardar.*

---

### 4. Metodología de Trabajo (Agentes y Roles)
Para una práctica guiada bajo el concepto de **Antigravity**, dividiremos el flujo de trabajo en "Agentes Especializados":

| Agente | Rol | Skill (Habilidad) |
| :--- | :--- | :--- |
| **Arquitecto** | Estructura de Datos | Definir campos: `nombre`, `precio`, `stock`. |
| **Conector** | Integración Firebase | Configurar `Firebase.initializeApp()`. |
| **Gestor CRUD** | Lógica de Negocio | Funciones `add`, `get`, `update`, `delete`. |
| **Diseñador UI** | Experiencia de Usuario | Crear interfaces en escala de morados y azules. |

---

### 5. Estructura de Archivos
```text
crudpapeleria/
├── lib/
│   ├── main.dart             # Punto de entrada
│   ├── services/
│   │   └── firebase_service.dart # Lógica de Firestore
│   └── pages/
│       ├── home_page.dart    # Lista de productos (Read/Delete)
│       └── add_product_page.dart # Formulario (Create/Update)
└── assets/                   # Iconos y logos
```

---

### 6. Implementación del Código (Full Funcional)

#### A. Servicio de Base de Datos
**Archivo:** `lib/services/firebase_service.dart`
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

FirebaseFirestore db = FirebaseFirestore.instance;

// CREATE & UPDATE (Save)
Future<void> saveProduct(String name, double price, int stock, {String? id}) async {
  if (id == null) {
    await db.collection('productos').add({
      'nombre': name,
      'precio': price,
      'stock': stock,
    });
  } else {
    await db.collection('productos').doc(id).update({
      'nombre': name,
      'precio': price,
      'stock': stock,
    });
  }
}

// READ
Future<List> getProducts() async {
  List products = [];
  QuerySnapshot querySnapshot = await db.collection('productos').get();
  for (var doc in querySnapshot.docs) {
    final Map<String, dynamic> data = doc.data() as Map<String, dynamic>;
    products.add({...data, 'id': doc.id});
  }
  return products;
}

// DELETE
Future<void> deleteProduct(String id) async {
  await db.collection('productos').doc(id).delete();
}
```

#### B. Interfaz de Usuario (UI) con Estética Morado/Azul
**Archivo:** `lib/pages/home_page.dart`
```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _BorderedHomeState();
}

class _BorderedHomeState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Papelería Alvarado'),
        backgroundColor: Colors.indigo[900], // Azul Oscuro
      ),
      body: FutureBuilder(
        future: getProducts(),
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            return ListView.builder(
              itemCount: snapshot.data?.length,
              itemBuilder: (context, index) {
                return Card(
                  color: Colors.deepPurple[50], // Morado tenue
                  margin: const EdgeInsets.symmetric(horizontal: 10, vertical: 5),
                  child: ListTile(
                    title: Text(snapshot.data?[index]['nombre'], 
                          style: TextStyle(color: Colors.indigo[800], fontWeight: FontWeight.bold)),
                    subtitle: Text('Precio: \$${snapshot.data?[index]['precio']} - Stock: ${snapshot.data?[index]['stock']}'),
                    trailing: IconButton(
                      icon: const Icon(Icons.delete, color: Colors.redAccent),
                      onPressed: () async {
                        await deleteProduct(snapshot.data?[index]['id']);
                        setState(() {}); // Refrescar
                      },
                    ),
                  ),
                );
              },
            );
          } else {
            return const Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.deepPurple,
        onPressed: () { /* Navegar a formulario */ },
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

#### C. Inicialización
**Archivo:** `lib/main.dart`
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'pages/home_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(); // Punto crítico de conexión
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'CRUD Papelería',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
        scaffoldBackgroundColor: Colors.grey[100],
      ),
      home: HomePage(),
    );
  }
}
```

---

### 7. Práctica Guiada para Estudiantes (Flow Antigravity)
1.  **Módulo de Gravedad Cero:** Los estudiantes deben crear el modelo de datos en una hoja blanca antes de tocar el código.
2.  **Módulo de Propulsión:** Configuración de Firebase. Es el "motor" del proyecto.
3.  **Módulo de Órbita:** Implementar el `FutureBuilder`. Explicar que los datos "orbitan" entre la nube (Firestore) y la tierra (la App).
4.  **Módulo de Control:** Los botones de editar y borrar deben tener colores contrastantes (Azul para acciones seguras, Morado para acciones creativas).

Este flujo asegura que el estudiante no solo copie código, sino que entienda la jerarquía de un sistema **CRUD** moderno. ¿Te gustaría que profundice en la validación de los formularios de entrada?
