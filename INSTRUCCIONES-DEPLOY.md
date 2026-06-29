# Guía de publicación — Diego Castillo Sitio Web
## GitHub + Netlify + Decap CMS (Admin Panel)

---

## PARTE 1 — Subir el sitio a GitHub

### Paso 1: Crear el repositorio

1. Ve a **github.com** e inicia sesión
2. Haz clic en el botón verde **"New"** (arriba a la izquierda)
3. Completa el formulario:
   - **Repository name:** `diego-castillo-web` (sin espacios ni tildes)
   - **Description:** `Sitio oficial Diego Castillo`
   - **Visibility:** selecciona **Public** (Netlify gratuito requiere repo público)
   - **NO** marques ningún checkbox de inicialización
4. Haz clic en **"Create repository"**

---

### Paso 2: Subir tus archivos

Tienes dos opciones. Elige la que te resulte más fácil:

---

#### OPCIÓN A — Subir desde el navegador (sin instalar nada, recomendada)

1. En la página de tu repositorio recién creado verás un mensaje
   que dice *"uploading an existing file"* — haz clic en ese enlace
2. Arrastra TODOS estos archivos y carpetas al área de carga:
   - `index.html`
   - `style.css`
   - `main.js`
   - La carpeta `IMG/` con todas tus fotos adentro
   - La carpeta `videos/` (si tienes videos locales)
3. En la sección **"Commit changes"** (abajo de la página):
   - Escribe: `Primer upload del sitio`
4. Haz clic en **"Commit changes"** (botón verde)

> ⚠️ GitHub no permite subir carpetas vacías desde el navegador.
> Si tu carpeta IMG está vacía, súbela cuando tengas al menos una foto adentro.

---

#### OPCIÓN B — Subir desde la terminal (si sabes usar Git)

```bash
# 1. En tu computador, abre Terminal y ve a la carpeta del sitio
cd ~/Downloads/diego-castillo-sitio

# 2. Inicializa Git
git init

# 3. Conecta con tu repositorio de GitHub
#    (reemplaza TU_USUARIO por tu usuario de GitHub)
git remote add origin https://github.com/TU_USUARIO/diego-castillo-web.git

# 4. Agrega todos los archivos
git add .

# 5. Primer commit
git commit -m "Primer upload del sitio"

# 6. Sube todo
git push -u origin main
```

---

### Paso 3: Verificar que todo esté correcto

1. Ve a **github.com/TU_USUARIO/diego-castillo-web**
2. Deberías ver los tres archivos: `index.html`, `style.css`, `main.js`
3. Si los ves, ✅ GitHub está listo. Continúa con la Parte 2.

---

## PARTE 2 — Publicar el sitio en Netlify

### Paso 4: Crear cuenta en Netlify

1. Ve a **netlify.com**
2. Haz clic en **"Sign up"**
3. Selecciona **"Sign up with GitHub"** — esto conecta ambas cuentas automáticamente
4. Autoriza a Netlify cuando GitHub te lo pida

---

### Paso 5: Crear tu sitio desde GitHub

1. En el dashboard de Netlify, haz clic en **"Add new site"**
2. Selecciona **"Import an existing project"**
3. Haz clic en **"Deploy with GitHub"**
4. Busca y selecciona tu repositorio **`diego-castillo-web`**
5. En la siguiente pantalla de configuración:
   - **Branch to deploy:** `main`
   - **Build command:** déjalo **vacío** (tu sitio es HTML puro, no necesita build)
   - **Publish directory:** déjalo **vacío** o escribe `.`
6. Haz clic en **"Deploy site"**

---

### Paso 6: Esperar el deploy

1. Netlify tardará entre 30 segundos y 2 minutos
2. Verás un mensaje **"Site is live"** con un link como:
   `https://nombre-aleatorio-123.netlify.app`
3. Haz clic en ese link — tu sitio ya está publicado en internet ✅

---

### Paso 7: Cambiar el nombre del sitio (opcional pero recomendado)

1. En el dashboard de Netlify, ve a **Site configuration → Site details**
2. Haz clic en **"Change site name"**
3. Escribe: `diego-castillo` (si está disponible)
4. Tu sitio quedará en: `https://diego-castillo.netlify.app`

> 💡 Si tienes un dominio propio (ej: diegocastillo.cl), puedes conectarlo
> desde **Domain management → Add custom domain**. Netlify te guía paso a paso.

---

## PARTE 3 — Instalar el Panel de Administración (Decap CMS)

### Paso 8: Crear los archivos del admin

Necesitas agregar una carpeta `admin` con dos archivos a tu repositorio.

#### Archivo 1: `admin/index.html`

Crea una carpeta llamada `admin` y dentro un archivo `index.html` con este contenido exacto:

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Admin — Diego Castillo</title>
</head>
<body>
  <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>
</body>
</html>
```

#### Archivo 2: `admin/config.yml`

Dentro de la misma carpeta `admin`, crea un archivo `config.yml` con este contenido:

```yaml
backend:
  name: github
  repo: TU_USUARIO/diego-castillo-web
  branch: main

media_folder: "IMG"
public_folder: "/IMG"

collections:
  - name: "shows"
    label: "Shows y Fechas"
    folder: "_data/shows"
    create: true
    slug: "{{slug}}"
    fields:
      - { label: "Nombre del evento", name: "title", widget: "string" }
      - { label: "Ciudad y lugar", name: "place", widget: "string" }
      - { label: "Día (ej: AGO)", name: "day", widget: "string" }
      - { label: "Mes/Año (ej: 2026)", name: "monthyr", widget: "string" }
      - label: "Estado"
        name: "status"
        widget: "select"
        options: ["Confirmado", "Por confirmar", "Agotado", "Cancelado", "Pasado"]
      - { label: "Link de entradas", name: "ticketLink", widget: "string", required: false }

  - name: "settings"
    label: "Configuración del sitio"
    files:
      - label: "Hero / Portada"
        name: "hero"
        file: "_data/hero.yml"
        fields:
          - { label: "Título", name: "title", widget: "string", default: "Diego Castillo" }
          - { label: "Tagline", name: "tagline", widget: "string" }
          - { label: "Eyebrow", name: "eyebrow", widget: "string" }
          - { label: "Foto de fondo", name: "image", widget: "image", required: false }
          - { label: "Texto botón principal", name: "btn1", widget: "string", default: "Reservar show" }
          - { label: "Texto botón secundario", name: "btn2", widget: "string", default: "Escuchar" }

      - label: "Single destacado"
        name: "music"
        file: "_data/music.yml"
        fields:
          - { label: "Título del single", name: "title", widget: "string" }
          - { label: "Tipo (Single / EP / Álbum)", name: "type", widget: "string" }
          - { label: "Año", name: "year", widget: "string" }
          - { label: "Portada", name: "cover", widget: "image", required: false }
          - { label: "Link Spotify", name: "spotify", widget: "string", required: false }
          - { label: "Link Apple Music", name: "apple", widget: "string", required: false }
          - { label: "Link YouTube", name: "youtube", widget: "string", required: false }

      - label: "Redes sociales"
        name: "socials"
        file: "_data/socials.yml"
        fields:
          - { label: "Instagram", name: "instagram", widget: "string", required: false }
          - { label: "YouTube", name: "youtube", widget: "string", required: false }
          - { label: "Spotify", name: "spotify", widget: "string", required: false }
          - { label: "Apple Music", name: "apple", widget: "string", required: false }
          - { label: "TikTok", name: "tiktok", widget: "string", required: false }

      - label: "Contacto y Booking"
        name: "contact"
        file: "_data/contact.yml"
        fields:
          - { label: "Correo de contacto", name: "email", widget: "string", required: false }
          - { label: "WhatsApp / Teléfono", name: "phone", widget: "string", required: false }
          - { label: "Nombre representante", name: "manager", widget: "string", required: false }
```

> ⚠️ IMPORTANTE: En la línea `repo:`, reemplaza `TU_USUARIO` por tu
> nombre de usuario real de GitHub. Por ejemplo:
> `repo: diegocastillo/diego-castillo-web`

---

### Paso 9: Subir la carpeta admin a GitHub

#### Opción A — Desde el navegador:

1. Ve a tu repositorio en GitHub
2. Haz clic en **"Add file" → "Create new file"**
3. En el campo del nombre escribe: `admin/index.html`
   (GitHub creará la carpeta automáticamente al escribir la barra `/`)
4. Pega el contenido del archivo `index.html` del admin
5. Haz clic en **"Commit changes"**
6. Repite para crear `admin/config.yml`

#### Opción B — Desde terminal:

```bash
cd ~/Downloads/diego-castillo-sitio
mkdir admin
# Crea los dos archivos con el contenido de arriba
git add admin/
git commit -m "Agrega panel de administración"
git push
```

---

### Paso 10: Activar Netlify Identity (el login del admin)

1. En tu dashboard de Netlify, haz clic en tu sitio
2. Ve a la pestaña **"Integrations"** → busca **"Identity"**
3. Haz clic en **"Enable Identity"**
4. En la misma sección, busca **"Git Gateway"** y haz clic en **"Enable Git Gateway"**
   (esto permite que el admin guarde cambios en GitHub desde el navegador)

---

### Paso 11: Crear tu usuario administrador

1. Todavía en Netlify, ve a **Identity → Invite users**
2. Escribe tu correo electrónico y haz clic en **"Send invite"**
3. Revisa tu correo — llegará una invitación de Netlify
4. Haz clic en el enlace del correo
5. Crea tu contraseña

---

### Paso 12: Entrar al panel de administración

1. Ve a: `https://tu-sitio.netlify.app/admin`
2. Inicia sesión con tu correo y contraseña
3. ¡El panel está listo! 🎉

---

## PARTE 4 — Actualizar el sitio desde ahora

### Cómo funciona el flujo desde ahora:

```
Tú editas en /admin  →  Netlify guarda en GitHub  →  Netlify publica automáticamente
```

Cada vez que guardes un cambio en el admin, Netlify detecta el cambio en GitHub
y republica el sitio en menos de 1 minuto. Sin tocar código.

### Para actualizar archivos HTML/CSS/JS manualmente:

1. Edita el archivo en tu computador
2. Ve a GitHub → tu repositorio → haz clic en el archivo
3. Haz clic en el ícono del lápiz (editar)
4. Pega el contenido nuevo
5. Haz clic en **"Commit changes"**
6. Netlify republica automáticamente en ~30 segundos

---

## Resumen de URLs importantes

| Qué                  | URL                                          |
|----------------------|----------------------------------------------|
| Tu sitio             | https://diego-castillo.netlify.app           |
| Panel admin          | https://diego-castillo.netlify.app/admin     |
| Tu repositorio       | https://github.com/TU_USUARIO/diego-castillo-web |
| Dashboard Netlify    | https://app.netlify.com                      |

---

## Solución de problemas frecuentes

**El sitio se ve en blanco o con errores de CSS:**
→ Verifica que `style.css` y `main.js` estén en la raíz del repositorio
  (mismo nivel que `index.html`), no dentro de subcarpetas.

**El admin dice "Not Found":**
→ Verifica que la carpeta `admin` con sus dos archivos esté en GitHub.
→ Espera 1–2 minutos y recarga.

**No puedo iniciar sesión en el admin:**
→ Asegúrate de haber activado **Identity** Y **Git Gateway** en Netlify (Paso 10).
→ Verifica que hayas aceptado la invitación del correo (Paso 11).

**Los cambios del admin no aparecen en el sitio:**
→ El sitio HTML actual no lee los archivos del CMS automáticamente.
→ Para conectarlos al 100% se necesita convertir el sitio a un generador
  estático (como Hugo o Eleventy). Puedo guiarte en ese paso cuando quieras.

---

*Guía generada para Diego Castillo — Junio 2026*
