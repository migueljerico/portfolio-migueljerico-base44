# 📘 Manual Técnico · Portfolio migueljerico base44

---

## 1. Arquitectura general

El proyecto se apoya en la plataforma **base44**, que sigue una arquitectura modular por capas. El siguiente diagrama describe el flujo esperado de la información, desde la entrada del usuario hasta el renderizado final:

```
┌──────────────────────────────────────────────────────────┐
│                  CAPA DE PRESENTACIÓN                   │
│  ┌────────────┐ ┌────────────┐ ┌─────────────────────┐  │
│  │  Hero /    │ │  Sections  │ │   Componentes UI    │  │
│  │  Landing   │ │  (About,   │ │   (Cards, Buttons,  │  │
│  │            │ │ Projects)  │ │   Navbar, Footer)   │  │
│  └─────┬──────┘ └──────┬─────┘ └──────────┬──────────┘  │
│        └───────────────┼──────────────────┘              │
└────────────────────────┼─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│                    CAPA DE LÓGICA                        │
│  ┌────────────────────────────────────────────────────┐  │
│  │  Controladores / Hooks / Servicios                 │  │
│  │  (filtrado de proyectos, modo oscuro, form         │  │
│  │   de contacto, internacionalización)               │  │
│  └─────────────────────┬──────────────────────────────┘  │
└────────────────────────┼─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│              CAPA DE DATOS / CONFIGURACIÓN              │
│  ┌──────────────┐ ┌───────────────┐ ┌──────────────┐    │
│  │  data/*.js   │ │  .env / cfg   │ │  APIs ext.   │    │
│  │  (proyectos, │ │  (vars entorno│ │  (GitHub,    │    │
│  │   skills)    │ │   base44)     │ │   CMS, etc.) │    │
│  └──────────────┘ └───────────────┘ └──────────────┘    │
└──────────────────────────────────────────────────────────┘
```

---

## 2. Módulos y componentes

### 2.1. `README.md` (único archivo presente)

| Atributo | Valor |
|---|---|
| **Ruta** | `./README.md` |
| **Responsabilidad** | Punto de entrada documental del proyecto. Describe propósito, instalación y motivación. |
| **Funciones clave** | Ninguna (es un archivo Markdown estático). |

> ℹ️ Conforme el proyecto crezca, se incorporarán los módulos listados a continuación. La siguiente tabla describe la **estructura objetivo** según las convenciones de base44:

| Carpeta/Archivo | Responsabilidad | Funciones exportadas clave |
|---|---|---|
| `src/sections/Hero.jsx` | Cabecera visual y llamada a la acción. | `default: Hero` |
| `src/sections/About.jsx` | Biografía y experiencia. | `default: About` |
| `src/sections/Projects.jsx` | Galería de proyectos con filtros. | `default: Projects`, `filterByTech(tech)` |
| `src/components/Navbar.jsx` | Navegación principal sticky. | `default: Navbar` |
| `src/components/ThemeToggle.jsx` | Conmutador claro/oscuro. | `default: ThemeToggle`, `useTheme()` |
| `src/data/projects.js` | Fuente de datos de proyectos. | `projects: Project[]` |
| `src/data/skills.js` | Habilidades técnicas. | `skills: Skill[]` |
| `src/lib/contact.js` | Envío de mensajes de contacto. | `sendContactMessage(payload)` |

---

## 3. APIs y endpoints

El proyecto no expone APIs propias en su estado actual. Cuando se integre un formulario de contacto o un CMS externo, los endpoints previstos serán:

| Método | Ruta | Descripción | Parámetros |
|---|---|---|---|
| `POST` | `/api/contact` | Envía un mensaje desde el formulario de contacto. | `name`, `email`, `message` |
| `GET`  | `/api/projects` | Devuelve la lista de proyectos en formato JSON. | `?tech=react` (filtro opcional) |
| `GET`  | `/api/repos` | Proxy al API de GitHub para listar repos públicos. | `?limit=10&sort=updated` |

---

## 4. Variables de entorno

| Variable | Valor de ejemplo | Obligatoria | Descripción |
|---|---|---|---|
| `BASE44_PROJECT_ID` | `prj_abc123xyz` | ✅ | Identificador del proyecto en base44. |
| `BASE44_API_KEY` | `sk_live_********` | ✅ | Clave de API para servicios de base44. |
| `CONTACT_EMAIL` | `hola@migueljerico.dev` | ✅ | Correo destino del formulario de contacto. |
| `GITHUB_TOKEN` | `ghp_********` | ❌ | Token personal para aumentar el rate-limit de GitHub. |
| `SITE_URL` | `https://migueljerico.dev` | ✅ | URL canónica del portfolio (usada en SEO). |
| `ANALYTICS_ID` | `G-XXXXXXXXXX` | ❌ | ID de Google Analytics o Plausible. |

> 🔐 **Importante:** nunca commitear el archivo `.env` real. Usar `.env.example` como plantilla.

---

## 5. Guía de despliegue

### 5.1. Preparación

```bash
# 1. Asegurar Node.js ≥ 18
node -v

# 2. Instalar dependencias y construir
npm ci
npm run build
```

### 5.2. Despliegue en Vercel (recomendado para base44)

```bash
# Instalar CLI (si no está)
npm i -g vercel

# Desplegar
vercel --prod
```

Configurar en el panel de Vercel las variables de entorno listadas en la sección 4.

### 5.3. Despliegue en Netlify

```bash
npm i -g netlify-cli
netlify deploy --build --prod
```

### 5.4. Despliegue en GitHub Pages (estático)

```bash
npm run build
npx gh-pages -d dist
```

---

## 6. Limitaciones conocidas y posibles mejoras

### 6.1. Limitaciones actuales

- ⚠️ El repositorio contiene **únicamente un `README.md`**; no hay código fuente todavía, por lo que la documentación técnica es **provisional y orientada a la arquitectura objetivo**.
- ❌ No existe aún un sistema de gestión de contenido (CMS) integrado: los proyectos y skills se almacenarán en archivos JS estáticos.
- ❌ El formulario de contacto requiere configuración manual del backend de envío.
- ❌ No hay tests automatizados ni CI/CD definidos.

### 6.2. Mejoras futuras

- 🚧 Incorporar un **CMS headless** (Strapi, Sanity o Contentful) para editar proyectos sin tocar código.
- 🚧 Añadir **blog técnico** con soporte Markdown + MDX.
- 🚧 Implementar **i18n** (al menos ES/EN).
- 🚧 Integrar **analytics respetuosas con la privacidad** (Plausible o Umami).
- 🚧 Añadir **tests E2E con Playwright** y tests unitarios con Vitest.
- 🚧 Configurar **GitHub Actions** para CI/CD automático en cada `push` a `main`.
- 🚧 Añadir **RSS feed** del blog y **sitemap.xml** dinámico.

---

<p align="center">Documentación técnica generada para @migueljerico · 2026</p>
