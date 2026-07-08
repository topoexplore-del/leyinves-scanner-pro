# 🔎 Manual del Screener — estilo Finviz

La pestaña **Screener** (en el dashboard de escritorio y en la app móvil) replica la mecánica de Finviz —Order by + Signal— sobre tu propio universo de ~450 tickers, e incluye lo que Finviz cobra en Elite: la **detección de patrones chartistas** calculada por tu propio pipeline.

## Ordenar por (Order by) — ~55 métricas

**Rendimiento**: cambio del día, gap de apertura, cambio desde apertura, performance semana / mes / trimestre / semestre / año.
**Técnicos**: score, AI, RSI(14), ADX, volumen relativo, volumen medio 3M, ATR, beta, volatilidad semanal y mensual, distancia a SMA 20/50/200, distancia a máximos y mínimos de 50 días y 52 semanas.
**Valoración**: market cap, P/E, Forward P/E, PEG, Price/Sales, Price/Book, dividend yield, payout ratio, EPS (TTM).
**Crecimiento y rentabilidad**: EPS growth, sales growth, ROE, ROA, margen bruto / operativo / neto.
**Salud financiera**: deuda/patrimonio, current ratio, quick ratio.
**Propiedad y mercado**: insider ownership, institutional ownership, short float, short ratio, float/outstanding, acciones en circulación, float, recomendación media de analistas (1=compra fuerte, 5=venta).
**Del sistema**: calidad Buffett, margen de seguridad, fuerza relativa, fundamental composite, upside proyectado, días a earnings.

Combínalo con la dirección (↓/↑), el filtro de sector y el buscador. Ejemplo del caso que planteaste: *Performance (Year)* + señal *Nuevo máximo 52 semanas* = las acciones más prometedoras del año que además están rompiendo máximos.

## Señal (Signal)

**De mercado**: Top Gainers, Top Losers, Más volátiles, Más activas (las cuatro se calculan sobre TU universo completo, como hace Finviz), volumen inusual (≥2x), nuevo máximo/mínimo de 52 semanas (y "cerca de"), ruptura del máximo de 20 días, sobre/bajo SMA200, cruce dorado y cruce de la muerte (SMA50 vs SMA200 en los últimos 10 días), sobrecompra (RSI≥70), sobreventa (RSI≤30), earnings en ≤7 días, y estado ENTRY del sistema.

**Patrones chartistas** (detectados por geometría de pivotes sobre ~140 velas): Doble techo, Doble suelo, Triple techo, Triple suelo, Hombro-Cabeza-Hombro, HCH invertido, Triángulo ascendente, Triángulo descendente, Triángulo simétrico, Cuña ascendente (bajista), Cuña descendente (alcista), Canal alcista, Canal bajista, Rectángulo alcista, Rectángulo bajista, Bandera alcista/bajista, Banderín alcista/bajista, **Taza con asa** (no está en Finviz — la añadimos), Soporte horizontal y Resistencia horizontal (≥3 toques). También puedes filtrar por "cualquier patrón", "cualquier alcista" o "cualquier bajista". Cada patrón lleva su confianza (%) en el tooltip.

## Cómo leerlo con criterio profesional

Un patrón detectado es **geometría, no confirmación**: el detector encuentra la figura, pero la ruptura con volumen —el momento operable— la confirmas tú en el gráfico (toca la fila para abrirlo). Los patrones y señales son EOD (anti-repaint): se recalculan con cada build. Las combinaciones más potentes: patrón alcista + fuerza relativa alta + régimen BULL, o Doble suelo + sobreventa + calidad Buffett ≥55 (reversión en negocio bueno).

## Qué NO está (honestidad sobre la fuente gratuita)

Upgrades/downgrades de analistas del día, compras/ventas de insiders recientes, "Major News", EPS/Revenue surprise y transacciones institucionales del periodo requieren fuentes de pago o la API de Finnhub con plan superior — Yahoo gratuito no los publica. Si algún día conectas una fuente que los tenga, el screener los aceptará como señales nuevas sin cambiar la interfaz (lee dinámicamente lo que venga en el snapshot).

## Nota sobre datos antiguos

Si el snapshot fue generado con una versión anterior, el screener lo detecta y te avisa: el ordenamiento funciona con las métricas disponibles, y las señales/patrones aparecen tras la primera build real (workflow *Full Refresh + Backtest*).
