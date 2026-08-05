# 📘 Manual Técnico · Portfolio migueljerico base44

---

## 1. Arquitectura general

El proyecto es una aplicación web *single-page* (SPA) construida sobre **React 18 + Vite + TailwindCSS** y desplegada mediante la plataforma **Base44** (Backend as a Service). Sigue una arquitectura modular por capas, donde la lógica de presentación, la gestión de estado/navegación y la capa de datos están claramente separadas. El siguiente diagrama describe el flujo de la información desde la interacción del usuario hasta el almacenamiento o servicios externos:

```
┌──────────────────────────────────────────────────────────────────┐
│                    CAPA DE PRESENTACIÓN                         │
│  ┌────────────────────┐  ┌─────────────────┐  ┌──────────────┐  │
│  │  Componentes UI    │  │  Componentes    │  │  Componentes │  │
│  │  (ui/):            │  │  Portfolio:     │  │  Transversales│  │
│  │  AuthLayout,       │  │  Hero, About,   │  │  CommandBar,  │  │
│  │  ProtectedRoute,   │  │  Portfolio,     │  │  ScrollProg., │  │
│  │  GoogleIcon        │  │  Contact,Footer │  │  Navbar, etc. │  │
│  └────────┬───────────┘  └────────┬────────┘  └───────┬───────┘  │
│           └───────────────────────┼────────────────────┘          │
└───────────────────────────────────┼────────────────────────────────┘
                                    │
┌───────────────────────────────────▼────────────────────────────────┐
│                      CAPA DE LÓGICA / RUTEO                       │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │  App.jsx (Router con React Router)                           │  │
│  │  Pages: Home, Login, Register, ForgotPassword, ResetPassword│  │
│  │  Hooks: use-mobile (detección breakpoint), use-size (tamaño) │  │
│  └────────────────────────┬─────────────────────────────────────┘  │
└───────────────────────────┼────────────────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────────────────┐
│                   CAPA DE DATOS / CONFIGURACIÓN                   │
│  ┌──────────────────────┐  ┌───────────────┐  ┌───────────────┐  │
│  │  api/base44Client.js │  │  .env / cfg   │  │  Base44 BaaS  │  │
│  │  (integración con   │  │  (vars entorno│  │  (backend,     │  │
│  │   Base44 backend)   │  │   base44)     │  │   auth, hosting)│  │
│  └──────────────────────┘  └───────────────┘  └───────────────┘  │
└────────────────────────────────────────────────────────────────────┘
```

> **Nota:** La navegación se realiza mediante **anclas HTML** (scroll suave) para las secciones del *Home* (Hero, Sobre mí, Portafolio, Contacto) y rutas completas para las páginas de autenticación (Login, Register, etc.). La barra de navegación fija resalta la sección visible mediante un **scroll spy** (implementado en `useActiveSection` hook o similar).

---

## 2. Módulos y componentes

### 2.1. Estructura de carpetas del repositorio

```
src/
├── api/
│   └── base44Client.js          # Integración con backend de Base44
├── components/
│   ├── portfolio/               # Componentes específicos del sitio público
│   │   ├── Hero.jsx             # Cabecera principal con foto, rol y CTA
│   │   ├── About.jsx            # Biografía, experiencia y valores
│   │   ├── Portfolio.jsx        # Galería de proyectos (contiene PortfolioCard)
│   │   ├── PortfolioCard.jsx    # Tarjeta individual de proyecto
│   │   ├── Contact.jsx          # Formulario de contacto
│   │   ├── Footer.jsx           # Pie de página con enlaces
│   │   ├── CommandBar.jsx       # Paleta tipo Cmd+K (búsqueda rápida)
│   │   └── ScrollProgress.jsx   # Barra de progreso de scroll
│   └── ui/                      # Componentes transversales / autenticación
│       ├── AuthLayout.jsx       # Layout para páginas de auth
│       ├── ProtectedRoute.jsx   # Ruta protegida (requiere sesión)
│       └── GoogleIcon.jsx       # Icono SVG de Google
├── hooks/
│   ├── use-mobile.jsx           # Detección de breakpoint móvil
│   └── use-size.jsx             # Hook genérico de tamaño (resize observer)
├── pages/
│   ├── Home.jsx                 # Página principal (secciones + layout)
│   ├── Login.jsx                # Inicio de sesión (generado por Base44)
│   ├── Register.jsx             # Registro de usuario (generado por Base44)
│   ├── ForgotPassword.jsx       # Recuperación de contraseña (generado por Base44)
│   └── ResetPassword.jsx        # Restablecimiento de contraseña (generado por Base44)
├── lib/                         # Utilidades y configuraciones
│   └── utils.js                 # Funciones auxiliares (cn, etc.)
├── App.jsx                      # Router principal (React Router)
├── main.jsx                     # Punto de entrada (Vite)
├── index.css                    # Estilos globales + directivas Tailwind
├── vite.config.js               # Configuración de Vite
├── tailwind.config.js           # Configuración de TailwindCSS
├── postcss.config.js            # Configuración de PostCSS
├── components.json              # Configuración de shadcn/ui
└── .env.example                 # Plantilla de variables de entorno
```

### 2.2. Módulos y componentes clave

| Ruta | Responsabilidad | Funciones exportadas clave |
|---|---|---|
| `src/api/base44Client.js` | Cliente HTTP para comunicarse con el backend de Base44 (autenticación, almacenamiento, etc.). | `base44Client`, `login()`, `register()`, `logout()`, `getCurrentUser()` |
| `src/components/portfolio/Hero.jsx` | Sección de bienvenida con foto, nombre, rol y llamada a la acción. | `Hero` (default) |
| `src/components/portfolio/About.jsx` | Sección "Sobre mí": biografía, experiencia, formación y valores. | `About` (default) |
| `src/components/portfolio/Portfolio.jsx` | Galería de proyectos filtrable. Renderiza múltiples `PortfolioCard`. | `Portfolio` (default) |
| `src/components/portfolio/PortfolioCard.jsx` | Tarjeta individual de proyecto con título, descripción, tecnologías y enlaces. | `PortfolioCard` (default) |
| `src/components/portfolio/Contact.jsx` | Formulario de contacto que envía mensajes a través de Base44 o servicio configurado. | `Contact` (default) |
| `src/components/portfolio/Footer.jsx` | Pie de página con enlaces a redes sociales, copyright y enlaces útiles. | `Footer` (default) |
| `src/components/portfolio/CommandBar.jsx` | Paleta de comandos estilo Cmd+K para navegación rápida. | `CommandBar` (default) |
| `src/components/portfolio/ScrollProgress.jsx` | Barra de progreso de scroll horizontal en la parte superior. | `ScrollProgress` (default) |
| `src/components/ui/AuthLayout.jsx` | Layout compartido para páginas de autenticación (Login, Register, etc.). | `AuthLayout` (default) |
| `src/components/ui/ProtectedRoute.jsx` | Componente que protege rutas, redirigiendo a login si no hay sesión. | `ProtectedRoute` (default) |
| `src/components/ui/GoogleIcon.jsx` | Componente SVG del icono de Google. | `GoogleIcon` (default) |
| `src/hooks/use-mobile.jsx` | Hook que detecta si el viewport está por debajo de un breakpoint móvil. | `useMobile` |
| `src/hooks/use-size.jsx` | Hook que devuelve el tamaño actual de un elemento usando `ResizeObserver`. | `useSize`, `useSizeRef` |
| `src/pages/Home.jsx` | Página principal que compone las secciones del portfolio (Hero, About, Portfolio, Contact, Footer). | `Home` (default) |
| `src/pages/Login.jsx` | Página de inicio de sesión (generada por Base44, personalizable). | `Login` (default) |
| `src/pages/Register.jsx` | Página de registro de usuario. | `Register` (default) |
| `src/pages/ForgotPassword.jsx` | Página para solicitar restablecimiento de contraseña. | `ForgotPassword` (default) |
| `src/pages/ResetPassword.jsx` | Página para restablecer la contraseña mediante token. | `ResetPassword` (default) |
| `src/App.jsx` | Configuración del enrutador principal (React Router) y layout global. | `App` (default) |
| `src/main.jsx` | Punto de entrada de la aplicación (renderiza `<App />` en el DOM). | — |
| `src/lib/utils.js` | Utilidades generales, como la función `cn()` para combinar clases Tailwind. | `cn` |

---

## 3. APIs y endpoints

El proyecto no expone APIs propias; toda la comunicación con servicios externos se realiza a través del **cliente de Base44** (`src/api/base44Client.js`). Los endpoints consumidos internamente son proporcionados por la plataforma Base44 y no son configurables directamente. Sin embargo, el formulario de contacto y la autenticación se apoyan en dichos servicios.

A continuación se listan los endpoints esperados del backend de Base44 (no expuestos públicamente, sino usados internamente):

| Método | Ruta (Base44) | Descripción | Parámetros / Body |
|---|---|---|---|
| `POST` | `/auth/login` | Inicia sesión de usuario. | `{ email, password }` |
| `POST` | `/auth/register` | Registra un nuevo usuario. | `{ name, email, password }` |
| `POST` | `/auth/forgot-password` | Envía correo de recuperación. | `{ email }` |
| `POST` | `/auth/reset-password` | Restablece la contraseña con token. | `{ token, password }` |
| `POST` | `/api/contact` | Envía un mensaje de contacto (si se configura). | `{ name, email, message }` |
| `GET` | `/api/projects` | Obtiene la lista de proyectos (si se almacenan en Base44). | `?category=...` |
| `GET` | `/api/profile` | Obtiene datos del perfil del usuario autenticado. | — |

> ⚠️ La disponibilidad de estos endpoints depende de la configuración del proyecto en el panel de Base44.

---

## 4. Variables de entorno

| Variable | Valor de ejemplo | Obligatoria | Descripción |
|---|---|---|---|
| `VITE_BASE44_PROJECT_ID` | `prj_abc123xyz` | ✅ | Identificador único del proyecto en Base44. |
| `VITE_BASE44_API_KEY` | `sk_live_xxxxxxxxxxxx` | ✅ | Clave de API para autenticación con el backend de Base44. |
| `VITE_SITE_URL` | `https://jerico-data-flow.base44.app` | ✅ | URL canónica del sitio (usada para SEO y rutas absolutas). |
| `VITE_CONTACT_EMAIL` | `hola@migueljerico.dev` | ❌ | Correo destino del formulario de contacto (si se configura un servicio externo). |
| `VITE_GITHUB_TOKEN` | `ghp_xxxxxxxxxxxx` | ❌ | Token de GitHub para aumentar límite de rate-limit (opcional). |
| `VITE_ANALYTICS_ID` | `G-XXXXXXXXXX` | ❌ | ID de Google Analytics o similar (opcional). |

> **Importante:** Las variables deben prefijarse con `VITE_` para que estén disponibles en el cliente (Vite expone solo las que comienzan con `VITE_`). No committear el archivo `.env` real; usar `.env.example` como plantilla.

---

## 5. Guía de despliegue

### 5.1. Requisitos previos

- Node.js ≥ 18 (recomendado 20 LTS)
- npm o pnpm
- Cuenta en [Base44](https://base44.app) con un proyecto creado

### 5.2. Configuración local

```bash
# 1. Clonar el repositorio
git clone https://github.com/migueljerico/portfolio-migueljerico-base44.git
cd portfolio-migueljerico-base44

# 2. Instalar dependencias
npm install

# 3. Copiar el archivo de variables de entorno y configurarlas
cp .env.example .env
# Editar .env con los valores reales (VITE_BASE44_PROJECT_ID, VITE_BASE44_API_KEY, etc.)

# 4. Iniciar el servidor de desarrollo
npm run dev
# El sitio estará disponible en http://localhost:5173 (puerto predeterminado de Vite)
```

### 5.3. Despliegue en Base44 (nativo)

Base44 ofrece despliegue integrado. Los pasos generales son:

```bash
# 1. Construir la aplicación para producción
npm run build

# 2. Desplegar usando el CLI de Base44 (o desde el panel web)
npx base44 deploy --prod
```

Alternativamente, desde el panel de Base44 se puede conectar el repositorio de GitHub y configurar despliegue automático en cada push a la rama `main`.

### 5.4. Despliegue en Vercel / Netlify (alternativa)

Dado que el proyecto es una SPA generada con Vite, se puede desplegar en cualquier plataforma de hosting estático:

```bash
# Construir
npm run build

# Desplegar en Vercel
npx vercel --prod

# Desplegar en Netlify
npx netlify deploy --prod --dir=dist
```

> ⚠️ Asegurarse de configurar las variables de entorno en la plataforma elegida. Para el correcto funcionamiento de la autenticación y el contacto, es necesario que el backend de Base44 esté accesible (no hay restricciones CORS si se configura adecuadamente).

---

## 6. Limitaciones conocidas y posibles mejoras

### 6.1. Limitaciones actuales

- ⚠️ **Dependencia de Base44**: La autenticación, el almacenamiento de proyectos y el envío de contacto dependen del backend de Base44. Si el servicio se interrumpe o cambia su API, el sitio podría verse afectado.
- ❌ **Gestión de contenido estática**: Los proyectos y habilidades se almacenan actualmente en archivos JavaScript estáticos (si no se ha configurado un CMS). Cualquier cambio requiere editar el código y redeploy.
- ❌ **Sin pruebas automatizadas**: El proyecto carece de tests unitarios, de integración o E2E.
- ❌ **Internacionalización**: Solo está implementado un idioma (español). No hay soporte para i18n.
- ❌ **Sin analytics**: No se ha integrado ninguna herramienta de análisis de tráfico (aunque está previsto).
- ❌ **Sin modo offline / PWA**: La aplicación no es una Progressive Web App y no funciona sin conexión.

### 6.2. Mejoras futuras

- 🚧 **CMS Headless**: Integrar un CMS como Strapi, Sanity o Contentful para gestionar proyectos y contenido dinámico sin tocar código.
- 🚧 **Blog técnico**: Añadir una sección de blog con soporte para Markdown/MDX y vista de artículos.
- 🚧 **Internacionalización (i18n)**: Implementar soporte multilingüe (español/inglés) usando `react-i18next` o similar.
- 🚧 **Analytics respetuosas**: Integrar Plausible o Umami para medir tráfico sin comprometer la privacidad.
- 🚧 **Modo oscuro persistente**: Mejorar el toggle de tema oscuro para que recuerde la preferencia del usuario (ya implementado en la base, pero se puede pulir).
- 🚧 **Pruebas automatizadas**: Añadir tests unitarios con Vitest y pruebas E2E con Playwright.
- 🚧 **CI/CD**: Configurar GitHub Actions para ejecutar tests y desplegar automáticamente en cada push a `main`.
- 🚧 **PWA**: Convertir la aplicación en una Progressive Web App con service worker y manifiesto para instalación en dispositivos.
- 🚧 **RSS y sitemap**: Generar dinámicamente un feed RSS y un sitemap.xml para mejorar el SEO y la distribución de contenido.

---

<p align="center">Documentación técnica generada a partir del análisis del repositorio migueljerico/portfolio-migueljerico-base44 · 2026</p>