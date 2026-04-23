¡Hola! Como desarrollador de software, he diseñado este plan de trabajo estructurado para que tus estudiantes no solo sigan un tutorial, sino que entiendan la arquitectura detrás de una integración profesional entre **Flutter** y **Firebase**.

---

## 🚀 Plan de Trabajo: Proyecto `applemusiccrud`

### 1. Preparación del Entorno
* **Estructura de Directorios:**
    1.  Crea la carpeta raíz: `crudreyes1299`.
    2.  Dentro de ella, inicializa el proyecto Flutter:
        ```bash
        mkdir crudreyes1299
        cd crudreyes1299
        flutter create applemusiccrud
        ```

### 2. Configuración de Firebase (Consola)
1.  Ve a [Firebase Console](https://console.firebase.google.com/).
2.  Crea un nuevo proyecto llamado `AppleMusicCRUD`.
3.  Registra tu app (Android/iOS) usando el ID del paquete.
4.  **Firestore:** Ve a "Build" > "Firestore Database" > "Create Database".
    * Crea la colección: `usuarios`.
    * Agrega un documento de prueba con los campos: `nombre` (string), `apellido` (string), `correo` (string).

---

## 🛠️ Integración de Librerías y Dependencias

Para implementar este proyecto, editamos el archivo `pubspec.yaml`. Usaremos **Antigravity** (un enfoque de arquitectura limpia y minimalista) junto con los plugins oficiales.

### Cómo agregar `firebase_core`:
En la sección `dependencies`, añade las siguientes líneas:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.24.2
  cloud_firestore: ^4.14.0
  # Otras librerías sugeridas para diseño moderno:
  google_fonts: ^6.1.0
  flutter_spinkit: ^5.2.0
```

---

## 🏗️ Implementación del CRUD (Arquitectura Antigravity)

Bajo el concepto de **Antigravity**, buscamos que el código "flote" libre de dependencias pesadas, separando la lógica de la UI.

### Estructura de Archivos (📁 `lib/`)
* `main.dart`: Punto de entrada e inicialización.
* `services/firebase_service.dart`: Lógica pura de Firestore (Agente de datos).
* `pages/home_page.dart`: Lista de usuarios (Lectura y Borrado).
* `pages/add_user_page.dart`: Formulario (Creación).
* `pages/edit_user_page.dart`: Formulario (Actualización).

---

### Código: `services/firebase_service.dart` (El Agente de Datos)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

FirebaseFirestore db = FirebaseFirestore.instance;

// LEER
Future<List> getUsuarios() async {
  List usuarios = [];
  QuerySnapshot querySnapshot = await db.collection('usuarios').get();
  for (var doc in querySnapshot.docs) {
    final Map<String, dynamic> data = doc.data() as Map<String, dynamic>;
    final person = {
      "uid": doc.id,
      "nombre": data['nombre'],
      "apellido": data['apellido'],
      "correo": data['correo'],
    };
    usuarios.add(person);
  }
  return usuarios;
}

// GUARDAR
Future<void> addUsuario(String nombre, String apellido, String correo) async {
  await db.collection('usuarios').add({
    "nombre": nombre,
    "apellido": apellido,
    "correo": correo,
  });
}

// ACTUALIZAR
Future<void> updateUsuario(String uid, String nNombre, String nApellido, String nCorreo) async {
  await db.collection('usuarios').doc(uid).set({
    "nombre": nNombre,
    "apellido": nApellido,
    "correo": nCorreo,
  });
}

// BORRAR
Future<void> deleteUsuario(String uid) async {
  await db.collection('usuarios').doc(uid).delete();
}
```

---

### Código: UI Atractiva (`main.dart` & `home_page.dart`)
Utilizaremos una paleta de colores **Apple Style**: Fondos oscuros, acentos en rojo vibrante y tarjetas blancas con bordes redondeados.

```dart
// Fragmento de la Interfaz Principal (Home)
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Apple Music Fans', style: TextStyle(color: Colors.white)),
      backgroundColor: const Color(0xFF1C1C1E), // Dark Mode Apple
    ),
    body: FutureBuilder(
      future: getUsuarios(),
      builder: (context, snapshot) {
        if (snapshot.hasData) {
          return ListView.builder(
            itemCount: snapshot.data?.length,
            itemBuilder: (context, index) {
              return Card(
                elevation: 4,
                margin: const EdgeInsets.symmetric(horizontal: 15, vertical: 8),
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(15)),
                child: ListTile(
                  title: Text("${snapshot.data?[index]['nombre']} ${snapshot.data?[index]['apellido']}"),
                  subtitle: Text(snapshot.data?[index]['correo']),
                  trailing: const Icon(Icons.edit, color: Color(0xFFFA233B)), // Rojo Apple
                  onTap: () async {
                    await Navigator.pushNamed(context, '/edit', arguments: snapshot.data?[index]);
                    setState(() {});
                  },
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
      backgroundColor: const Color(0xFFFA233B),
      onPressed: () => Navigator.pushNamed(context, '/add'),
      child: const Icon(Icons.add),
    ),
  );
}
```

---

## 🤖 Metodología: Agentes, Roles y Skills

Para un flujo de trabajo moderno en el aula, dividimos el desarrollo en **Agentes**:

| Agente | Rol | Skills (Habilidades) |
| :--- | :--- | :--- |
| **Cloud Agent** | Administrador Firebase | Configuración de consola, reglas de seguridad de Firestore. |
| **Logic Agent** | Desarrollador Backend | CRUD, Manejo de `Future` y `Async/Await`. |
| **Experience Agent** | Diseñador UX/UI | Manejo de Widgets, colores complementarios, fuentes. |
| **QA Agent** | Tester | Depuración, manejo de errores de conexión y validación de campos. |

### Flujo de Trabajo (Workflow):
1.  **Definición:** El Cloud Agent entrega el archivo `google-services.json`.
2.  **Estructura:** El Logic Agent crea los métodos en `firebase_service.dart`.
3.  **Visualización:** El Experience Agent conecta la UI con los métodos del Logic Agent.
4.  **Validación:** El QA Agent verifica que al borrar un usuario, la lista se actualice en tiempo real.

> **Nota para estudiantes:** El enfoque "Antigravity" significa que no dependemos de un estado global complejo para empezar; usamos la simplicidad de Flutter para que los datos fluyan orgánicamente desde la nube hacia el widget.

¿Deseas que profundicemos en el código específico para la validación de los campos de Correo y Apellido?
