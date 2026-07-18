# 🚀 Portfolio migueljerico · base44

<p align="left">
  <img src="https://img.shields.io/badge/Lenguaje-Múltiple-4A90E2?style=for-the-badge" alt="Lenguaje" />
  <img src="https://img.shields.io/badge/Stack-Portfolio%20Web-FF6F61?style=for-the-badge" alt="Stack" />
  <img src="https://img.shields.io/badge/Estado-En%20Desarrollo-FFC107?style=for-the-badge" alt="Estado" />
  <img src="https://img.shields.io/badge/Licencia-MIT-2ECC71?style=for-the-badge" alt="Licencia" />
</p>

*Portfolio personal de @migueljerico generado con la plataforma base44, una base modular para mostrar proyectos, habilidades y experiencia profesional en la web.*

---

## 📋 Descripción

Este repositorio contiene la base del portfolio personal de **@migueljerico**, construido sobre **base44**, una plataforma que facilita la creación rápida de sitios web personalizables sin renunciar a un control fino del código.

El proyecto nace con el objetivo de centralizar la presencia profesional del autor en internet: servir como carta de presentación, repositorio curado de proyectos relevantes y punto de contacto para colaboraciones, ofertas laborales o contribuciones open source.

Está pensado para **desarrolladores, reclutadores técnicos y colaboradores potenciales** que quieran conocer de un vistazo las competencias técnicas, la trayectoria y los proyectos destacados del autor, con un diseño limpio, modular y fácil de desplegar.

---

## ✨ Funcionalidades

| Funcionalidad | Descripción |
|---|---|
| 🏠 Página de inicio | Presentación profesional con foto, nombre, rol y eslogan personal. |
| 👤 Sección "Sobre mí" | Biografía, experiencia, formación y valores profesionales. |
| 💼 Portafolio de proyectos | Listado filtrable de proyectos con descripción, tecnologías y enlaces. |
| 🛠️ Stack tecnológico | Visualización de las herramientas, lenguajes y frameworks dominados. |
| 📬 Formulario de contacto | Canal directo para recibir mensajes sin exponer el correo personal. |
| 📱 Diseño responsive | Adaptación completa a dispositivos móviles, tablets y escritorio. |
| 🌙 Modo oscuro | Conmutador de tema claro/oscuro persistente en el navegador. |
| ⚡ Optimización SEO | Metaetiquetas, Open Graph y sitemap para mejorar el posicionamiento. |

---

## 🔗 Acceso / Demo

> _Pendiente de despliegue. Una vez publicado, el enlace se incluirá en esta sección._

---

## ⚙️ Instalación

> ⚠️ El repositorio contiene actualmente un único `README.md` como punto de partida. A continuación se detallan los pasos de instalación esperados según el stack base44.

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/migueljerico/portfolio-migueljerico-base44.git
   cd portfolio-migueljerico-base44
   ```

2. **Inicializar la estructura del proyecto base44**
   ```bash
   npx base44 init
   ```

3. **Instalar dependencias**
   ```bash
   npm install
   ```

4. **Configurar variables de entorno**
   ```bash
   cp .env.example .env
   # Edita .env con tus claves personales
   ```

5. **Levantar el servidor de desarrollo**
   ```bash
   npm run dev
   ```

---

## 🚀 Uso

Una vez levantado el entorno de desarrollo, abre el navegador en `http://localhost:3000` para ver el portfolio. La estructura modular de base44 permite editar cada sección desde archivos independientes:

```bash
# Estructura de edición típica en base44
src/
├── sections/        # Bloques de la landing (Hero, About, Projects...)
├── components/      # Componentes reutilizables (Cards, Buttons...)
├── data/            # Contenido editable (proyectos, skills, experiencia)
└── styles/          # Temas y estilos globales
```

Ejemplo de personalización del contenido en `src/data/projects.js`:

```javascript
export const projects = [
  {
    title: "Mi proyecto destacado",
    description: "Descripción breve del proyecto y su impacto.",
    tech: ["React", "Node.js", "PostgreSQL"],
    link: "https://github.com/migueljerico/proyecto",
  },
];
```

---

## 📁 Estructura del proyecto

```
portfolio-migueljerico-base44/
└── README.md
```

> 📌 _El repositorio se encuentra en fase inicial. La estructura anterior corresponde al estado actual; la estructura completa esperada con base44 se detalla en la sección de Uso._

---

## 🛠️ Tecnologías

| Herramienta | Versión / Detalle | Uso en el proyecto |
|---|---|---|
| **base44** | Última estable | Plataforma base de construcción del portfolio. |
| **Node.js** | ≥ 18.x | Entorno de ejecución para herramientas de build. |
| **npm** | ≥ 9.x | Gestión de dependencias y scripts. |
| **Git** | 2.x+ | Control de versiones del proyecto. |
| **Markdown** | CommonMark | Documentación (este README). |

---

## 📚 Contexto formativo o motivación del proyecto

Este proyecto nace con la motivación de consolidar la marca personal de **@migueljerico** en la web. La elección de **base44** como base responde al deseo de contar con un *starter* moderno, desacoplado y fácil de mantener, que permita iterar rápidamente sobre el contenido (proyectos, experiencia, blog) sin reescribir la base técnica cada vez.

Sirve además como **lienzo de aprendizaje** para experimentar con nuevas librerías, patrones de diseño y prácticas de despliegue continuo, manteniendo un producto real (el propio portfolio) como vehículo de mejora continua.

---

<p align="center">Creado por @migueljerico y documentado por Ollama Cloud (MiniMax M3) · 2026</p>
