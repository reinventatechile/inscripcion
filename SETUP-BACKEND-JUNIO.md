# 🔧 Backend evento JUNIO — Sheet nuevo + Flow

Objetivo: que las inscripciones del **27 de junio** lleguen a un **Google Sheet nuevo**
(sin mezclarse con las de mayo) y dejar **Flow** bien configurado.

La forma más rápida y segura es **DUPLICAR** tu sistema actual de mayo (que ya funciona:
guarda en el Sheet, manda emails y comprobantes) y apuntarlo a una copia nueva.

---

## PASO 1 — Duplicar el Google Sheet de mayo

1. Abre el Google Sheet del evento de mayo
   (el de la URL `...spreadsheets/d/1Tw8Wy7jneZHLPySBYDOKRaV.../`).
2. Menú **Archivo → Hacer una copia**.
3. Nómbrala: **Reinventate — Inscripciones JUNIO 2026**.
4. Marca "Copiar también los comentarios" no es necesario. Crea la copia.
5. En la copia: **borra las filas de inscritas de mayo** (deja solo la fila de
   títulos/encabezados). Así parte limpia, pero con la misma estructura.

> Al hacer "Hacer una copia", Google **copia también el Apps Script** que está
> pegado al Sheet (Extensiones → Apps Script). Por eso reutilizamos todo tu sistema.

---

## PASO 2 — Volver a desplegar el Apps Script de la copia (URL nueva)

1. En el Sheet NUEVO de junio: **Extensiones → Apps Script**.
2. Arriba a la derecha: **Implementar → Nueva implementación**.
3. Tipo: **Aplicación web**.
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquiera**
4. Clic en **Implementar**. Autoriza si lo pide.
5. Copia la **URL de la aplicación web** (termina en `/exec`).
   👉 **Esa URL me la pasas** y yo la conecto en el formulario (campo `APPS_SCRIPT_URL`).

---

## PASO 3 — Para dejar FLOW bien configurado (necesito ver el código)

El pago con Flow vive **dentro de ese Apps Script** (no en la web). Para arreglarlo bien:

1. En el Apps Script de la copia, abre el archivo **`Código.gs`** (o `Code.gs`).
2. **Selecciona todo (Ctrl+A) y cópialo**, y pégamelo en el chat.
3. Con eso reviso:
   - Que el **monto** sea $25.000 y la **moneda** CLP.
   - Que las `apiKey` / `secretKey` de Flow estén bien y NO en modo sandbox.
   - Que las URLs de retorno (`pago-exitoso` / `pago-fallido`) estén correctas.
   - Que los **emails y textos** digan "27 de junio · Hotel Borde Lago".

Si además tienes a mano tus credenciales de Flow (panel de Flow → tu comercio →
API Keys), confírmame si estás en **producción** o **sandbox** — el error típico de
Flow es quedarse en sandbox o tener la secretKey mal pegada.

---

## PASO 4 — Yo conecto todo

Cuando me pases (1) la **URL `/exec` nueva** y (2) el **código del Apps Script**:
- Pego la URL nueva en `inscripcion/index.html` (ya está marcado dónde).
- Te dejo el Apps Script corregido para Flow + emails del nuevo evento.
- Probamos una inscripción de prueba de punta a punta antes de publicar.

---

## ¿Y si no encuentras el Apps Script original?
Avísame y te escribo uno nuevo desde cero (te paso el `Código.gs` listo para pegar),
pero la vía de **duplicar** es más rápida y reutiliza lo que ya te funcionaba.

---

## ⚠️ IMPORTANTE — PRECIOS NUEVOS (3 etapas)

El formulario ahora maneja **precio dinámico** y lo **envía** al Apps Script en cada
inscripción (campo `precio`, además de `descuento` y `codigoDescuento`):

| Etapa | Fechas | Precio | Con código 20% |
|---|---|---|---|
| Early bird | hasta mié 24 jun | $22.900 | $18.320 |
| Última llamada | 25–27 jun | $25.900 | $20.720 |

- El **código de descuento** por defecto es **`REINVENTADA20`** (cámbialo en el `CONFIG`
  del formulario, en `DESCUENTO_CODES`). Es distinto del código de **cortesía** (ese es gratis).

**Por eso, en el Apps Script hay que revisar 2 cosas cuando me pases el código:**
1. **Flow (`crearPago`)**: el monto a cobrar debe ser **`data.precio`** que llega en el
   POST — NO un valor fijo de $25.000. (Si quedó fijo, Flow cobraría mal.)
2. **Guardar en el Sheet**: registrar también las columnas `precio`, `descuento` y
   `codigoDescuento` para saber cuánto y con qué código pagó cada inscrita.

Cuando me pegues el `Código.gs`, te lo dejo ajustado para esto.
