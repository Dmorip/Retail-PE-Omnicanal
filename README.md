# Retail-PE-Omnicanal
# 🛒 Retail-PE-Omnicanal: Pipeline de Datos Medallion con PySpark

Este proyecto implementa una arquitectura de datos moderna (**Data Lakehouse**) para una cadena de retail en el mercado peruano. El objetivo es procesar ventas omnicanal (tiendas físicas y web) utilizando una arquitectura **Medallion (Bronze-Silver-Gold)** en **Databricks**.

## 🇵🇪 Contexto del Proyecto
La empresa cuenta con datos de ventas en diversos formatos (CSV/SQL) y logs de interacciones web (JSON). El desafío consistió en unificar estas fuentes, limpiar datos inconsistentes (nombres de distritos de Lima, formatos de fecha y nulos) y generar métricas de negocio accionables.

## 🏗️ Arquitectura de Datos
Se utilizó la metodología **Medallion** para garantizar la trazabilidad y calidad del dato:

1.  **Capa Bronze (Raw):** Ingesta de datos crudos en formato **Delta Lake**. Conservación de la historia completa con metadatos de auditoría (`ingestion_timestamp`).
2.  **Capa Silver (Cleansed):** 
    *   Estandarización de distritos (Ej: "surco" -> "SURCO").
    *   Casteo dinámico de fechas mediante `coalesce`.
    *   **Manejo de Cuarentena:** Los registros corruptos (montos nulos) se desvían a una tabla de errores para auditoría sin detener el pipeline.
3.  **Capa Gold (Curated):** Agregaciones de negocio para visualización. Cálculo de **Ticket Promedio** y **Ventas Totales por Tienda**.

## 🛠️ Stack Tecnológico
*   **Procesamiento:** Apache Spark (PySpark).
*   **Almacenamiento:** Delta Lake (Transacciones ACID).
*   **Plataforma:** Databricks.
*   **Lenguajes:** Python (Lógica de limpieza) y SQL (Creación de esquema inicial).
*   **Control de Versiones:** GitHub + Databricks Git Folders.

## 🚀 Cómo funciona
1.  `01_ingestion`: Carga los datos crudos desde archivos CSV/JSON a tablas Delta Bronze.
2.  `02_refining`: Aplica reglas de limpieza, deduplicación y separa la data en `sales_silver` y `sales_quarantine`.
3.  `03_aggregating`: Genera la tabla final `sales_gold_performance` lista para ser consumida por Power BI.

## 📊 Principales KPI calculados
*   **Ventas por Distrito:** Identificación de las zonas de mayor demanda en Lima.
*   **Ticket Promedio por Canal:** Comparativa de rendimiento entre tienda física y web.
*   **Tasa de Error de Datos:** Porcentaje de registros en cuarentena para mejora de procesos en origen.

---
**Desarrollado por:** [Dmorip]  
