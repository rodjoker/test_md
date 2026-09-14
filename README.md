# Fichas y reglas — Prep entrevista técnica

Documento vivo. Se va a ir ampliando a medida que avanzamos de etapa (Express → NestJS → React → Next.js).

---

## 1. Ficha de arranque rápido — Express

### Setup inicial (comandos)

| Paso | Comando | Notas |
|---|---|---|
| Crear proyecto | `mkdir mi-backend && cd mi-backend` | — |
| Inicializar package.json | `npm init -y` | genera el manifest |
| Instalar Express | `npm install express` | framework HTTP |
| Instalar dotenv | `npm install dotenv` | manejo de variables de entorno |
| Instalar nodemon (dev) | `npm install -D nodemon` | reinicio automático en desarrollo |
| Habilitar ES Modules (opcional) | agregar `"type": "module"` en `package.json` | si preferís `import` sobre `require` |

### Estructura de carpetas sugerida

```
mi-backend/
├── src/
│   ├── index.js          # entry point: crea la app y la levanta
│   ├── config/           # configuración (env, conexión a DB)
│   ├── routes/           # definición de rutas (solo mapeo, sin lógica)
│   ├── controllers/      # lógica de cada endpoint
│   ├── middlewares/      # auth, validación, manejo de errores
│   └── models/           # esquemas de datos
├── .env                  # variables reales (NO se commitea)
├── .env.example           # plantilla de variables (SÍ se commitea)
├── .gitignore
└── package.json
```

### package.json — scripts sugeridos

| Script | Comando | Uso |
|---|---|---|
| `start` | `node src/index.js` | producción |
| `dev` | `nodemon src/index.js` | desarrollo (reinicio automático) |

### Variables de entorno típicas

| Variable | Ejemplo | Uso |
|---|---|---|
| `PORT` | `3000` | puerto en el que escucha el server |
| `NODE_ENV` | `development` / `production` | comportamiento condicional (logs, errores, etc.) |
| `DATABASE_URL` | `mongodb://localhost:27017/mi-db` | conexión a la base de datos |
| `JWT_SECRET` | string aleatorio largo | firma de tokens (cuando lleguemos a login) |

### Middlewares base que vas a necesitar sí o sí

| Middleware | Para qué |
|---|---|
| `express.json()` | parsear body JSON de las requests |
| `express.urlencoded({ extended: true })` | parsear bodies tipo form |
| Middleware de manejo de errores (al final de la cadena) | centralizar respuestas de error |

---

## 2. Reglas de seguridad y buenas prácticas (hasta login)

| Área | Regla | Por qué |
|---|---|---|
| Variables de entorno | Nunca hardcodear secrets; usar `.env` + `.gitignore` (nunca se commitea `.env`) | evita exponer credenciales en el repo |
| Contraseñas | Hashear con `bcrypt` (10-12 salt rounds), nunca texto plano ni nada reversible | si hay filtración de la DB, las contraseñas reales no quedan expuestas |
| JWT | Firmar con secret fuerte desde `.env`; expiración corta (ej. 15min-1h) + refresh token si aplica | limita la ventana de uso si el token se filtra |
| Sesiones | Decidir explícitamente stateless (JWT) vs stateful (session store); si usás cookies: `httpOnly` + `secure` + `sameSite` | mitiga robo de token vía XSS/CSRF |
| Validación de inputs | Validar y sanitizar todo input antes de tocar la DB (`express-validator`, `zod`, `joi`) | previene injection y datos corruptos |
| CORS | Configurar orígenes permitidos explícitamente, nunca wildcard (`*`) en producción | evita que cualquier dominio consuma tu API |
| Headers HTTP | Usar `helmet` | mitiga vectores comunes (clickjacking, sniffing de MIME, etc.) |
| Rate limiting | Limitar intentos en endpoints sensibles como `/login` (`express-rate-limit`) | previene fuerza bruta |
| Manejo de errores | Middleware central de errores; nunca devolver stack trace en producción | evita fuga de información interna |
| Logs | Nunca loguear passwords, tokens ni datos sensibles | evita fuga de credenciales vía logs |
| HTTPS | Obligatorio en producción | protege datos en tránsito |
| Mínimo privilegio | El usuario de la DB que usa la app no debe tener permisos de admin | limita el daño si la app se compromete |

---

## 3. Glosario de términos

| Término | Definición |
|---|---|
| **Middleware** | Función que recibe `req`, `res` y `next`. Puede modificar `req`/`res`, terminar el ciclo respondiendo, o llamar a `next()` para pasar el control al siguiente middleware/ruta en el stack. |
| **Stack (de middlewares)** | El orden en que Express registra `app.use()` / `app.get()` / etc. Una request recorre ese stack de arriba hacia abajo hasta que algo responde o hasta que ocurre un error. |
| **Route handler** | Un middleware especial (técnicamente lo es) que suele terminar el ciclo respondiendo, en vez de llamar a `next()`. Ej: `app.get('/health', (req, res) => {...})`. |
| **Middleware de errores** | Middleware que declara **4 parámetros** (`err, req, res, next`) en vez de 3. Express lo distingue exclusivamente por la cantidad de parámetros. Debe ir **al final** del stack: Express solo lo alcanza si el error ocurre *después* de donde está declarado, nunca busca hacia atrás. |
| **`next()`** | Función que delega el control al siguiente elemento del stack. `next(err)` (Express 4) delega específicamente a un middleware de errores. |
| **Captura automática de errores (Express 5)** | A diferencia de Express 4, en Express 5 tanto un `throw` síncrono como el rechazo de una promesa dentro de un handler `async` llegan solos al middleware de errores, sin `try/catch` ni `next(err)` manual. |
| **Hashing** | Transformación **unidireccional**: no existe operación inversa para recuperar el input original a partir del resultado. Se usa para contraseñas porque solo hace falta *comparar*, nunca *recuperar* el original. |
| **Cifrado (encryption)** | Transformación **reversible** mediante una clave (ej. AES). Si la clave se filtra, todo lo cifrado con ella queda expuesto de una sola vez. Por eso no se usa para contraseñas. |
| **Salt** | Valor aleatorio único que `bcrypt` genera por cada contraseña antes de hashear. Evita que contraseñas iguales generen el mismo hash, y neutraliza ataques con *rainbow tables*. |
| **Salt rounds (cost factor)** | Parámetro de `bcrypt` que controla cuánto trabajo computacional toma generar el hash. Deliberadamente lento — hace que la fuerza bruta sea impracticable en tiempo razonable. |

## 4. Backend — NestJS
*(pendiente — Etapa 5)*

## 5. Frontend — React
*(pendiente — Etapa 6)*

## 6. Frontend — Next.js
*(pendiente — Etapa 7)*
