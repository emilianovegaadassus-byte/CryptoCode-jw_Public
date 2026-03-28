# Política de Seguridad de CryptoCode

En CryptoCode, la seguridad de tus datos y fondos es nuestra prioridad. Este documento describe cómo reportar vulnerabilidades y nuestras prácticas de seguridad.

---

## 📢 Reporte de vulnerabilidades

Si descubres una vulnerabilidad de seguridad en la plataforma (incluyendo el sitio web, API, base de datos o infraestructura), **no la divulgues públicamente**. Por favor, envíanos un reporte a:

📧 **contact@cryptocode-jw.com** (con asunto `[VULN]`).

Incluye en tu reporte:
- Descripción clara del problema.
- Pasos para reproducirlo (si es aplicable).
- Posible impacto.
- Tu nombre y forma de contacto (si quieres reconocimiento).

**Tiempo de respuesta:** Nos comprometemos a responder en un plazo máximo de 72 horas.

---

## 🔐 Prácticas de seguridad implementadas

- **Encriptación de datos sensibles:** Las API keys de Binance se almacenan cifradas con **AES-256-GCM**. La clave de cifrado está aislada del código y la base de datos.
- **Autenticación:** Uso de **JWT en cookies httpOnly**, con rotación de tokens y expiración corta.
- **Protección contra ataques comunes:** Sanitización de entradas, validación estricta de JSON, rate limiting por IP y por usuario.
- **Auditoría completa:** Tabla `audit_log` que registra cada acción crítica (creación, modificación, eliminación) con valores antes/después.
- **Conexiones seguras:** Todo el tráfico se sirve exclusivamente por HTTPS (TLS 1.2+).
- **Hardening del servidor:** Firewall configurado (puertos 22, 80, 443 únicamente), actualizaciones automáticas, fail2ban para SSH.
- **Backups cifrados:** Diarios, almacenados en ubicación separada con retención de 7 días.

---

## 📝 Divulgación responsable

Una vez recibido un reporte, trabajaremos contigo para entender y solucionar el problema. Después de la corrección, publicaremos un aviso en el [changelog](community/changelog.md) y, si lo deseas, te daremos crédito por el hallazgo.

---

## 🚫 Lo que NO hacemos

- No ejecutamos órdenes en tu nombre sin tu consentimiento explícito.
- No almacenamos contraseñas en texto plano.
- No compartimos tus datos personales con terceros sin tu autorización.

---

**Gracias por ayudarnos a mantener CryptoCode seguro.**
