# Optimización de Cadena de Suministro 

Este proyecto forma parte de mi portafolio como **Analista de Datos** y tiene como objetivo analizar y optimizar la cadena de suministro a través de **SQL y Tableau**.
Se enfoca en medir indicadores clave de desempeño (**KPIs**) como nivel de servicio (SLA), costos logísticos, tiempos de ciclo e inventario.

---

## Estructura del repositorio

```
/optim_supply_chain/
│── optim_supply_chain.sql     # Script SQL con cálculos de KPIs
│── README.md                  # Documentación del proyecto
│── /data/                     # Carpeta opcional con datasets de ejemplo
```

---

## Descripción técnica

El archivo **`optim_supply_chain.sql`** calcula los siguientes KPIs:

* **% SLA Cumplido** = pedidos entregados dentro de 48 horas.
* **Tiempo de Ciclo Promedio** = horas entre orden y entrega.
* **Costo Logístico Unitario** = costo de despacho promedio por pedido.
* **Rotación de Inventario** = unidades vendidas vs stock promedio.

Esta Query se diseño para ejecutarse en **BigQuery**, pero puede adaptarse a otros motores SQL.

---

## Cómo ejecutar el SQL?

### En BigQuery (UI):

1. Abre [Google BigQuery](https://console.cloud.google.com/bigquery).
2. Crea un dataset (ejemplo: `supply_chain`).
3. Copia y pega el contenido de `optim_supply_chain.sql`.
4. Ajusta los nombres de tablas (`dataset.orders`, `dataset.inventory`) según tus datos.
5. Ejecuta la consulta.

### En terminal con `bq CLI`:

```bash
bq query --use_legacy_sql=false < optim_supply_chain.sql
```

---

## Dashboard en Tableau

Con los KPIs generados en `optim_supply_chain.sql`, se construirá un **Panel en Tableau** siguiendo estas instrucciones:

### Hojas sugeridas

1. **Tendencia de SLA (%)** por semana.
2. **Costo logístico unitario** por centro y canal.
3. **Tiempo de ciclo promedio** (horas entre pedido y entrega).
4. **Rotación de inventario** por centro logístico.

### Campos calculados en Tableau

```tableau
// SLA (%) en Tableau
SUM([SLA_OK]) / COUNT([order_id]) * 100

// Costo logístico unitario
SUM([shipping_cost]) / COUNT([order_id])
```

### Parámetros y filtros recomendados

* **Rango de fechas** → seleccionar periodo de análisis.
* **Canal** → filtrar por Retail, E-commerce, Wholesale, etc.
* **Centro logístico** → comparación entre CLRM01, CLRM02, CLRM03.

### Algunas sugerencias de diseño

* Usar **colores semáforo (verde, amarillo, rojo)** para SLA (%).
* Agregar **líneas de referencia** con el SLA objetivo (ejemplo: 95%).
* Incluir **tooltips detallados** con información por pedido.
* Diseñar con un layout de **cuatro secciones**: SLA, costo, tiempo de ciclo e inventario.

---

 Autor: **Joaquín Muñoz (Joaco)**
 Rol: Data Analyst (Scientist) | Portafolio Personal




