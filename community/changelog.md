# Historial de versiones

Todas las modificaciones importantes en CryptoCode se documentan aquí.

---

## [2.5.0] - 2026-03-27

### Añadido
- Machine Learning: Signal Classifier (57%), LSTM (63%), Market Regime (75%) con auto‑retrain semanal.
- Risk Management Tier 1: 9 funciones avanzadas (tamaño dinámico, ajuste por volatilidad, salidas parciales, etc.).
- Dashboard: 11 páginas, diseño unificado, modo testnet, panel de live trading (Premium).
- Autenticación: JWT con httpOnly, encriptación de API keys AES-256-GCM.
- Base de datos: PostgreSQL, 18 índices, auto‑cleanup, auditoría completa.
- VPS: Configuración de producción con firewall, fail2ban, SSL.

### Cambiado
- Migración de SQLite a PostgreSQL.
- Eliminación del módulo Telegram (deprecado).
- Optimización de queries: 100x más rápidas.

### Corregido
- 17 bugs críticos y menores, incluyendo data leakage en ML, cálculo de heat, y fallos en scheduling.

---

## [2.2.0] - 2025-12-15

### Añadido
- Pipeline de snapshot con 6 indicadores.
- 8 plantillas de estrategia (aggressive, moderate, conservative, etc.).
- Risk Management Tier 0: SL/TP, trailing stops, heat limit.
- Portfolio virtual y dashboard básico.
- 598 tests con 95% de cobertura.

---

## [2.0.0] - 2025-06-01

Primer lanzamiento estable con análisis técnico básico.

---

Para una lista completa de cambios por fecha, consulta el registro interno de implementaciones.