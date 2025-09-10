# Optimización de Cadena de Suministro

Este proyecto forma parte de mi portafolio como **Data Analyst** y tiene como objetivo analizar y optimizar la cadena de suministro a través de **SQL y Tableau**.  
Se enfoca en medir indicadores clave de desempeño (**KPIs**) como nivel de servicio (SLA), costos logísticos, tiempos de ciclo e inventario.

---

## Estructura del repositorio

/optim_supply_chain/
│── optim_supply_chain.sql # Script SQL con cálculos de KPIs
│── README.md # Documentación del proyecto
│── /data/ # Datasets de ejemplo


---

## Descripción técnica

El archivo **`optim_supply_chain.sql`** calcula los siguientes KPIs:

- **% SLA Cumplido** = pedidos entregados dentro de 48 horas.  
- **Tiempo de Ciclo Promedio** = horas entre orden y entrega.  
- **Costo Logístico Unitario** = costo de despacho promedio por pedido.  
- **Rotación de Inventario** = unidades vendidas vs stock promedio.  

El query está diseñado para ejecutarse en **Google BigQuery**, pero puede adaptarse a otros motores SQL.

---

## Cómo ejecutar el SQL

### En BigQuery (UI):
1. Abre [Google BigQuery](https://console.cloud.google.com/bigquery).
2. Crea un dataset (ejemplo: `supply_chain`).
3. Copia y pega el contenido de `optim_supply_chain.sql`.
4. Ajusta los nombres de tablas (`dataset.orders`, `dataset.inventory`) según tus datos.
5. Ejecuta la consulta.

### En terminal con `bq CLI`:
```bash
bq query --use_legacy_sql=false < optim_supply_chain.sql



