# 🚀 Guía de Despliegue — AssetTracker PRO con Firebase

---

## ¿Qué vas a tener al final?

- Una **URL pública** tipo `https://tu-proyecto.web.app` que cualquier persona puede abrir desde su celular, computador o tablet.
- Los datos guardados en **Firestore** (base de datos en la nube de Google, gratis hasta 1 GB).
- **Actualizaciones en tiempo real**: si alguien agrega un equipo, todos los que tienen la página abierta lo ven al instante.

---

## PASO 1 — Crear el proyecto en Firebase (5 min)

1. Ve a 👉 **https://console.firebase.google.com**
2. Haz clic en **"Agregar proyecto"**
3. Dale un nombre (ej: `assettracker-empresa`)
4. Desactiva Google Analytics si no lo necesitas → **Crear proyecto**

---

## PASO 2 — Activar Firestore (base de datos)

1. En el menú izquierdo → **Firestore Database**
2. Clic en **"Crear base de datos"**
3. Selecciona **"Comenzar en modo de prueba"** (funciona 30 días, suficiente para comenzar)
4. Elige la región **`us-central1`** (más cercana y económica) → **Listo**

> ⚠️ Después de 30 días debes actualizar las reglas en Firestore → Reglas.
> Puedes copiar el contenido del archivo `firestore.rules` incluido.

---

## PASO 3 — Obtener la configuración de Firebase

1. En la consola → **Configuración del proyecto** (ícono de engranaje ⚙️ arriba izquierda)
2. Scroll hacia abajo → sección **"Tus apps"**
3. Clic en **`</>`** (Web)
4. Registra la app con un nombre → **"Registrar app"**
5. Copia el objeto `firebaseConfig` que aparece, luce así:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "mi-proyecto.firebaseapp.com",
  projectId: "mi-proyecto",
  storageBucket: "mi-proyecto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

6. Abre el archivo **`dashboard.html`** y **reemplaza** el bloque `firebaseConfig` que dice `"PEGA_TU_..."` con tus valores reales.

---

## PASO 4 — Instalar Node.js y Firebase CLI

1. Instala **Node.js** desde 👉 https://nodejs.org (versión LTS)
2. Abre una terminal (PowerShell o CMD en Windows) y ejecuta:

```bash
npm install -g firebase-tools
```

3. Inicia sesión con tu cuenta de Google:

```bash
firebase login
```

---

## PASO 5 — Desplegar en Firebase Hosting

1. Abre la terminal **en la carpeta donde tienes los archivos** del proyecto (donde está `dashboard.html`)
2. Ejecuta:

```bash
firebase init hosting
```

Responde las preguntas así:

| Pregunta | Respuesta |
|----------|-----------|
| ¿Usar un proyecto existente? | Sí → selecciona el que creaste |
| Directorio público | `.` (un punto, indica la carpeta actual) |
| ¿Configurar como SPA? | **No** |
| ¿Sobreescribir index.html? | **No** |

3. Ahora despliega:

```bash
firebase deploy
```

4. Al terminar verás algo así:

```
✔  Deploy complete!

Hosting URL: https://assettracker-empresa.web.app
```

**¡Esa es tu URL pública! Compártela con quien quieras.** 🎉

---

## PASO 6 — Activar Hosting para Firestore (reglas)

Para que Firestore funcione desde la URL pública, ve a:

**Firebase Console → Firestore → Reglas** y pega esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /equipos/{docId} {
      allow read, write: if true;
    }
  }
}
```

Luego haz clic en **Publicar**.

---

## Estructura de archivos en tu carpeta

```
mi-proyecto/
├── dashboard.html       ← archivo principal (con tu firebaseConfig)
├── dashboard.css        ← estilos
├── firebase.json        ← config de hosting
├── firestore.rules      ← reglas de seguridad
└── GUIA_DESPLIEGUE.md   ← este archivo
```

---

## Comandos útiles

```bash
# Ver tu URL activa
firebase open hosting:site

# Re-desplegar después de cambios
firebase deploy

# Solo desplegar hosting (sin reglas)
firebase deploy --only hosting
```

---

## 🔒 Seguridad futura (recomendado)

Cuando no quieras que cualquier persona en internet pueda agregar/eliminar equipos, considera:

- Activar **Firebase Authentication** (login con Google o email/password)
- Actualizar las reglas de Firestore para requerir autenticación:

```
allow read, write: if request.auth != null;
```

---

## ¿Problemas? Contacta soporte

- Documentación Firebase: https://firebase.google.com/docs
- Estado de Firebase: https://status.firebase.google.com
