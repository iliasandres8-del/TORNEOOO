# Torneo Relámpago · Club Deportivo F-10

Sitio web del torneo: cuadro de eliminación directa (16 equipos / 15 partidos),
inscripción de equipos y jugadores, resultados, goleadores, tarjetas y valla
menos vencida — todo en vivo, conectado a Firebase.

- `index.html` → sitio público (el que ve cualquier persona).
- `admin.html` → panel privado para que la organización cargue equipos y resultados.
- No necesitas instalar nada ni programar: solo debes pegar tus propias claves
  de Firebase en un archivo y subir la carpeta a GitHub.

---

## 1. Crear el proyecto de Firebase

1. Entra a **https://console.firebase.google.com** e inicia sesión con tu cuenta de Google.
2. Clic en **"Crear un proyecto"**, ponle un nombre (ej. `torneo-f10`) y créalo.
3. Dentro del proyecto, en la página principal, clic en el ícono **`</>`** ("Web") para agregar una app web.
4. Ponle un apodo (ej. "sitio torneo") y clic en **"Registrar app"**. Firebase te mostrará un bloque de código con un objeto `firebaseConfig`.
5. Copia esos valores (apiKey, authDomain, projectId, etc.) y pégalos en el archivo:

   **`js/firebase-config.js`**

   reemplazando los valores de ejemplo (`TU_API_KEY`, `TU_PROYECTO`, etc.). Guarda el archivo.

## 2. Activar la base de datos (Firestore)

1. En el menú lateral izquierdo: **Compilación → Firestore Database**.
2. Clic en **"Crear base de datos"** → elige **"Modo producción"** → elige la ubicación más cercana (ej. `southamerica-east1` o `us-central`) → **"Habilitar"**.
3. Ve a la pestaña **"Reglas"** dentro de Firestore Database, borra lo que hay y pega el contenido del archivo **`firestore.rules`** (incluido en este proyecto). Clic en **"Publicar"**.

   Esto permite que cualquiera pueda **ver** el torneo, pero solo tú (con tu usuario admin) puedas **editar** equipos y resultados.

## 3. Crear tu usuario de administrador

1. En el menú lateral: **Compilación → Authentication**.
2. Clic en **"Comenzar"** → elige el proveedor **"Correo electrónico/contraseña"** → actívalo → **"Guardar"**.
3. Ve a la pestaña **"Users"** → **"Add user"** → escribe el correo y la contraseña con la que vas a entrar al panel admin (ej. `admin@f10.com`). Guárdalos bien, son los que usarás en `admin.html`.

No necesitas crear más usuarios: cualquiera que tenga ese correo y contraseña podrá administrar el torneo.

## 4. Probar el sitio en tu computador (opcional)

Los navegadores no siempre permiten abrir `index.html` con doble clic cuando usa `type="module"`. La forma más simple de probarlo localmente:

- Si tienes **Visual Studio Code**, instala la extensión "Live Server" y clic derecho sobre `index.html` → "Open with Live Server".
- O súbelo directo a GitHub Pages (paso 5) y pruébalo ya publicado.

## 5. Publicar en GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser público o privado).
2. Sube **todo el contenido de esta carpeta** (`index.html`, `admin.html`, `css/`, `js/`, `assets/`, etc.) a ese repositorio.
3. En el repositorio: **Settings → Pages**.
4. En "Source" elige la rama `main` y la carpeta `/ (root)` → **Save**.
5. Espera 1-2 minutos y GitHub te dará un link tipo `https://tu-usuario.github.io/tu-repo/`. Esa es la página pública del torneo.
6. El panel admin quedará en `https://tu-usuario.github.io/tu-repo/admin.html` (no aparece en ningún menú del sitio público, solo compártelo con quien organice el torneo).

> Nota: aunque el código quede visible en GitHub, tus datos están seguros:
> las reglas de Firestore (paso 2) son las que de verdad protegen quién puede
> escribir. La contraseña del admin nunca queda en el código, vive en Firebase
> Authentication.

## 6. Usar el panel admin

Entra a `admin.html`, inicia sesión con el correo/contraseña del paso 3.

**Pestaña "Cuadro":** primer paso, un solo clic en "Inicializar cuadro" para crear los 15 partidos vacíos (octavos, cuartos, semis, final).

**Pestaña "Equipos":** clic en "+ Nuevo equipo":
- Nombre del equipo.
- Semilla del 1 al 16: define en qué cruce de octavos queda el equipo (semilla 1 juega contra semilla 2, 3 contra 4, y así sucesivamente — igual que en el gráfico del cuadro).
- Agrega los jugadores uno por uno: nombre, número de camiseta, y marca "CAP" si es el capitán o "POR" si es el arquero (esto último es lo que permite calcular la valla menos vencida por equipo).

En cuanto guardas un equipo con semilla, aparece automáticamente en su casillero del cuadro, tanto en el panel admin como en la página pública.

**Pestaña "Partidos y resultados":** ahí aparecen los 15 partidos agrupados por ronda. Un partido solo se puede registrar cuando ya se conocen los dos equipos (los de rondas posteriores se llenan solos cuando termina el partido anterior). Clic en "Registrar resultado":
- Marcador de cada equipo.
- Si hay empate, se piden los penales (según el reglamento: 3 por equipo, luego muerte súbita).
- Goles: agrega cada gol con el equipo, el jugador (elige de la lista de jugadores de ese equipo) y el minuto (opcional).
- Tarjetas: igual, equipo, jugador, amarilla o roja, minuto.
- Al guardar, el ganador avanza automáticamente al siguiente partido del cuadro.

Todo esto se refleja **al instante** en la página pública (cuadro, goleadores, tarjetas, valla menos vencida) sin que nadie tenga que refrescar nada de tu lado.

## 7. Cambiar el escudo o los colores

- El escudo usado en el sitio está en `assets/logo.png`. Puedes reemplazar ese archivo por tu logo en alta calidad (mismo nombre) cuando lo tengas en formato PNG con fondo transparente.
- Los colores, tipografías y estilos generales están centralizados en `css/styles.css`, en la sección `:root` al principio del archivo (variables `--red`, `--gold`, `--black`, etc.).
- El texto de premios, fecha, hora y sede del torneo está directamente en `index.html`, dentro de la sección `<section class="hero ...">` — puedes editarlo ahí para el siguiente torneo sin tocar nada más.

---

### Estructura del proyecto

```
├── index.html          → sitio público
├── admin.html          → panel de administración
├── firestore.rules     → reglas de seguridad para pegar en Firebase
├── css/
│   └── styles.css      → todos los estilos (colores, tipografías, layout)
├── js/
│   ├── firebase-config.js → tus claves de Firebase (edítalo tú)
│   ├── db.js               → inicialización de Firebase
│   ├── bracket-data.js     → estructura fija de los 15 partidos
│   ├── app.js               → lógica del sitio público
│   └── admin.js             → lógica del panel admin
└── assets/
    └── logo.png          → escudo del club
```
