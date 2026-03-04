# 🚀 English Spelling Practice — Guía de Despliegue

## Estructura del proyecto

```
english-practice/
├── public/
│   └── index.html          ← La app completa
├── firebase.json            ← Config de Firebase Hosting
├── firestore.rules          ← Reglas de seguridad Firestore
├── firestore.indexes.json   ← Índices de Firestore
├── .firebaserc              ← ID de tu proyecto Firebase
├── .gitignore
└── README.md
```

---

## PASO 1 — Configurar Firebase

### 1.1 Ir a la consola de Firebase
Abre: https://console.firebase.google.com y selecciona tu proyecto.

### 1.2 Activar Firestore
1. En el menú lateral, ve a **"Firestore Database"**
2. Clic en **"Crear base de datos"**
3. Selecciona **"Modo de prueba"** (o modo producción si quieres más seguridad)
4. Elige la región más cercana (ej: `us-central1`)
5. Clic en **"Crear"**

### 1.3 Obtener la configuración de Firebase
1. Ve a **Configuración del proyecto** (ícono de engranaje ⚙️)
2. En la sección **"Tus apps"**, clic en el ícono **Web** (`</>`)
3. Registra la app con un nombre (ej: "english-practice")
4. Copia los valores del objeto `firebaseConfig`

### 1.4 Pegar la configuración en el código
Abre `public/index.html` y busca esta sección (aproximadamente línea 330):

```javascript
const firebaseConfig = {
  apiKey: "TU_API_KEY",                          // ← Reemplaza
  authDomain: "TU_PROJECT.firebaseapp.com",      // ← Reemplaza
  projectId: "TU_PROJECT_ID",                    // ← Reemplaza
  storageBucket: "TU_PROJECT.appspot.com",       // ← Reemplaza
  messagingSenderId: "123456789",                // ← Reemplaza
  appId: "TU_APP_ID"                             // ← Reemplaza
};
```

### 1.5 Configurar .firebaserc
Abre `.firebaserc` y reemplaza con tu Project ID:

```json
{
  "projects": {
    "default": "tu-project-id-real"
  }
}
```

---

## PASO 2 — Subir a Git

```bash
# Ir a la carpeta del proyecto
cd english-practice

# Inicializar Git
git init

# Agregar todos los archivos
git add .

# Primer commit
git commit -m "English Spelling Practice - primera versión"

# Conectar con tu repositorio en GitHub
git remote add origin https://github.com/TU_USUARIO/english-practice.git

# Subir
git branch -M main
git push -u origin main
```

> 💡 Si no tienes el repo en GitHub, créalo primero en https://github.com/new

---

## PASO 3 — Instalar Firebase CLI y desplegar

### 3.1 Instalar Firebase CLI
```bash
npm install -g firebase-tools
```

### 3.2 Iniciar sesión
```bash
firebase login
```

### 3.3 Desplegar
```bash
cd english-practice

# Despliega hosting + reglas de Firestore
firebase deploy
```

### 3.4 ¡Listo!
Firebase te dará una URL como:
```
https://tu-project-id.web.app
```

¡Esa es tu app en vivo! 🎉

---

## Cómo funciona la app

### 🔒 Patrón de desbloqueo
- La primera vez que entras, creas un patrón (mínimo 4 puntos)
- Lo confirmas dibujándolo dos veces
- Cada vez que vuelvas, dibujas tu patrón para entrar
- El patrón genera un ID único que se usa para guardar tus datos en Firestore
- Botón "Borrar patrón" para empezar de nuevo si lo olvidas

### 📝 Práctica
- Agrega palabras una por una o pega una lista completa
- La traducción al español se genera automáticamente
- Escribe la palabra en inglés de memoria y presiona Enter
- Verde = correcto, Rojo = incorrecto
- La palabra original está oculta (borrosa) hasta que verificas

### 🏆 Aprendidas
- Las palabras que escribes correctamente se pueden mover aquí
- Se guarda la fecha en que las aprendiste

### ☁️ Base de datos
- Todo se guarda automáticamente en Firebase Firestore
- Puedes acceder desde cualquier navegador con tu patrón
- Se muestra un indicador "☁️ Guardando..." cada vez que se sincronizan datos

---

## Seguridad

Las reglas actuales de Firestore (`firestore.rules`) permiten lectura/escritura abierta.
Para producción, puedes cambiarlas a:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.resource.data.keys().hasAll(['words', 'learned']);
    }
  }
}
```

---

## Notas

- La traducción usa la API de Claude (funciona dentro de claude.ai).
  Si la usas fuera de claude.ai, necesitarás agregar tu API key de Anthropic.
- Los datos se guardan en Firestore bajo `users/{patron_hash}`
- El patrón se guarda localmente (localStorage) como hash — no se puede revertir
