-- ══════════════════════════════════════════
-- Poyecto-RetailPro — Extrayendo métricas clave con SQL
-- Autor: Valdez Rocio
-- Fecha: 23/06/2026
-- ══════════════════════════════════════════

-- Consulta 1 — Resumen ejecutivo mensual
-- Total facturado, cantidad de pedidos y ticket promedio, agrupados por mes.
SELECT
  EXTRACT(MONTH FROM fecha_ventas) AS mes,
  SUM(cantidad * precio_unitario) AS total_facturado,
  COUNT(*) AS cantidad_pedidos,
  SUM(cantidad * precio_unitario) / COUNT(*) AS ticket_promedio
FROM ventas
GROUP BY EXTRACT(MONTH FROM fecha_ventas)
ORDER BY mes;


-- Consulta 2 - Ranking de productos
-- Unidades vendidas, Total generado.
SELECT
id_producto,
SUM(cantidad) AS unidades_vendidas,
SUM(cantidad * precio_unitario) AS total_generado
FROM ventas
GROUP BY id_producto
ORDER BY total_generado DESC
LIMIT 5;


-- Consulta 3 - Clientes recurrentes
-- Cantidad de pedido, total gastado
SELECT
  id_cliente,
  COUNT(*) AS cantidad_pedidos,
  SUM(cantidad * precio_unitario) AS total_gastado
FROM ventas
GROUP BY id_cliente
HAVING COUNT(*) > 1
ORDER BY total_gastado DESC;


-- Consulta 4 — Meses por encima/por debajo del promedio mensual
-- Total facturado por mes, promedio general
WITH ventas_por_mes AS (
SELECT
    EXTRACT(MONTH FROM fecha_ventas) AS mes,
    SUM(cantidad * precio_unitario) AS total_facturado
  FROM ventas
  GROUP BY EXTRACT(MONTH FROM fecha_ventas)
promedio_mensual AS (
  SELECT AVG(total_facturado) AS promedio_general
  FROM ventas_por_mes
)
SELECT
  v.mes,
  v.total_facturado,
  CASE
    WHEN v.total_facturado > p.promedio_general THEN 'Por encima'
    ELSE 'Por debajo'
  END AS etiqueta
FROM ventas_por_mes v
CROSS JOIN promedio_mensual p
ORDER BY v.mes;




-- Bloque de cierre — Hallazgos
-- 1) Como todas las compras corresponden al mes 3, la comparativa 'Por encima/Por debajo' contra el promedio mensual
--    no discrimina entre meses: el mes 3 queda etiquetado como 'por debajo'. Total mes 3: 6444.00.
-- 2) El producto 1 concentra el mayor volumen del Top 5, con 3 unidades vendidas y un total generado de 3600.00.
-- 3) El cliente 1 es recurrente (más de un pedido): realizó 2 pedidos y gastó un total de 2640.00.
