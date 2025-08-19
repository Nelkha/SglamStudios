
# SGLAM — SPA React (Frontend) con integraciones Firebase, Cloudinary y Serverless


**🔗 Despliegue (demo en vivo):** https://www.sglamstudios.com.ar

> 📩 Nota para reclutadores: este repositorio público contiene únicamente este README y el enlace de despliegue. Si desea revisar el código fuente completo, por favor envíe un correo cordial a **marioagallego91@gmail.com** con su nombre, empresa y motivo; responderé con acceso temporal al repositorio privado o con un paquete del código según prefiera.

Aplicación SPA en React (Vite) para gestionar artistas, galerías, eventos y turnos, con backend serverless para tareas administrativas y envío de correos. Este README describe con precisión las integraciones y el comportamiento real detectado en el código.

## 🧭 Contenidos

- [Resumen](#resumen)
- [Características principales](#caracter%C3%ADsticas-principales)
- [Arquitectura y dependencias](#arquitectura-y-dependencias)
- [Flujo de turnos y comportamiento detallado](#flujo-de-turnos-y-comportamiento-detallado)
- [Variables de entorno detectadas](#variables-de-entorno-detectadas)
- [Endpoints y backend](#endpoints-y-backend)
- [Requisitos y ejecución local](#requisitos-y-ejecuci%C3%B3n-local)
- [Consideraciones de seguridad y despliegue](#consideraciones-de-seguridad-y-despliegue)
- [Qué contiene este repositorio (archivos relevantes)](#qu%C3%A9-contiene-este-repositorio-archivos-relevantes)
- [Cómo contribuir / pedir acceso al código completo](#c%C3%B3mo-contribuir--pedir-acceso-al-c%C3%B3digo-completo)
- [Licencia](#licencia)

---

## 📋 Resumen

SPA creada con React + Vite y Tailwind para la gestión de artistas, galerías y turnos.

El frontend usa Firebase (Firestore + Auth). Para subida de imágenes el proyecto integra Cloudinary. El backend incluye endpoints serverless de ejemplo (p. ej. `api/sendEmail.js`) y lógica de administración alojada en componentes y utilidades del proyecto.

---



## ✨ Características principales

Descripción breve: aplicación SPA para gestionar una comunidad artística (artistas, galerías, eventos) y coordinar turnos/servicios con panel administrativo y almacenamiento en la nube.

Listado de funcionalidades principales:

1. Gestión de artistas
  - Crear, editar y eliminar perfiles de artista.
  - Actualizar descripciones, enlaces a redes y metadatos.
  - Asociar media (fotos y videos) a cada artista desde el panel administrativo.

2. Administración de media
  - Subida y gestión de imágenes y videos usando Cloudinary como CDN/almacenamiento.
  - Eliminación y actualización de recursos multimedia asociados a artistas.

3. Turnos y solicitudes
  - Formulario público para solicitar turnos; el formulario valida disponibilidad y muestra las franjas disponibles sin exponer la lista completa de turnos aprobados.
  - Solicitudes registradas en Firestore con estados (por ejemplo: pendiente, aprobada, rechazada).
  - Panel administrativo para revisar, aprobar o rechazar solicitudes.
  - Calendario administrativo que muestra todos los turnos aprobados, ordenados por fecha y hora.

4. Comunicaciones por correo
  - Endpoint serverless de ejemplo para envío de emails (configurable con credenciales SMTP).

5. Integraciones y seguridad
  - Frontend conectado a Firebase (Auth + Firestore) para autenticación y persistencia.
  - Uso de Cloudinary para upload y entrega de imágenes/videos.
  - Recomendación de roles y reglas en Firestore para separar funciones públicas y administrativas.

---
## ✨ Galeria
<img width="200" alt="image" src="https://github.com/user-attachments/assets/bdd9278c-9de6-43ea-9ecf-f0222eebc328" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/2a07927a-86e5-4fd7-8bed-3543715d7f00" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/80b94e40-a4a4-4b5e-b042-c61314b8541f" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/6d2c8ae4-6f8e-4fe0-aa86-35ea5f121e3c" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/65c8f361-2708-4517-86e9-01010939df4e" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/c7f5d7af-ec2f-4f89-8173-b77296235b82" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/ccd2d067-2a75-4a98-89ad-216b3d53ca0b" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/4152e37b-d6bf-4f48-9ea2-420d136dbbdd" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/d163dcd3-6b3c-4531-9ab3-1a9daff52c5c" />

<img width="200" alt="image" src="https://github.com/user-attachments/assets/34d3f1dd-0170-4816-89be-c563c1beb5fd" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/73d52b12-9718-4cab-bae0-060de87788e4" />
<img width="200" alt="image" src="https://github.com/user-attachments/assets/0694c0d9-5cc2-4e80-9e1d-45aa8792b31f" />










## 🔁 Flujo de turnos y comportamiento detallado

Cómo funciona, en líneas generales:

1. El usuario público solicita un turno seleccionando una fecha; la UI muestra únicamente las franjas disponibles para esa fecha (mecanismo que evita exponer datos de turnos ya aprobados en la vista pública).
2. La solicitud queda registrada en la base de datos con un estado inicial (por ejemplo: "pendiente").
3. El equipo administrativo revisa las solicitudes desde el panel privado y decide aprobar o rechazar cada solicitud.
4. Una vez aprobada, la solicitud cambia su estado y pasa a formar parte del calendario administrativo, donde se listan y ordenan todos los turnos aprobados por fecha y hora.

Notas técnicas y consideraciones:
- El proceso de aprobación y el calendario se gestionan desde la capa administrativa que actualiza Firestore; no hay clave secreta embebida en el cliente para aprobar turnos.
- La comprobación de disponibilidad en el punto de solicitud evita conflictos con turnos aprobados (consulta y filtrado por fecha/estado antes de mostrar opciones al usuario).

---

## 🏗️ Arquitectura y dependencias clave

- Frontend: React 19 + Vite
- Estilos: Tailwind CSS
- Animaciones: Framer Motion
- BBDD y Auth (cliente): Firebase (Firestore, Auth)
- Email: `nodemailer` usado por el ejemplo en `api/sendEmail.js`

Dependencias importantes (ver `package.json`): `firebase`, `framer-motion`, `nodemailer`, `react-router-dom`, `tailwindcss`.

---

## 🔁 Flujo técnico (resumen)

1. El frontend crea/lee documentos en Firestore usando la configuración expuesta en `src/firebase.js`.
2. Las solicitudes de contacto/solicitudes se almacenan y pueden visualizarse/gestionarse desde componentes admin.
3. Para el envío de correos, el proyecto incluye `api/sendEmail.js` que usa `nodemailer` y variables de entorno (`EMAIL_USER`, `EMAIL_PASS`).

Nota: este repositorio contiene ejemplos de endpoints; algunos flujos del README de referencia (por ejemplo, generación de tokens de registro) pueden ser conceptuales y no estar directamente implementados aquí.

---

## 🔑 Variables de entorno

Variables obligatorias para ejecutar la app en modo desarrollo y para que los ejemplos funcionen:

- VITE_FIREBASE_API_KEY
- VITE_FIREBASE_AUTH_DOMAIN
- VITE_FIREBASE_PROJECT_ID
- VITE_FIREBASE_STORAGE_BUCKET
- VITE_FIREBASE_MESSAGING_SENDER_ID
- VITE_FIREBASE_APP_ID
- VITE_FIREBASE_MEASUREMENT_ID (opcional)
- EMAIL_USER — cuenta usada por `api/sendEmail.js` (ej.: Gmail)
- EMAIL_PASS — contraseña o app password asociado a `EMAIL_USER`
 - VITE_CLOUDINARY_CLOUD_NAME — nombre del cloud en Cloudinary
 - VITE_CLOUDINARY_UPLOAD_PRESET — upload preset público para carga directa desde cliente/admin

Notas:
- Las variables `VITE_...` son expuestas al código cliente por Vite; evita incluir secretos sensibles en variables de entorno públicas si no es necesario.
- Si usas otro proveedor SMTP o API (SendGrid, Mailgun), adapta `api/sendEmail.js` en consecuencia y no subas credenciales al repositorio.

---


## 🔌 Endpoints serverless incluidos

- Endpoint de ejemplo para envío de correos (requiere configuración SMTP): recibe payload con destinatario, asunto y cuerpo y reenvía usando un transportador SMTP en el backend.

Notas sobre backend e integraciones:
- El proyecto incluye utilidades para subir media a Cloudinary desde el panel administrativo; la subida puede realizarse desde el cliente (direct upload con preset) o mediante llamadas desde el backend según configuraciones.
  
## 🚀 Requisitos y ejecución local

Requisitos mínimos:

- Node.js >= 18
- npm (o yarn)

Instalación y ejecución:

1. Instala dependencias:

```bash
npm install
```

2. Crea un archivo `.env` en la raíz con las variables listadas arriba (o configura las variables en tu entorno/hosting).

3. Ejecuta en desarrollo:

```bash
npm run dev
```

Notas de ejecución:
- El script `dev` lanza Vite (ver `package.json`).
- `api/sendEmail.js` es compatible con despliegues tipo Vercel/Netlify (carpeta `api/`). En entornos serverless necesitarás configurar las variables `EMAIL_USER` y `EMAIL_PASS` en el proveedor.

---

## 🔒 Consideraciones de seguridad y despliegue

- Nunca subas claves ni contraseñas al repositorio.
- Protege los endpoints que modifican datos sensibles: verifica roles de admin (Firestore rules o token verificación con Firebase Admin SDK en funciones server-side).
- Configura reglas estrictas de Firestore para lecturas/escrituras (no permitir accesos públicos sobre colecciones sensibles).
- Si usas Gmail para `nodemailer`, crea una contraseña de aplicación o usa un servicio SMTP profesional.
- Habilita HTTPS y CORS apropiado en producción.

---

## 📦 Qué contiene este repositorio

Estructura principal (resumen):

- `src/` — código React
  - `App.jsx`, `main.jsx`, `index.css`
  - `firebase.js` — configuración cliente Firebase
  - `components/` — múltiples componentes UI (Artista, Eventos, Admin, Gallery, Forms...)
  - `hooks/` — hooks personalizados
  - `utils/` — utilidades (ej: `sendEmail.js` wrapper en utils si existe)
- `src/utils/uploadToImgBB.js` — helper de subida a Cloudinary 
- `api/` — endpoints serverless (`sendEmail.js`)
- `public/` — assets públicos
- `package.json`, `vite.config.js`, `tailwind` y configs.

Archivos detectados y relevantes:
- `src/firebase.js` — usa variables `VITE_FIREBASE_*`.
- `api/sendEmail.js` — ejemplo de endpoint con `nodemailer` que según el código existente usa `EMAIL_USER` y `EMAIL_PASS`.

---

## 🛠️ Cómo contribuir o pedir acceso al código completo

Si este repositorio es un demo y deseas acceso a la versión completa o privada del proyecto, contacta al mantenedor del repositorio para recibir instrucciones o un paquete del código.



## 📬 Contacto para reclutadores

**Mario A. Gallego** — marioagallego91@gmail.com

Por favor incluya en el correo: nombre, compañía, puesto y motivo de la solicitud. Responderé con acceso privado al código o instrucciones para revisar el proyecto según su preferencia.
---

---

## 📜 Licencia

Contenido demo para evaluación técnica. Para acuerdos de licencia o uso en producción, contactar al autor.


