# RETO 3 (React) — Rescata la app de inventario

**Tiempo objetivo: 30-40 min.** Sin IA, un cambio a la vez, **habla en voz alta** (qué ves, qué sospechas, qué pruebas).
Esto es lo que RR.HH. anunció: *"arreglar una app rota para ponerla a correr"*.
Es una **evaluación de nivel**: no hay mentoría previa. Si te atascas, pide una pista (te daré pistas graduadas, nunca el arreglo directo).

## Lo que debe hacer la app (cuando esté arreglada)
1. Mostrar una tabla con los productos que devuelve la API (8 de prueba).
2. El contador "N productos" y el buscador por nombre (sin distinguir mayúsculas) funcionan.
3. El botón **Eliminar** borra el producto en la API **y** desaparece de la pantalla al instante.
4. Si la API falla, se muestra un mensaje de error (ya está implementado).
5. La app pide la lista **una sola vez** (no en bucle).

## Cómo arrancar (2 terminales)
**Antes:** el puerto 4000 debe estar libre. Si tienes abierto el servidor del reto de backend (`npm run dev` de `test_backend_crud_inventario`), páralo con Ctrl+C.

1. **API de práctica** (Mongo en memoria + 8 productos, no toca nada real):
   ```
   cd C:\Users\danie\projects-backend\test_react_api
   npm run api
   ```
   Debe decir `API de practica en http://localhost:4000`. Prueba `http://localhost:4000/api/products` en el navegador.
2. **La app React:**
   ```
   cd C:\Users\danie\projects_frontend\test_react_tecnic_test
   npm run dev
   ```
   Abre la URL que te dé Vite (normalmente `http://localhost:5173`).

La respuesta de la API tiene esta forma: `{ "success": true, "data": [ ...productos ], "meta": { page, limit, total, totalPages } }`. Cada producto: `_id, sku, name, price, stock, category`.

## Reglas
- Hay **5 fallos** de tipos distintos, y aparecen **en cadena**: al arreglar uno se descubre el siguiente.
- No cambies el diseño ni reescribas componentes: arregla lo mínimo. No toques la API.
- Usa las herramientas del navegador: **Consola** y pestaña **Network** (F12). Léelas ANTES de tocar código.
- Un cambio, una prueba. Si algo ya funcionaba, no lo rompas.

## Al terminar
1. Dime "listo". Corro mi batería de pruebas sobre tu copia y comparo con el código limpio.
2. Resumen de 5 líneas: qué fallos encontraste, cómo los detectaste (qué herramienta/mensaje) y por qué ocurría cada uno.
3. Frase de cierre en voz alta: *"Esto es lo que revisaría la próxima vez antes de empezar a tocar código: ..."*
