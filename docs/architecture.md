\# Arquitectura General de CryptoCode



CryptoCode es una plataforma modular que transforma datos de mercado en recomendaciones de trading, con gestión de riesgo integrada. Este documento describe la arquitectura de alto nivel, sin entrar en detalles de implementación.



\---



\## 🏗️ Visión general



La plataforma se compone de \*\*dos pipelines principales\*\* que se ejecutan de forma asíncrona y coordinada:



1\. \*\*Pipeline de snapshot\*\* – Obtiene datos de mercado y calcula indicadores técnicos cada 15 minutos.

2\. \*\*Pipeline de estrategia\*\* – Para cada usuario y cada estrategia activa, genera recomendaciones personalizadas.



Ambos pipelines se apoyan en una base de datos PostgreSQL optimizada y un dashboard web.



┌─────────────────────────────────────────────────┐

│ ORQUESTADOR (Scheduler) │

│ Coordina snapshots + estrategias + cleanup │

└────────────┬────────────────────────────────────┘

│

┌────────┴────────┐

▼ ▼

┌─────────────┐ ┌──────────────────────────────┐

│ Snapshot │ │ Pipeline de Estrategia │

│ Global │ │ (por usuario y estrategia) │

└────┬────────┘ └──────────────┬───────────────┘

│ │

└──────────┬────────────────┘

▼

┌──────────────┐

│ PostgreSQL │

└──────────────┘





\---



\## 📡 Pipeline de Snapshot



\- \*\*Fuente de datos:\*\* Binance (API pública, OHLC de \~600 pares).

\- \*\*Indicadores calculados:\*\* RSI, MACD, Supertrend, PMAX, volatilidad, z‑score, volumen relativo.

\- \*\*Frecuencia:\*\* Cada 15 minutos (configurable).

\- \*\*Almacenamiento:\*\* Tablas `market\_snapshot` (por par) y `global\_snapshot` (resumen de mercado).

\- \*\*Retención:\*\* 14 días, con auto‑cleanup diario.



\---



\## 🧠 Pipeline de Estrategia (12 etapas)



Cada estrategia de un usuario pasa por este flujo, generando una propuesta de compra/venta/mantener con gestión de riesgo aplicada.



| Etapa | Descripción |

|-------|-------------|

| 1. Cargar configuración | Combina plantilla base con overrides del usuario. |

| 2. Obtener snapshot | Último snapshot de mercado disponible. |

| 3. Discovery | Filtrado inicial por volumen, liquidez, score técnico. |

| 4. Filters | Elimina activos con señales contradictorias o baja calidad. |

| 5. Analysis | Calcula métricas adicionales (momentum, divergencias, etc.). |

| 6. Scoring | Puntaje unificado 0‑10 combinando indicadores y ML. |

| 7. Signals | BUY/SELL/HOLD según umbrales. |

| 8. Selection | Selecciona top N activos respetando límites. |

| 9. Risk Management | Ajusta tamaños, SL/TP, valida heat. |

| 10. Proposal Build | Estructura recomendaciones finales. |

| 11. Simulation | Aplica propuesta al portfolio virtual. |

| 12. Persistence | Guarda run, propuesta y actualiza portafolios. |



\---



\## 🛡️ Risk Management Layer



Incluye \*\*15 funciones\*\* organizadas en dos tiers:



\- \*\*Tier 0 (fundamentos):\*\* Stop‑loss, take‑profit, trailing stops, portfolio heat, auto‑exit, métricas de riesgo.

\- \*\*Tier 1 (avanzado):\*\* Tamaño dinámico, ajuste por volatilidad, salidas parciales, ajuste por correlación, coordinación multi‑estrategia, etc.



Todas operan en tiempo real durante la etapa 9 del pipeline.



\---



\## 🤖 Machine Learning



Tres modelos entrenados con datos históricos:



\- \*\*Signal Classifier (MLP)\*\* – predice BUY/SELL/HOLD con 57% de accuracy.

\- \*\*LSTM\*\* – captura dependencias temporales, 63% accuracy.

\- \*\*Market Regime Classifier\*\* – detecta tendencia / rango / alta volatilidad, 75% accuracy.



Los modelos se reentrenan automáticamente cada semana con nuevos datos (walk‑forward validation). Su salida se integra en el scoring final.



\---



\## 🗄️ Base de Datos



\*\*PostgreSQL 14+\*\* con optimizaciones:



\- 18 índices esenciales para queries <1ms.

\- Retención automática (market: 14d, runs: 60d, audit: 90d).

\- Auditoría completa (`audit\_log`).

\- JSON validation y foreign keys con cascade.



\---



\## 🔌 Dashboard y API



El dashboard web (Express.js) ofrece 11 páginas con autenticación JWT. La API pública documentada en \[api-documentation.md](api-documentation.md) expone endpoints para:



\- Estado del sistema.

\- Listado de estrategias disponibles.

\- Resultados de backtesting.

\- (Futuro) WebSockets para datos en tiempo real.



\---



\## 🚀 Infraestructura



\- \*\*Hosting:\*\* VPS en Contabo (Ubuntu 22.04).

\- \*\*Seguridad:\*\* Firewall (solo puertos 22,80,443), fail2ban, actualizaciones automáticas.

\- \*\*SSL:\*\* Let's Encrypt con renovación automática.

\- \*\*Monitoreo:\*\* Sentry + endpoints `/health`.



\---



\## 🔒 Seguridad



\- API keys encriptadas con AES-256-GCM.

\- JWT almacenado en cookie httpOnly.

\- Rate limiting por IP y usuario.

\- Auditoría de todas las acciones críticas.



\---



\## 📚 Más información



\- \[API pública](api-documentation.md)

\- \[Roadmap](../community/roadmap.md)

\- \[Changelog](../community/changelog.md)

