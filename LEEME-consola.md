# Consola de aplicaciones — Hermanos Vizcaíno, S.C.A.

Página de inicio del departamento administrativo: accesos a todas las webs y un panel "Hoy" con lo urgente.

Dirección prevista: `https://marvizcainodat.github.io/consola/`

## Qué muestra

| Bloque | De dónde sale |
|---|---|
| Aplicaciones | Lista `APPS` al principio del `<script>` de `index.html` |
| Correo de hoy (con botón Imprimir) | Pestaña `consolaResumen` de datt, fila `hoy`. La escribe la tarea programada de las 8:45 |
| Calendario · 3 días | La misma fila `hoy` (campo `calendario`), escrita por la misma tarea |
| Vencimientos · 3 días | Facturas emitidas y recibidas de datt (`invoicesOut`, `invoicesIn` y sus líneas de cobro/pago). Misma lógica que Tesorería |
| Albaranes | Reservado para la fase 2 |

## Por qué el correo no lo lee el script de datt
El Gmail de la oficina (hnosvizcaino) y la cuenta de datos (vizcainodat) están separados a propósito, para que un ataque de correo masivo no bloquee la cuenta de los datos. Por eso el script de datt nunca toca el correo: la tarea de las 8:45 lee el correo y el calendario con la cuenta de la oficina y deja solo el texto del resumen en datt.

## Seguridad
La consola usa la **misma dirección y clave que Tesorería**, guardadas en el navegador con la misma clave interna (`panelTesoreriaCfg`). Como las dos páginas están en `marvizcainodat.github.io`, si ya entraste en Tesorería en ese navegador, la consola entra sola. Sin clave solo se ven los botones; los datos no.

## Formato de la fila `hoy` (pestaña `consolaResumen`)
Columna A: `hoy`. Columna B: JSON compacto:

```json
{
  "fecha": "2026-10-19",
  "generado": "2026-10-19T08:47",
  "correo": {
    "urgente":      [{"quien": "", "asunto": "", "accion": ""}],
    "sinContestar": [{"quien": "", "asunto": "", "accion": ""}],
    "semana":       [{"quien": "", "asunto": "", "accion": ""}],
    "contestados":  [{"quien": "", "asunto": "", "accion": ""}],
    "facturas":     [{"proveedor": "", "numero": "", "nota": ""}],
    "cerrado":      ["texto corto"],
    "nada": "texto si no ha entrado nada relevante"
  },
  "calendario": [{"fecha": "2026-10-19", "hora": "10:00", "titulo": "", "lugar": ""}]
}
```
Cualquier grupo puede ir vacío o no estar.

## Puesta en marcha
1. **Code.gs (una línea):** añadir `'consolaResumen'` al final de la lista `COLS` y crear una **nueva versión** del despliegue (Implementar → Administrar implementaciones → Editar → Nueva versión). Ver `PARCHE-Code.gs-consola.md`.
2. **GitHub:** crear el repositorio `consola` en la cuenta `marvizcainodat`, subir `index.html` (y `logo.png` si se quiere el logo) y activar GitHub Pages (Settings → Pages → rama `main`, carpeta raíz).
3. Abrir `https://marvizcainodat.github.io/consola/`.

## Ajustes rápidos (al principio del `<script>`)
- `APPS`: añadir o quitar botones. Cuando exista la web pública, poner su dirección en `url`.
- `DIAS_EFECTOS`, `DIAS_CALENDARIO`: días a mostrar (ahora 3).
- `REFRESCO_MIN`: cada cuántos minutos se actualiza sola (ahora 5).

## Fases siguientes
2. Programa en el PC que cada día vuelca a datt los albaranes y facturas nuevos de las carpetas → tarjetas "Sin valorar" y "Pendientes de facturar", con botones para marcar.
3. Presupuestos con sus líneas.
4. Contratos: esquema de cada contrato y fechas que generan avisos.
