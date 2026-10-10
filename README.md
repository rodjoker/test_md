# Reto técnico: "esta API está rota, ponla a correr"

Backend Express + MongoDB (Mongoose) con autenticación JWT. Alguien lo dejó roto.
Tu trabajo: diagnosticarlo, corregirlo y demostrar que funciona. **Habla en voz alta mientras lo haces**
(en la entrevista se evalúa el método, no solo el resultado).

## Reglas
- **Sin IA.** Solo documentación oficial, la consola y tu cabeza.
- Cambia **una cosa a la vez** y prueba después de cada cambio.
- No reescribas archivos enteros: arregla la causa, no tapes el síntoma.
- Apunta cada problema que encuentres: síntoma, causa y arreglo.
- Tiempo sugerido: 45 minutos.

## Cómo levantar el entorno
Necesitas dos terminales en esta carpeta.

1. Terminal A, base de datos de práctica (en memoria, desaparece al parar con Ctrl+C):
   ```
   npm run db
   ```
   Escucha en `mongodb://127.0.0.1:27018`.
2. Terminal B, la API:
   ```
   npm start
   ```
   Debe quedar en `http://localhost:4000`. Si cambias código tienes que reiniciarla.

## Qué debe funcionar al final
| Prueba | Resultado esperado |
|---|---|
| `GET /health` | 200 |
| `POST /api/auth/register` con name, email y password (mín. 8) | 201 + token |
| `POST /api/auth/login` | 200 + token |
| Login con clave mala 3 veces | 423 (cuenta bloqueada) |
| `GET /api/users` con `Authorization: Bearer <token>` | 200 + lista |
| `GET /api/users` sin token | 401 |
| `PATCH /api/users/:id` sobre **tu propio** usuario | 200 |
| `PATCH /api/users/:id` sobre **otro** usuario (siendo rol user) | 403 |
| `DELETE /api/users/:id` de un id que no existe | 404 |

Ejemplo de petición:
```
curl -X POST http://localhost:4000/api/auth/register -H "Content-Type: application/json" ^
  -d "{\"name\":\"Ana Prueba\",\"email\":\"ana@test.com\",\"password\":\"Password123\"}"
```

## Al terminar
Resume en 5 líneas: qué estaba roto, cómo lo encontraste y qué cambiarías para que no vuelva a pasar.
No leas nada fuera de esta carpeta que te dé la respuesta. Cuando termines me avisas y comparamos con la hoja de respuestas.
