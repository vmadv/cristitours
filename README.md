# Cristitours — Panel de Reservas

Aplicación web estática (sin backend) para gestionar las reservas de los free tours de **Cristitours** en Las Palmas de Gran Canaria.

---

## Archivos

| Archivo | Descripción |
|---|---|
| `index.html` | Panel principal de reservas (app completa) |
| `reservas.html` | Vista alternativa / versión anterior del panel |
| `email-preview.html` | Vista previa del correo de encuesta de valoración |
| `survey-5stars.html` | Pantalla de agradecimiento tras dar 5 estrellas → redirige a Google Reviews |
| `Cristitours.png` | Logo de la marca |
| `Diseño sin título (1).png` | Imagen de diseño auxiliar |

---

## Tours gestionados

| Clave | Nombre completo |
|---|---|
| `centro` | Free Tour Centro Histórico |
| `canteras` | Free Tour Canteras |
| `portugues` | Free Tour Barrio Portugués |

---

## Fuentes de datos (Google Sheets vía JSONP)

Todas las lecturas se hacen mediante el endpoint `gviz/tq` de Google Sheets, sin autenticación (hojas públicas).

| Hoja | ID | Contenido |
|---|---|---|
| Tour Centro | `1m1zWlmDS6PUFSImDC1Su6REXDfA2UXVKNQXbDdrxhT0` | Reservas directas centro histórico |
| Tour Canteras | `12wV5XVLvkLch0ok3HaDge2mA2cPjH5VlgDr3M95uYto` | Reservas directas canteras |
| Tour Portugués | `1swBStgOQz9vBkUgShnjzDsnVOSAIu9zysnbBbP3UtPA` | Reservas directas barrio portugués |
| OTA (Civitatis/Guruwalk) | `16F7BZB0rP3HWY91yTu8GH-ICEZfMZcjzzjg457lfALE` | Reservas de plataformas externas |
| Valoraciones | `1pUSk4af86dHbZN_eHZD-tgwdmElof85PnzVEPAth42k` | Puntuaciones de la encuesta post-tour |
| Rendimiento / Ads | `1m27_fhTFtuGlVaXJUTPCQPX5BcePEfleGUb_K2Falw8` | Coste de campañas publicitarias |

---

## Funcionalidades principales (`index.html`)

### Autenticación
- Pantalla de PIN de 4 dígitos al abrir la app.
- El PIN se verifica localmente; la sesión se mantiene en `sessionStorage`.

### Panel de reservas
- **Estadísticas** en cabecera: total de reservas, personas y otras métricas del día/semana.
- **Barra de filtros**: Todas, Hoy, Próximas, Pasadas, Opiniones, Rendimiento.
- **Calendario** mensual con navegación para saltar a cualquier día.
- **Tarjetas de reserva** con:
  - Nombre del cliente, tour, fecha/hora, número de personas.
  - Badge de estado: Próxima / Hoy / Pasada / Cancelada.
  - Acciones rápidas: llamar, enviar WhatsApp, confirmar, cancelar, eliminar.

### Novedades
- Sección amarilla con reservas nuevas recién llegadas.
- Cada novedad puede descartarse individualmente o todas a la vez.
- Estado persistido en `localStorage` (clave `dismissed_nov_*`).

### Gestión de estados (persistidos en `localStorage`)
- **Confirmada**: la reserva fue contactada y confirmada.
- **Cancelada**: flujo con bottom sheet de opciones (WhatsApp al cliente, reasignación, etc.).
- **Eliminada**: oculta la tarjeta de la vista.

### Flujo de encuesta post-tour
1. El guía envía un correo con `email-preview.html` (tabla HTML con 5 estrellas clicables).
2. Si el cliente pulsa ★★★★★, llega a `survey-5stars.html`.
3. Esta página genera una reseña de texto, la copia al portapapeles y redirige a Google Reviews tras 2,5 s.

### Rendimiento / Ads
- Vista de coste de campañas leída desde la hoja de Ads.
- Comparativa de reservas vs. gasto publicitario.

---

## Estado local (`localStorage`)

La app no tiene backend; todo el estado mutable se guarda en `localStorage` del navegador.

| Prefijo de clave | Uso |
|---|---|
| `dismissed_nov_*` | Novedades descartadas |
| `confirmed_*` | Reservas marcadas como confirmadas |
| `cancelled_*` | Reservas marcadas como canceladas |
| `deleted_*` | Reservas eliminadas de la vista |

---

## Tecnología

- HTML + CSS + JavaScript vanilla (sin frameworks ni bundler).
- Datos externos vía JSONP (Google Visualization Query Language).
- Diseño mobile-first con system font stack (`-apple-system`, `BlinkMacSystemFont`).
- Sin dependencias npm ni proceso de build — se abre directamente en el navegador.
