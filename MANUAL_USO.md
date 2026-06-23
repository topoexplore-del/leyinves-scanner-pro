# 📖 Manual de Uso — Leyinves Scanner Pro v18

Este manual explica cómo leer e interpretar cada parte del sistema. La filosofía es simple: el scanner **filtra y prioriza** ideas de inversión LONG (solo compras), pero la decisión final siempre es tuya. Ninguna métrica aquí es una garantía.

## 1. El régimen de mercado (lo primero que debes mirar)

En la parte superior del dashboard y en cada alerta verás el **régimen de mercado**, calculado con el método de Faber (2007) sobre el SPY:

- 🟢 **BULL**: el SPY está sobre su media de 200 días y la de 50 sobre la de 200. Las señales LONG operan con criterios normales. Históricamente, la gran mayoría del retorno del mercado se produce en este régimen.
- 🟡 **NEUTRAL**: el mercado está en transición. El sistema exige un requisito extra: solo alerta señales con fuerza relativa en el percentil 60 o superior.
- 🔴 **BEAR**: el SPY está bajo su SMA200 con estructura bajista. **Las alertas de compra se suspenden automáticamente.** Un sistema LONG-only operando en mercado bajista es la receta histórica para los peores drawdowns; en este régimen la mejor posición suele ser efectivo.

Esta es la mejora de mayor impacto del sistema: en el archivo `backtest.json`, el bucket `regime_filtered` te muestra cómo habría rendido la misma señal técnica operando *solo* en régimen BULL, comparado con `tech_signals` (sin filtro) y `baseline` (comprar siempre). Si el filtro funciona para un ticker, verás win-rate superior con menos operaciones.

## 2. Calidad Buffett y margen de seguridad

Cada acción (no aplica a ETFs) recibe un **Buffett Quality Score (0-100)** inspirado en los criterios de Warren Buffett y Charlie Munger. Evalúa diez condiciones de negocio: ROE > 15% (rentabilidad del capital), ROA > 7%, margen bruto > 40% (el indicador clásico de foso competitivo o "moat"), margen operativo > 15%, margen neto > 10%, deuda/patrimonio < 0.8x, liquidez corriente > 1.2, FCF yield > 4% (el negocio genera caja real, no solo utilidades contables), earnings yield > 5%, y crecimiento de ingresos > 5%.

Veredictos: 🏰 **MOAT** (≥75) — negocio excepcional tipo Buffett; ✅ **QUALITY** (≥55) — empresa sólida; ➖ **PROMEDIO** (≥35); ⚠️ **DÉBIL** (<35) — evítala aunque el técnico se vea bien.

El **MOS (Margin of Safety)** estima el descuento o sobreprecio frente al valor intrínseco usando el Número de Graham (√(22.5 × EPS × valor en libros por acción)), con fallback a valoración por flujo de caja libre. Un MOS de +20% sugiere que pagas 20% menos que el valor estimado; un MOS de −30% sugiere que pagas caro. Es una estimación gruesa — Graham la diseñó para negocios estables; en empresas de crecimiento tiende a ser pesimista.

La combinación más potente del sistema: **señal técnica 4/4 + régimen BULL + veredicto MOAT/QUALITY + MOS positivo**. Eso une el "cuándo" (técnico + régimen) con el "qué" (calidad + precio razonable), que es exactamente la síntesis momentum + value que la literatura financiera ha documentado como más robusta.

## 3. Fuerza relativa vs SPY (RS Rank)

Cada ticker muestra su retorno de 3 y 6 meses **en exceso del SPY**, y un percentil 0-100 contra todo el universo (~480 tickers). Un `rs_rank` de 85 significa que el ticker ha superado al 85% del universo. El momentum transversal (comprar lo relativamente fuerte) es uno de los factores con más evidencia histórica (Jegadeesh & Titman, 1993). Como regla práctica: prefiere señales con rs_rank ≥ 70 y desconfía de "gangas" técnicas con rs_rank < 30 — lo barato que sigue cayendo suele seguir cayendo.

## 4. Las pestañas del dashboard

**Radar**: la tabla principal por grupos. Columnas clave: *Score* (0-100, técnico), *AI* (probabilidad heurística 5-95%), *State* (WAIT → ACCUM → ENTRY → ENTRY+), *ABC* (estructura de medias: A = alineadas alcistas), RSI, ADX, volumen relativo y rendimientos 1D/5D/20D. Navega con ↑↓, pulsa M para multi-gráfico, Esc para volver.

**Analysis**: vista fundamental con P/E, ROE, ROA y crecimiento de EPS con semáforo de colores, ordenable por cualquier métrica.

**Entry Zones**: candidatas con zonas de operación. Desde v17.1 las zonas se calculan con ATR: entrada en leve pullback (−0.3 ATR), TP1 a +1 ATR, TP2 a +2 ATR y stop a −1.5 ATR de la entrada. El stop respira con la volatilidad real de cada activo.

**Backtest**: resultados históricos por ticker y horizonte (1 semana a 1 año). Cómo leerlo correctamente: compara siempre la estrategia contra su `baseline` (comprar cada semana sin filtro). Un win-rate de 65% no significa nada si el baseline del mismo ticker es 63%; el *edge* es la diferencia. El bucket `regime_filtered` debería mostrar mejor ratio que `tech_signals` en la mayoría de tickers — si no lo hace en uno concreto, ese ticker no respeta el régimen general (típico en defensivas y oro).

## 5. Las alertas (email y Telegram)

Una alerta de COMPRA solo se envía cuando las 4 capas validan: **Radar** (estado ENTRY/ENTRY+ con score ≥60 y AI ≥65), **Analysis** (composite fundamental ≥55, ROE ≥8%, ROA ≥3%, sin señales de contabilidad sospechosa ni deuda excesiva), **Entry Zones** (upside ≥8% con momentum 20D positivo) y **Game Theory** (probabilidad ≥65%, valor esperado positivo y Kelly ≥10%). Además, el régimen debe ser BULL o NEUTRAL (con RS alto).

Cada alerta incluye: precio y zonas (entry/TP1/TP2/SL por ATR), calidad Buffett y MOS, percentil de fuerza relativa, periodos de tenencia recomendados, y un **tamaño de posición sugerido** calculado para arriesgar el 1% del capital hasta el stop (se expresa en acciones por cada $10,000 — escala proporcionalmente a tu cuenta). Arriesgar un porcentaje fijo y pequeño por operación es la regla de supervivencia más importante del trading.

Las alertas están deduplicadas: una misma señal solo se notifica una vez al día, aunque el chequeo corra varias veces.

## 6. Flujo de trabajo sugerido

Al final del día (después de las 16:30 ET) revisa el régimen primero. Si es BEAR, no hay nada que hacer — paciencia. Si es BULL/NEUTRAL, mira las alertas confirmadas 4/4; de ellas, prioriza las que tengan Buffett ≥55, MOS ≥0 y rs_rank ≥70. Verifica el gráfico en la pestaña Radar (TradingView integrado) para confirmar que la estructura te convence. Si entras, usa la zona de entrada, coloca el stop ANTES que nada, y dimensiona con la regla del 1%. Anota la operación; el sistema te da el plan completo (entrada, objetivos, stop, horizonte), lo difícil es respetarlo.

## 7. Limitaciones que debes conocer

El backtest usa fundamentales actuales como proxy histórico (la API gratuita no da fundamentales históricos), las operaciones del backtest se solapan entre horizontes (los win-rates no son trades independientes), el universo tiene sesgo de supervivencia, y la "AI Probability" es una heurística calibrada, no un modelo entrenado. El Buffett Score depende de los datos de Yahoo, que ocasionalmente faltan o llegan desfasados para empresas no estadounidenses. Nada de esto invalida el sistema como **filtro de ideas**, pero sí invalida usarlo como máquina automática de decisiones.

⚠️ **Este sistema es educativo e informativo. No es asesoría financiera. Toda inversión en bolsa implica riesgo de pérdida del capital.**
