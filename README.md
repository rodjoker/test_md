# RETO 2 (backend) — CRUD de inventario desde cero

**Tiempo objetivo: 40-45 min.** Sin IA, un cambio a la vez, habla en voz alta como si el CTO te viera.
Esto es lo que RR.HH. anunció: *"crear un CRUD en backend"*.

## Lo que ya tienes
Servidor Express 5, conexión a Mongo y un manejador de errores MÍNIMO (`src/middleware/error.middleware.js`: solo respeta `err.status`, todo lo demás es 500). `src/routes/index.js` está vacío. Todo lo demás lo escribes tú.

## Arranque
1. Terminal 1: `npm run db` (Mongo desechable en memoria, puerto 27018).
2. Terminal 2: `npm run dev` → `GET http://localhost:4000/health` debe responder `{"success":true}`.
3. Prueba con curl, Postman o un `.http`.

## Qué construir: recurso `products`
Campos: `sku` (texto), `name` (texto), `price` (número), `stock` (entero), `category` (texto, opcional), más `createdAt/updatedAt`.

| Método y ruta | Qué hace | Respuesta OK |
|---|---|---|
| `POST /api/products` | Crea | **201** `{ success: true, data: producto }` |
| `GET /api/products` | Lista paginada | **200** `{ success: true, data: [...], meta: { page, limit, total, totalPages } }` |
| `GET /api/products/:id` | Uno | **200** `{ success, data }` |
| `PATCH /api/products/:id` | Actualiza parcial | **200** `{ success, data }` (ya actualizado) |
| `DELETE /api/products/:id` | Borra | **204** sin cuerpo |
| `POST /api/products/:id/stock/decrement` | Descuenta stock con `{ "quantity": n }` | **200** `{ success, data }` con el stock nuevo |

Los errores van con `{ success: false, message }`.

## Reglas
**Validación (400 si falla):** `sku` obligatorio · `name` obligatorio, 2-100 caracteres · `price` obligatorio, **número** (no string) y ≥ 0 · `stock` entero ≥ 0, por defecto 0 · `category` opcional.
**SKU:** único **sin distinguir mayúsculas** (`abc-1` y `ABC-1` son el mismo) → **409** si ya existe. **No se puede cambiar** con PATCH (se ignora).
**category:** se guarda y se filtra en minúsculas.
**PATCH:** mismas validaciones que al crear, pero todo opcional. Cuerpo vacío → 400.
**Ids:** formato inválido → **400**. Formato válido pero inexistente → **404** (en GET, PATCH, DELETE y decrement).
**Lista:**
- `page` (por defecto 1) y `limit` (por defecto 10, **máximo 50**). Valores basura (`abc`, `-4`) no deben romper nada: caen a valores válidos.
- `category=x` filtra.
- `q=texto` busca en `name` sin distinguir mayúsculas. **Ojo:** `q=(((` no puede reventar la API.
- `sort=price` (ascendente), `sort=-price` (descendente), `sort=name`. Sin `sort`: los más nuevos primero.
- `meta.total` es el total que cumple el filtro, no el de la página.
**Decrement:** `quantity` entero ≥ 1 (si no, 400). Si no hay stock suficiente → **409** y el stock NO cambia. Piensa qué pasa si **20 peticiones llegan a la vez** y quedan 10 unidades: tienen que salir exactamente 10 éxitos y 10 conflictos, nunca stock negativo.
**JSON malformado** → 400. **Ruta desconocida** → 404.

## Estructura sugerida (la que usas en tus proyectos)
`models/` → `controllers/` (o `services/`) → `routes/`, y montar en `routes/index.js`.

## Al terminar
1. Pásame un "listo" y yo corro mi batería de pruebas contra tu copia.
2. Prepara tu resumen de 5 líneas: qué decidiste y por qué (en especial el decrement y el 409 del SKU).
3. Tu frase de cierre en voz alta: *"Esto es lo que haría diferente con más tiempo: ..."*
