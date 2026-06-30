---
name: visual-polish
description: Audita y mejora la calidad visual de páginas web: jerarquía, espaciado, tipografía, color, microinteracciones y consistencia.
metadata:
  audience: developers, designers
  workflow: ui-review
---

## Qué hace

Evalúa un archivo HTML/CSS y propone mejoras visuales concretas. No toca backend ni contenido — solo presentaición.

## Checklist de auditoría visual

### 1. Jerarquía visual
- El title principal (h1) debe ser inconfundible: bold, 2-3x el tamaño del body, sin competir con otros elementos.
- Los CTAs deben ser el elemento más contrastante de la página.
- Una página = una acción principal. Si hay múltiples CTAs, que uno sea dominante.
- No mezclar más de 2 énfasis distintos (bold, color, tamaño) en una misma sección.

### 2. Espaciado y aire
- El padding vertical entre secciones debe ser generoso (80-120px en desktop, 48-64px en mobile).
- El line-height del body debe estar entre 1.6 y 1.8.
- En tarjetas/listas: mínimo 12px de padding interior.
- No hay texto pegado a bordes. Nunca.

### 3. Tipografía
- Máximo 2 familias tipográficas. Una para títulos, otra para texto.
- Evitar más de 3 pesos (regular, semibold, bold). Pesos intermedios (300, 500, 800, 900) rara vez se notan.
- El tracking (letter-spacing) en mayúsculas debe ser de al menos 1px.
- El ancho de línea (max-width en párrafos) no debe exceder 70 caracteres.

### 4. Color
- Paleta limitada: 1 color primario, 1 secundario, 1 de acento, 2 neutros.
- El texto principal debe tener contraste ≥ 4.5:1 contra el fondo.
- No usar negro puro (#000) para texto. Usar grises oscuros (#1a1a1a, #212121).
- Los botones principales deben tener hover state visible (darken 10% o lighten 10%).
- Fondo blanco puro (#fff) cansa. Usar off-white muy sutil (#fafafa, #f8f8f8).

### 5. Cards y contenedores
- Border-radius consistente (mismo valor en toda la página).
- Sombras sutiles o ninguna. Sombras muy grandes se ven baratas.
- Si hay sombra, que sea uniforme: `box-shadow: 0 4px 24px rgba(0,0,0,.08)`.
- Todos los elementos clickeables deben tener cursor: pointer y hover state.

### 6. Microinteracciones
- Hover en botones: transición de 200-300ms, no instantánea.
- Hover en links: subrayado o cambio de color sutil.
- Scroll suave activado (`scroll-behavior: smooth`).
- Los iconos deben tener tamaño consistente (mismo font-size o dimensiones).

### 7. Responsive
- Mobile-first: diseñar primero para 375px, luego escalar.
- Tap targets mínimos de 44x44px (estándar Apple/Google).
- Menú hamburguesa obligatorio si hay más de 3 links de navegación.
- Imágenes deben tener max-width: 100%.

### 8. Header y footer
- Header fijo con altura definida (64-80px) más padding interno.
- El header transparente requiere que el texto sea legible contra el hero (usar sombra en texto o fondo semitransparente).
- Footer debe tener color de fondo diferente al contenido, con links en gris, no negro.
- Logo debe ser clickeable y llevar al inicio.

### 9. Formularios
- Labels visibles (no solo placeholders).
- Inputs con padding interno de 12-16px, border-radius consistente.
- Botón de submit debe tener el mismo ancho del input en mobile (full-width).
- Mensajes de error deben estar cerca del campo, no en un popup.

## Modo de uso

Cuando pidan revisar lo visual de un archivo, aplicá esta checklist punto por punto. Reportá cada hallazgo con:
1. **Qué**: el problema específico
2. **Dónde**: línea o selector
3. **Por qué**: impacto en el usuario
4. **Cómo**: código de la solución

No implementes nada sin preguntar antes.
