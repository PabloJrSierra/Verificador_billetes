# 🏦 Verificador de Billetes BCB — Serie B

> Herramienta web para verificar si un billete boliviano de la **Serie B** tiene valor legal, basada en el Comunicado de Prensa **CP9/2026** del Banco Central de Bolivia (28 de febrero de 2026).

![HTML](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Tesseract.js](https://img.shields.io/badge/Tesseract.js-5.0-blue?style=flat)
![Sin backend](https://img.shields.io/badge/Backend-Ninguno-green?style=flat)
![Licencia](https://img.shields.io/badge/Licencia-MIT-brightgreen?style=flat)

---

## 📱 Demo en vivo

> Despliega en [Netlify Drop](https://app.netlify.com/drop) o [tiiny.host](https://tiiny.host) arrastrando el archivo `verificador-billetes.html`. Solo necesitas HTTPS para que la cámara funcione en móvil.

---

## ✨ Características

- 📷 **Escaneo por cámara** — Captura el número de serie con la cámara trasera del celular
- 🔍 **OCR integrado** — Reconocimiento óptico de caracteres con Tesseract.js (100% local, sin servidores)
- ✏️ **Ingreso manual** — Opción de escribir el número de serie a mano como alternativa
- 🟢🔴 **Resultado inmediato** — Indica claramente si el billete es válido o no tiene valor legal
- 📵 **Sin conexión requerida** — Todo el procesamiento ocurre en el dispositivo
- 📱 **Responsive** — Optimizado para móviles

---

## 🚀 Cómo usar

### Opción A — Abrir directo en el navegador (desktop)
```bash
# Clona el repositorio
git clone https://github.com/tu-usuario/verificador-billetes-bcb.git

# Abre el archivo en tu navegador
open verificador-billetes.html
```
> ⚠️ La cámara **no funcionará** desde `file://` en móvil. Ver Opción B.

### Opción B — Despliegue en HTTPS (recomendado para móvil)

**Netlify Drop** (más fácil, 1 minuto):
1. Ve a [app.netlify.com/drop](https://app.netlify.com/drop)
2. Arrastra `verificador-billetes.html`
3. Obtén tu URL `https://...netlify.app`
4. Ábrela en tu celular ✅

**GitHub Pages**:
1. Sube el repositorio a GitHub
2. Ve a `Settings → Pages → Branch: main`
3. Tu app estará en `https://tu-usuario.github.io/verificador-billetes-bcb/verificador-billetes.html`

---

## 📖 Cómo funciona

### Flujo de uso
```
1. Selecciona el tipo de billete (10, 20 o 50 Bs)
        ↓
2. Abre la cámara y encuadra el número de serie
        ↓
3. Toca "Capturar y escanear"
        ↓
4. La imagen se congela → OCR procesa el número
        ↓
5. Resultado: ✅ Válido  o  ⛔ Sin valor legal
```

### Tecnología de visión

El proceso de reconocimiento del número de serie ocurre en 4 pasos:

| Paso | Qué hace |
|------|----------|
| **1. Captura** | `getUserMedia` congela un frame del video en un `<canvas>` |
| **2. Preprocesamiento** | Escala 2×, escala de grises, contraste +2, umbralización global |
| **3. OCR** | Tesseract.js analiza la franja superior (45% de la imagen) donde vive el serial |
| **4. Validación** | El número extraído se compara contra los rangos del CP9/2026 |

### Lógica de validación

Los billetes de la **Serie B** son inválidos si su número de serie cae dentro de alguno de los rangos publicados por el BCB. Cualquier número fuera de esos rangos se considera válido.

```javascript
// Ejemplo simplificado
function esInvalido(tipo, numero) {
  return RANGOS[tipo].some(([desde, hasta]) => numero >= desde && numero <= hasta);
}
```

---

## 📋 Billetes cubiertos

| Denominación | Rangos inválidos |
|---|---|
| **Bs 50** | 10 rangos (ej: 67250001–67700000) |
| **Bs 20** | 16 rangos (ej: 87280145–91646549) |
| **Bs 10** | 12 rangos (ej: 77100001–77550000) |

Fuente: [Comunicado de Prensa CP9/2026 — Banco Central de Bolivia](https://www.bcb.gob.bo)

---

## 🗂️ Estructura del proyecto

```
verificador-billetes-bcb/
│
└── verificador-billetes.html   # App completa (HTML + CSS + JS en un solo archivo)
```

El proyecto es intencionalmente **un solo archivo** sin dependencias locales. Tesseract.js se carga desde CDN.

---

## 💡 Consejos para mejor lectura OCR

- ☀️ Buena iluminación, evita reflejos sobre el billete
- 📏 Distancia de 10–15 cm entre el celular y el billete
- ↔️ El número de serie debe quedar horizontal y centrado en el recuadro dorado
- 🪟 Encuadra **solo la franja del número**, no todo el billete
- Si el OCR falla, usa el ingreso manual — es igual de confiable

---

## ⚠️ Aviso legal

Esta herramienta es de carácter **orientativo**. Está basada en la información oficial publicada por el Banco Central de Bolivia en el CP9/2026. Ante cualquier duda sobre la autenticidad de un billete, consulta directamente con una entidad bancaria autorizada.

---

## 🤝 Contribuciones

¡Son bienvenidas! Si encuentras un error en los rangos, tienes una mejora de OCR o quieres agregar soporte para otras denominaciones, abre un **Issue** o un **Pull Request**.

---

## 📄 Licencia

MIT — libre para usar, modificar y distribuir.
