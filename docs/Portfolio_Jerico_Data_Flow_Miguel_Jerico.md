# Portfolio Jerico Data Flow — Documentación técnica exhaustiva

## 📋 Resumen
Portfolio personal *single-page* de Miguel Jericó Díaz, construido sobre **React 18 + Vite + TailwindCSS** y desplegado mediante la plataforma **Base44** (BaaS), que sirve como carta de presentación digital en su transición profesional hacia perfiles de **Análisis de Datos e IA aplicada al negocio**, mostrando simultáneamente proyectos de BI/IA y su capacidad de construir un producto frontend completo.

## 🔑 Puntos clave
- 🧭 **Navegación por anclas** con barra fija y *pill* activa que resalta la sección visible (Inicio, Sobre mí, Portafolio, Contacto).
- ⚛️ **Stack moderno y coherente**: React 18 + Vite + TailwindCSS + primitivas tipo **shadcn/ui** (vía `components.json`).
- ☁️ **Base44 como BaaS**: genera la base de código, gestiona autenticación y despliega automáticamente bajo el dominio `jerico-data-flow.base44.app`.
- 🧱 **Componentización por dominio** (`/components/portfolio`) frente a una capa transversal (`/components/ui`), lo que da una arquitectura limpia y escalable.
- 🧰 **Power-user features**: `CommandBar` (paleta tipo Cmd+K) y `ScrollProgress` (barra de progreso de scroll).
- 🔐 **Andamiaje de autenticación** generado por Base44 (Login, Register, ForgotPassword, ResetPassword, ProtectedRoute) que queda disponible para futuras áreas privadas, aunque el flujo público no lo requiere.
- 📦 **Réplica en GitHub** para control de versiones, junto al despliegue nativo en Base44.
- 🎯 **Diferenciador frente al resto del portfolio**: mientras los demás proyectos enseñan análisis de datos (Power BI, Python, R), esta web enseña **producto y frontend**.

---

## 📝 Detalle

### 1. Descripción general del proyecto

| Campo | Detalle |
|---|---|
| **Nombre** | Portfolio Personal Data & IA — Jerico Data Flow |
| **Autor** | Miguel Jericó Díaz |
| **URL** | https://jerico-data-flow.base44.app |
| **Tipo** | Web personal *single-page* (SPA con navegación por anclas) |
| **Estado** | Proyecto personal · en producción |
| **Tecnologías** | React 18, Vite, TailwindCSS, Base44 |
| **Repositorio** | Réplica en GitHub |

**Objetivo funcional:** actuar como carta de presentación digital dentro de la transición profesional de Miguel hacia perfiles de Análisis de Datos e IA aplicada al negocio, combinando **15 años de experiencia previa** en operaciones y liderazgo de equipos con las nuevas competencias técnicas adquiridas (Power BI, Python, R, automatización e IA generativa).

**Objetivo diferenciador:** a diferencia de los ejercicios de análisis de datos del portfolio, este proyecto pone el foco en la **capa de producto y frontend**: arquitectura de componentes, diseño de interacción y despliegue de una aplicación web completa.

---

### 2. Arquitectura técnica y stack

#### 2.1 Stack tecnológico por capas

| Capa | Tecnología | Rol en el proyecto |
|---|---|---|
| **Framework UI** | React 18 | Renderizado de componentes, estado, composición |
| **Bundler / dev server** | Vite (`vite.config.js`) | Arranque en frío rápido, HMR, build optimizado |
| **Estilos** | TailwindCSS (`tailwind.config.js` + `postcss.config.js`) | Sistema de utilidades y *design tokens* |
| **Primitivas UI** | shadcn/ui (configuración en `components.json`) | Botones, inputs, tarjetas reutilizables |
| **Backend / Auth / Hosting** | Base44 (`base44Client.js`) | BaaS que genera código, autentica y despliega |
| **Control de versiones** | GitHub | Réplica del código e historial |

#### 2.2 Estructura de carpetas del repositorio

```
src/
├── api/
│   └── base44Client.js          # Integración con backend de Base44
├── components/
│   ├── portfolio/               # Componentes específicos del sitio público
│   │   ├── Hero.jsx
│   │   ├── About.jsx
│   │   ├── Portfolio.jsx
│   │   ├── PortfolioCard.jsx
│   │   ├── Contact.jsx
│   │   ├── Footer.jsx
│   │   ├── CommandBar.jsx
│   │   └── ScrollProgress.jsx
│   └── ui/                      # Componentes transversales / autenticación
│       ├── AuthLayout.jsx
│       ├── ProtectedRoute.jsx
│       └── GoogleIcon.jsx
├── hooks/
│   ├── use-mobile.jsx           # Detección de breakpoint móvil
│   └── use-size.jsx             # Hook genérico de tamaño
├── pages/
│   ├── Home.jsx                 # Página principal renderizada
│   ├── Login.jsx                # Generada por Base44
│   ├── Register.jsx
│   ├── ForgotPassword.jsx
│   └── ResetPassword.jsx
├── App.jsx                      # Router + layout
├── main.jsx                     # Punto de entrada (montaje en DOM)
└── index.css                    # Estilos globales + directivas de Tailwind
```

#### 2.3 Capas de la aplicación (de arriba a abajo)

```
┌─────────────────────────────────────────────────┐
│  main.jsx → monta <App /> en #root              │
├─────────────────────────────────────────────────┤
│  App.jsx → define rutas y layouts               │
├─────────────────────────────────────────────────┤
│  Pages (Home.jsx) → compone secciones           │
├─────────────────────────────────────────────────┤
│  Components/portfolio → Hero, About, Portfolio, │
│  PortfolioCard, Contact, Footer, CommandBar,    │
│  ScrollProgress                                 │
├─────────────────────────────────────────────────┤
│  Components/ui → AuthLayout, ProtectedRoute,    │
│  GoogleIcon                                     │
├─────────────────────────────────────────────────┤
│  Hooks → use-mobile, use-size                   │
├─────────────────────────────────────────────────┤
│  api/base44Client.js → backend Base44           │
└─────────────────────────────────────────────────┘
```

---

### 3. Diseño de interacción y UX

#### 3.1 Navegación principal

- **Tipo:** barra fija (*sticky*) en la parte superior.
- **Mecanismo:** anclas a 4 secciones visibles en el flujo público:
  - `#inicio` — Hero
  - `#sobre-mi` — About
  - `#portafolio` — Portfolio
  - `#contacto` — Contact
- **Indicador de sección activa:** una "píldora" (*pill*) que se desplaza de forma suave para resaltar la sección visible. Esto se consigue típicamente con un `IntersectionObserver` que detecta qué sección está en el viewport.
- **Scroll entre secciones:** *smooth scroll* (nativo del navegador o `scroll-behavior: smooth` en CSS).

#### 3.2 Secciones y componentes asociados

| Sección | Componente | Función principal |
|---|---|---|
| **Inicio** | `Hero.jsx` | Titular impactante, propuesta de valor, CTA principal |
| **Sobre mí** | `About.jsx` | Bio profesional,skills, recorrido profesional |
| **Portafolio** | `Portfolio.jsx` + `PortfolioCard.jsx` | Grid de proyectos de BI/IA con tarjetas |
| **Contacto** | `Contact.jsx` | Formulario y/o canales de contacto |
| **— (transversal)** | `Footer.jsx` | Enlaces secundarios, copyright, año dinámico |
| **— (transversal)** | `CommandBar.jsx` | Paleta de comandos estilo Cmd+K / Ctrl+K |
| **— (transversal)** | `ScrollProgress.jsx` | Barra de progreso de scroll |

#### 3.3 Componentes de soporte (power-user features)

- **`CommandBar.jsx`** — paleta de comandos al estilo Cmd+K / Ctrl+K. Permite saltar rápido a secciones, copiar email, abrir LinkedIn, etc. Es un *power-user feature* muy alineado con perfiles técnicos-data.
- **`ScrollProgress.jsx`** — barra de progreso de scroll (probablemente fija en parte superior o lateral) que indica al usuario cuánta página le queda por ver.

#### 3.4 Diseño responsive

- **Hook `use-mobile.jsx`**: probablemente expone un boolean `isMobile` basado en `window.innerWidth` o en el breakpoint `md:` (768px) de Tailwind.
- **Hook `use-size.jsx`**: hook genérico para observar dimensiones, útil en animaciones o layouts dinámicos.
- Ambos habilitan un enfoque **mobile-first** con los breakpoints estándar de Tailwind.

---

### 4. Integración con Base44 (backend-as-a-service)

#### 4.1 Cliente de integración

- **`src/api/base44Client.js`** centraliza toda la comunicación con los servicios de Base44.
- Esto aporta dos beneficios clave:
  1. **Portabilidad:** si en el futuro se migra de Base44 a otro BaaS, solo se toca este archivo.
  2. **Separación de responsabilidades:** los componentes no mezclan lógica de UI con llamadas HTTP.

#### 4.2 Andamiaje de autenticación (generado por Base44)

Aunque la página es pública, Base44 genera por defecto la siguiente estructura, que queda disponible para flujos autenticados:

| Archivo | Función |
|---|---|
| `Login.jsx` | Pantalla de inicio de sesión |
| `Register.jsx` | Pantalla de registro |
| `ForgotPassword.jsx` | Recuperación de contraseña |
| `ResetPassword.jsx` | Restablecimiento de contraseña |
| `ProtectedRoute.jsx` | HOC/wrapper para rutas privadas |
| `AuthLayout.jsx` | Layout específico para flujos de auth |
| `GoogleIcon.jsx` | Pista visual de login social con Google disponible |

> **Nota:** las páginas públicas visitadas **no requieren inicio de sesión**, por lo que el usuario nunca llega a este flujo en la experiencia actual del portfolio.

#### 4.3 Implicaciones de la coexistencia auth + público

- Mantener este código "disponible pero no usado" es válido porque:
  - Si en el futuro se añade un área privada (dashboard de cliente, área de descargas, blog privado), el andamiaje ya está listo.
  - El impacto en el bundle final es bajo si Vite aplica *tree-shaking* sobre rutas no utilizadas, aunque conviene verificar si las páginas de auth entran en el bundle principal al no haber *lazy loading* explícito con `React.lazy()`.

---

### 5. Despliegue y operaciones

| Aspecto | Detalle |
|---|---|
| **Hosting** | Infraestructura gestionada por Base44 |
| **Dominio** | `jerico-data-flow.base44.app` (subdominio Base44) |
| **Build** | Vite genera el bundle optimizado de producción |
| **CI/CD** | Gestionado automáticamente por Base44 (re-deploy al detectar cambios) |
| **Repositorio** | GitHub (réplica del código para control de versiones) |
| **SSL/HTTPS** | Habilitado por defecto al estar bajo el dominio de Base44 |

---

### 6. Objetivos de negocio del sitio

1. **Presentar el perfil profesional** de Miguel en una sola vista, sin fricción.
2. **Mostrar proyectos de BI e IA aplicada** mediante tarjetas que enlazan a cada caso de estudio.
3. **Ofrecer un canal de contacto directo** (formulario y/o datos de contacto) sin depender de un cliente de correo externo.
4. **Transmitir una imagen profesional moderna**, coherente con un perfil técnico-data, alejada del típico CV plano.

---

### 7. Análisis de calidad técnica

#### ✅ Lo que está muy bien

1. **Stack moderno y coherente** — React 18 + Vite + Tailwind + shadcn/ui es exactamente lo que se usa hoy en producción en startups y consultoras tech. Comunica que Miguel trabaja con herramientas actuales.
2. **Separación de responsabilidades clara** — `api/`, `components/portfolio/`, `components/ui/`, `hooks/`, `pages/`. Arquitectura preparada para escalar.
3. **Componentización por dominio** — la carpeta se llama `portfolio/` (no genéricos `Header/Footer`), lo que refleja un modelo mental correcto: componentes nombrados por su propósito, no por su tipo.
4. **Power-user features** — `CommandBar` y `ScrollProgress` son detalles de criterio de producto que diferencian este portfolio del 90% de portfolios de data analysts hechos con plantilla.
5. **Hook `use-size` reutilizable** — buena práctica de código limpio, no acoplado a un caso concreto.
6. **Documentación previa en PDF** — el simple hecho de contar con este documento técnico ya sitúa el proyecto por encima de la media.

#### ⚠️ Áreas a revisar / oportunidades de mejora

1. **Accesibilidad (a11y)** — no mencionada explícitamente. Recomendable auditar:
   - Contraste de colores (mínimo WCAG AA).
   - Navegación por teclado (orden de tabulación, focus visible).
   - `aria-label` en iconos y botones sin texto.
   - Soporte de `prefers-reduced-motion` para usuarios con sensibilidad al movimiento.
2. **SEO** — al ser una SPA en Base44, el SEO depende de cómo Base44 gestione SSR/SSG. Verificar:
   - `<title>` y `<meta description>` únicos y descriptivos.
   - Open Graph y Twitter Cards para que se vea bien al compartir en LinkedIn.
   - Datos estructurados `Person` (JSON-LD) de Schema.org.
   - `sitemap.xml` y `robots.txt`.
3. **Rendimiento** — auditar con Lighthouse:
   - LCP (Largest Contentful Paint) < 2.5 s.
   - CLS (Cumulative Layout Shift) < 0.1.
   - Bundle size: hacer *lazy loading* de páginas de auth con `React.lazy()` para sacarlas del bundle inicial si no se usan.
4. **Internacionalización** — si Miguel busca oportunidades fuera del ámbito hispanohablante, un switch ES/EN duplica el alcance.
5. **Analytics ético** — incorporar Plausible, Umami o GA4 para medir visitas, fuentes de tráfico y comportamiento.
6. **Formulario de contacto** — al usar Base44 como backend, el envío pasa por su API. Asegurar:
   - Validación en cliente y servidor.
   - Protección anti-spam (honeypot, reCAPTCHA invisible o Turnstile).
   - Mensajes de éxito y error claros.
7. **Modo oscuro** — muy habitual en portfolios data/tech; verificar contraste en ambos modos si existe.
8. **Contenido dinámico del portfolio** — si las tarjetas leen los proyectos de un array hardcodeado, valorar migrar a un JSON externo o CMS ligero para añadir proyectos sin redeploy.

---

### 8. Roadmap sugerido (priorizado)

| Prioridad | Acción | Impacto esperado |
|---|---|---|
| 🔴 Alta | Auditoría SEO + Open Graph | Visibilidad en búsquedas y al compartir en redes |
| 🔴 Alta | Auditoría accesibilidad WCAG AA | Inclusión + calidad percibida |
| 🟠 Media | Lazy loading de páginas de auth | Reducción del bundle inicial |
| 🟠 Media | Analytics ético (Plausible/Umami) | Decisiones basadas en datos reales |
| 🟡 Baja | i18n ES/EN | Más oportunidades laborales internacionales |
| 🟡 Baja | CMS ligero para proyectos | Escalabilidad del contenido sin redeploy |
| 🟢 Opcional | Dominio personalizado | Branding a largo plazo |
| 🟢 Opcional | Modo oscuro (si no existe) | UX moderna y coherente con perfil técnico |

---

### 9. Resumen en una frase

> Portfolio *single-page* con **React 18 + Vite + Tailwind + shadcn/ui**, desplegado en **Base44**, con una arquitectura limpia y orientada a producto que demuestra no solo capacidades de análisis de datos, sino también de construcción de aplicaciones web completas — exactamente el perfil híbrido que demanda el mercado de **Data & IA aplicada al negocio**.

---

## ✅ Conclusiones / siguientes pasos

- ✅ El portfolio está **en producción y operativo** en `jerico-data-flow.base44.app`, con un stack moderno, una arquitectura limpia y componentes de interacción avanzados (`CommandBar`, `ScrollProgress`, *pill* activa) que elevan la calidad percibida por encima de la media del sector.
- 🔄 **Siguientes pasos naturales** según la prioridad del roadmap:
  1. Auditar **SEO y Open Graph** para mejorar el descubrimiento del perfil.
  2. Auditar **accesibilidad WCAG AA** (contraste, teclado, `aria-*`, `prefers-reduced-motion`).
  3. Aplicar **lazy loading** a las páginas de auth generadas por Base44 para aligerar el bundle inicial.
  4. Integrar **analytics ético** (Plausible o Umami) para tomar decisiones de contenido basadas en datos.
  5. Valorar un **dominio personalizado** y **i18n ES/EN** para ampliar alcance profesional.
- 📌 La base técnica está consolidada: el siguiente salto de calidad viene de **contenido, accesibilidad y medición**, no de reescribir la arquitectura.