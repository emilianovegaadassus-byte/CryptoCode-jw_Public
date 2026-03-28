# \# API Pública de CryptoCode

# 

# Esta API permite acceder a datos agregados del sistema, consultar estrategias y ejecutar backtests. \*\*No expone operaciones de trading real ni información sensible de usuarios.\*\*

# 

# \---

# 

# \## 📍 Base URL

# https://cryptocode-jw.com/api/v1

# 

# text

# 

# Todas las peticiones deben incluir el header `Authorization: Bearer <token>` (excepto `/auth/\*`).

# 

# \---

# 

# \## 🔐 Autenticación

# 

# Obtén un token JWT mediante:

# POST /auth/login

# 

# text

# 

# \*\*Body:\*\*

# ```json

# {

# &#x20; "email": "usuario@ejemplo.com",

# &#x20; "password": "tu\_contraseña"

# }

# Respuesta: Cookie httpOnly con el token.

# 

# 📊 Endpoints disponibles

# 1\. Estado del sistema

# text

# GET /health

# Devuelve información de conectividad y versión.

# 

# Ejemplo:

# 

# json

# {

# &#x20; "status": "healthy",

# &#x20; "version": "2.5.0",

# &#x20; "database": "connected",

# &#x20; "ml\_models": \["signal", "lstm", "regime"]

# }

# 2\. Listado de estrategias

# text

# GET /strategies

# Obtiene las plantillas de estrategia disponibles (moderate, aggressive, etc.) con sus parámetros públicos.

# 

# Respuesta:

# 

# json

# {

# &#x20; "strategies": \[

# &#x20;   {

# &#x20;     "name": "Moderate",

# &#x20;     "description": "Balance entre riesgo y oportunidad",

# &#x20;     "risk\_level": "medium",

# &#x20;     "params": { "max\_positions": 10, "max\_heat": 8.0 }

# &#x20;   },

# &#x20;   ...

# &#x20; ]

# }

# 3\. Ejecutar un backtest

# text

# POST /backtest

# Body:

# 

# json

# {

# &#x20; "strategy": "Moderate",

# &#x20; "start\_date": "2025-01-01",

# &#x20; "end\_date": "2025-12-31",

# &#x20; "initial\_capital": 10000,

# &#x20; "params\_overrides": { "max\_positions": 5 }

# }

# Respuesta:

# 

# json

# {

# &#x20; "final\_equity": 12500,

# &#x20; "total\_trades": 42,

# &#x20; "win\_rate": 0.62,

# &#x20; "max\_drawdown": 0.12,

# &#x20; "equity\_curve": \[\[timestamp, equity], ...]

# }

# 4\. Último snapshot global

# text

# GET /market/latest

# Devuelve el resumen del mercado más reciente: condición (UP/DOWN/NEUTRAL), porcentaje de activos en verde, etc.

# 

# 5\. Historial de predicciones ML (resumido)

# text

# GET /ml/history?limit=10

# Muestra las últimas predicciones del clasificador (solo datos agregados, sin información de usuario).

# 

# 🧪 Rate limits

# Por IP: 100 peticiones por minuto.

# 

# Por usuario: 200 peticiones por minuto.

# 

# Endpoints de backtest: 5 por hora por usuario.

# 

# 📝 Notas

# La API está en versión beta. Algunos endpoints pueden cambiar.

# 

# Los datos de backtest son simulaciones y no garantizan resultados futuros.

# 

# Para más información o soporte, escribe a contact@cryptocode-jw.com

