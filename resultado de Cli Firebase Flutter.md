Para integrar Firebase en Flutter de manera profesional en 2026, el uso de **Firebase CLI** es el estándar de oro, ya que automatiza la configuración de archivos nativos. Aquí tienes la guía técnica detallada para preparar tu entorno en Windows.

---

## 1. Software Necesario: Node.js y npm
Para usar Firebase CLI, necesitas el motor de ejecución **Node.js**, el cual incluye automáticamente **npm** (Node Package Manager).

### Verificación de Instalación
Antes de instalar nada, abre una terminal (PowerShell o CMD) y ejecuta:
```bash
node -v
npm -v
```
* **Si aparecen versiones (ej. v20.x.x):** Ya lo tienes.
* **Si dice "no se reconoce como un comando":** Sigue los pasos de abajo.

### Instalación paso a paso (Global en Windows)
1.  **Descarga:** Ve al sitio oficial [nodejs.org](https://nodejs.org/) y descarga la versión **LTS** (Long Term Support).
2.  **Instalador:** Ejecuta el archivo `.msi`. 
3.  **Configuración:** * Acepta los términos.
    * **Importante:** Asegúrate de que la opción **"Add to PATH"** esté seleccionada (esto permite que funcione de manera global).
    * Marca la casilla para instalar "Tools for Native Modules" si planeas hacer desarrollos complejos.
4.  **Finalizar:** Reinicia tu computadora para que las variables de entorno se actualicen.

---

## 2. Instalación de Firebase CLI (firebase-tools)
Una vez que `npm` está listo, instalamos las herramientas de Firebase de forma global para que estén disponibles en cualquier carpeta de tu proyecto.

### Comando de Instalación Global
Ejecuta este comando en tu terminal:
```bash
npm install -g firebase-tools
```
> **Nota:** El parámetro `-g` indica que la instalación es **global**.

---

## 3. Comandos Esenciales de Firebase CLI

Para que tu aplicación Flutter "hable" con Firebase, debes seguir este flujo de comandos:

### A. Acceder con tu Cuenta de Google
Este comando abrirá una ventana en tu navegador para que autorices el acceso.
```bash
firebase login
```
* **Tip:** Si ya habías iniciado sesión y quieres cambiar de cuenta, usa `firebase logout` primero.

### B. Verificar Conexión
Para ver la lista de proyectos que tienes creados en la consola de Firebase:
```bash
firebase projects:list
```

### C. Vincular con Flutter (FlutterFire CLI)
Aunque ya tienes `firebase-tools`, Flutter requiere un paso adicional para generar el archivo `firebase_options.dart`. Primero, instala el activador de FlutterFire:
```bash
dart pub global activate flutterfire_cli
```
Luego, dentro de tu carpeta `crudpapeleria`, ejecuta:
```bash
flutterfire configure
```
Este comando te permitirá seleccionar tu proyecto de la consola y configurará automáticamente Android, iOS y Web por ti.

---

## 4. Resumen de Flujo de Trabajo
Para el proyecto de la papelería, el orden lógico de ejecución es el siguiente:

1.  **Instalar Node.js** (Motor).
2.  **npm install -g firebase-tools** (Herramienta de control).
3.  **firebase login** (Vinculación de identidad).
4.  **flutterfire configure** (Generación de código de enlace en Flutter).



### ¿Qué versión utilizar?
En este momento, te recomiendo trabajar con **Node.js v20 LTS** o superior y una versión de **npm 10.x.x**, ya que ofrecen la mayor estabilidad con las librerías actuales de Flutter y Firebase.

¿Deseas que te proporcione el comando específico para solucionar errores de "scripts deshabilitados" en la PowerShell de Windows, que suele ser el error más común en este paso?
