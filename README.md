# KILOCAL — La central mayorista

**KILOCAL** es un marketplace mayorista B2B para kioscos y almacenes de barrio en Argentina. Los mayoristas publican sus catálogos; los kioscos comparan precios por caja, arman su carrito y envian el pedido por WhatsApp. **KILOCAL no pone camión ni toca plata**: el mayorista entrega y factura como siempre.

**URL pública**: https://alex451-bip.github.io/kilocal/

**Estado**: piloto con datos de muestra, lanzamiento comercial pendiente. No somos facturación ni entrega; pedidos por WhatsApp sujetos a confirmación del mayorista. Ver `PLAN_CHATGPT_KILOCAL.md` (plan de 90 días) y `terminos.html` / `privacidad.html`.

## Qué tiene hoy

- **Catálogo**: búsqueda, categorías con ícono, filtros (categoría/entrega/zona), paginado, catálogo fusionado de la constante + Firebase (sin duplicados).
- **Comparador de precios** por caja y por unidad, chips de mejor precio, **Repetir mi última compra**, **ahorro del carrito vs precio más caro**.
- **Pedidos independientes por mayorista** (cada proveedor tiene su WhatsApp propio), estados completos del pedido (`recibido → confirmado → preparando → en camino → entregado/cancelado`).
- **Panel mayorista**: publicar producto, toggle activo/pausado, ver pedidos recibidos y cambiar estados.
- **Cerebro KILOCAL** (asistente local por voz de texto): responde "¿precio de X?", "compará X", "mayoristas con entrega", "agrega lo más barato de X", sin conexión.
- **Pestañas**: Catálogo · Mayoristas · Publicá tu lista · Logística y cobertura (comisión 0% 1° mes).
- **Seguridad**: catálogo público de lectura, **pedidos accesibles solo con sesión autenticada** (verificado `401` público), auth anónima, Firebase RTDB.
- **Legal**: `terminos.html`, `privacidad.html` (borradores para revisión con abogado, Ley 25.326).

## Arquitectura

- **Single-file web app** en HTML/CSS/JS (GitHub Pages), sin build ni dependencias npm — portable, $0 infra (mientras respete cuotas gratuitas).
- **Firebase Realtime Database** (`kiosco-8ae45-default-rtdb`, namespace `/kilocal`) como base compartida: `/kilocal/products`, `/kilocal/orders`. Rules: products lectura pública / escritura con sesión y validación de estructura (`id`+`nombre`); orders lectura/escritura con sesión.
- **localStorage** como caché y carrito (offline-friendly).
- **WhatsApp deep links** (wa.me) para cerrar pedidos — sin API de WhatsApp durante el piloto.
- **Deploy**: GitHub Pages (`.nojekyll`, branch `main`).

## Datos y límites honestos

- 20 productos semilla de ejemplo: son **muestra comercial**, no stock real. Mayoristas reales cargan sus listas (Excel/PDF → importador futuro P1).
- No hay pedidos reales ni transacciones validadas todavía. **Roadmap** `PLAN_CHATGPT_KILOCAL.md`: 10 mayoristas + 30 kioscos + 50 pedidos entregados + 5 mayoristas que paguen.
- `/kilocal/products` público: cualquiera puede leerlo; solo sesiones autenticadas pueden escribir — pero la sesión es anónima, así que para datos comerciales serios (PII, precios de clientes) falta roles/email (Admin SDK o Blaze).

## Archivos

```
index.html      → la app completa
terminos.html   → términos y condiciones (borrador)
privacidad.html → política de privacidad (borrador)
README.md       → este archivo
PLAN_CHATGPT_KILOCAL.md → plan de 90 días de investigación/chatgpt (en repo aparte)
```

## Comandos

No requiere build. Para publicar: copiar `app/index.html` a `kilocal_pub/`, `git add . && git commit -m "…" && git push` (GitHub Pages rebuild automático).
