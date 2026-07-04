---
name: theme-builder
description: Construye elementos HTML con soporte de tema claro y oscuro (día/noche)
---

# Theme Builder - Constructor de Temas Día/Noche

Habilidad para crear estructuras HTML con soporte nativo para cambiar entre tema claro (día) y tema oscuro (noche). Usa variables CSS personalizadas (CSS Custom Properties) y almacenamiento local para recordar preferencias.

## Características Principales

- 🌙 Toggle día/noche integrado
- 🎨 Variables CSS personalizables
- 💾 Persistencia de preferencia en localStorage
- 🔄 Detección automática de preferencia del sistema
- 📱 Diseño responsive
- ✨ Transiciones suaves entre temas

## Estructura Base

Toda estructura HTML debe incluir:

### 1. Variables CSS (raíz)

```css
:root {
  /* Tema Claro (Día) */
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #1a1a1a;
  --text-secondary: #666666;
  --border-color: #e0e0e0;
  --accent-color: #667eea;
  --shadow: rgba(0, 0, 0, 0.1);
}

[data-theme="dark"] {
  /* Tema Oscuro (Noche) */
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --text-primary: #ffffff;
  --text-secondary: #b0b0b0;
  --border-color: #404040;
  --accent-color: #7c8fee;
  --shadow: rgba(0, 0, 0, 0.3);
}
```

### 2. Toggle de Tema (HTML)

```html
<button id="theme-toggle" class="theme-toggle" aria-label="Cambiar tema">
  <span class="theme-icon">🌙</span>
</button>
```

### 3. Estilos Base

```css
* {
  transition: background-color 0.3s, color 0.3s, border-color 0.3s;
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.theme-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  background: var(--bg-secondary);
  color: var(--text-primary);
  cursor: pointer;
  font-size: 20px;
  z-index: 1000;
  box-shadow: 0 2px 8px var(--shadow);
}

.theme-toggle:hover {
  background: var(--accent-color);
  transform: scale(1.1);
}
```

### 4. JavaScript para Gestionar Tema

```javascript
const THEME_KEY = 'theme-preference';
const DARK_THEME = 'dark';
const LIGHT_THEME = 'light';

function initTheme() {
  // Obtener preferencia guardada o detectar del sistema
  let savedTheme = localStorage.getItem(THEME_KEY);
  
  if (!savedTheme) {
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    savedTheme = prefersDark ? DARK_THEME : LIGHT_THEME;
  }
  
  applyTheme(savedTheme);
}

function applyTheme(theme) {
  const htmlElement = document.documentElement;
  
  if (theme === DARK_THEME) {
    htmlElement.setAttribute('data-theme', DARK_THEME);
    updateToggleIcon('☀️');
  } else {
    htmlElement.removeAttribute('data-theme');
    updateToggleIcon('🌙');
  }
  
  localStorage.setItem(THEME_KEY, theme);
}

function toggleTheme() {
  const htmlElement = document.documentElement;
  const currentTheme = htmlElement.getAttribute('data-theme') || LIGHT_THEME;
  const newTheme = currentTheme === DARK_THEME ? LIGHT_THEME : DARK_THEME;
  applyTheme(newTheme);
}

function updateToggleIcon(icon) {
  const toggle = document.getElementById('theme-toggle');
  if (toggle) {
    toggle.querySelector('.theme-icon').textContent = icon;
  }
}

// Inicializar tema al cargar
document.addEventListener('DOMContentLoaded', () => {
  initTheme();
  
  const toggleButton = document.getElementById('theme-toggle');
  if (toggleButton) {
    toggleButton.addEventListener('click', toggleTheme);
  }
});

// Detectar cambios en preferencia del sistema
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
  const newTheme = e.matches ? DARK_THEME : LIGHT_THEME;
  applyTheme(newTheme);
});
```

## Paleta de Colores Recomendada

### Tema Claro
| Variable | Color | Uso |
|----------|-------|-----|
| `--bg-primary` | `#ffffff` | Fondo principal |
| `--bg-secondary` | `#f5f5f5` | Fondos secundarios |
| `--text-primary` | `#1a1a1a` | Texto principal |
| `--text-secondary` | `#666666` | Texto secundario |
| `--border-color` | `#e0e0e0` | Bordes |
| `--accent-color` | `#667eea` | Acentos interactivos |

### Tema Oscuro
| Variable | Color | Uso |
|----------|-------|-----|
| `--bg-primary` | `#1a1a1a` | Fondo principal |
| `--bg-secondary` | `#2d2d2d` | Fondos secundarios |
| `--text-primary` | `#ffffff` | Texto principal |
| `--text-secondary` | `#b0b0b0` | Texto secundario |
| `--border-color` | `#404040` | Bordes |
| `--accent-color` | `#7c8fee` | Acentos interactivos |

## Componentes Típicos

### Card
```html
<div class="card">
  <h2>Título</h2>
  <p>Contenido</p>
</div>
```

```css
.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px var(--shadow);
}
```

### Botón
```css
.button {
  background: var(--accent-color);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
}

.button:hover {
  filter: brightness(1.1);
}
```

### Input
```css
input, textarea {
  background: var(--bg-secondary);
  color: var(--text-primary);
  border: 1px solid var(--border-color);
  padding: 10px;
  border-radius: 6px;
}

input:focus, textarea:focus {
  outline: none;
  border-color: var(--accent-color);
  box-shadow: 0 0 0 3px var(--accent-color);
}
```

## Cuándo Usar

✅ Crear formularios con soporte de tema  
✅ Construir dashboards responsivos  
✅ Diseñar galerías de imágenes  
✅ Desarrollar interfaces de usuario  
✅ Cuando necesites persistencia de preferencias  

## Ejemplo Completo

Cuando construyas una página, incluye:
1. Variables CSS en `:root` y `[data-theme="dark"]`
2. Clase `.theme-toggle` con botón
3. Transiciones suaves en elementos
4. JavaScript de inicialización y toggle
5. atributo `data-theme` en el elemento `html`

Esto garantiza una experiencia consistente con soporte completo de temas día/noche.
