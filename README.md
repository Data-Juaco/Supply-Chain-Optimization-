-- Proyecto: Optimización de Cadena de Suministro
-- Descripción: KPIs base para análisis de nivel de servicio, inventario y costos logísticos

WITH pedidos AS (
  SELECT
    p.order_id,
    p.center_id,
    p.channel,
    DATE(p.order_date) AS order_date,
    DATE(p.delivery_date) AS delivery_date,
    p.status,
    p.units,
    p.shipping_cost,
    p.item_cost,
    -- SLA esperado = entrega dentro de 48 horas (ejemplo)
    CASE WHEN TIMESTAMP_DIFF(p.delivery_date, p.order_date, HOUR) <= 48 THEN 1 ELSE 0 END AS SLA_OK
  FROM `dataset.orders` p
),

inventario AS (
  SELECT
    i.sku,
    i.center_id,
    AVG(i.stock_units) AS avg_stock,
    SUM(i.units_sold) AS units_sold
  FROM `dataset.inventory` i
  GROUP BY 1,2
),

kpi_pedidos AS (
  SELECT
    center_id,
    channel,
    FORMAT_DATE('%Y-%W', order_date) AS semana,
    COUNT(order_id) AS total_pedidos,
    SUM(units) AS total_unidades,
    ROUND(SUM(SLA_OK)/COUNT(order_id)*100,2) AS SLA_pct,
    ROUND(AVG(TIMESTAMP_DIFF(delivery_date, order_date, HOUR)),1) AS ciclo_promedio_hrs,
    ROUND(SUM(shipping_cost)/COUNT(order_id),2) AS costo_log_unit
  FROM pedidos
  GROUP BY 1,2,3
),

kpi_inventario AS (
  SELECT
    center_id,
    ROUND(SUM(units_sold)/NULLIF(AVG(avg_stock),0),2) AS rotacion_inventario
  FROM inventario
  GROUP BY 1
)

SELECT 
  k.center_id,
  k.channel,
  k.semana,
  k.total_pedidos,
  k.total_unidades,
  k.SLA_pct,
  k.ciclo_promedio_hrs,
  k.costo_log_unit,
  i.rotacion_inventario
FROM kpi_pedidos k
LEFT JOIN kpi_inventario i
  ON k.center_id = i.center_id
ORDER BY semana, center_id, channel;


