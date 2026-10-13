# 📊 Superstore Business Analysis

## 📌 Descripción del Proyecto

Este proyecto analiza datos de ventas de una tienda minorista utilizando SQL Server.

A través de una serie de preguntas de negocio, se exploran patrones relacionados con ventas, rentabilidad, clientes y productos utilizando consultas SQL y funciones analíticas avanzadas.

---

## 🎯 Objetivos

Aplicar técnicas de análisis de datos con SQL para obtener información útil sobre el desempeño comercial de una empresa y practicar consultas utilizadas habitualmente en entornos de Business Intelligence y Data Analytics.

---

## 🧠 Habilidades SQL Demostradas

- GROUP BY
- ORDER BY
- Funciones de agregación (`SUM`, `AVG`, `COUNT`)
- CASE WHEN
- Common Table Expressions (CTEs)
- Window Functions
- ROW_NUMBER()
- RANK()
- LAG()

---

## 📈 Preguntas de Negocio Analizadas

### 1. ¿Cuáles son los 10 productos más vendidos?
Identificar los productos que generan el mayor volumen de ventas y contribuyen significativamente a los ingresos de la empresa.

````sql
SELECT TOP 10 Product_Name, SUM(Sales) AS Ventas_Totales
FROM Orders
GROUP BY Product_Name
ORDER BY Ventas_Totales DESC;

````
<img width="403" height="211" alt="ask1" src="https://github.com/user-attachments/assets/ced82e0b-3867-4e06-9771-32a789a60eb7" />

### 2. ¿Qué clientes generan más ingresos para la empresa?

Determinar qué clientes tienen el mayor impacto en las ventas totales.

### 3. ¿Qué porcentaje de las ventas totales aporta cada región?

Analizar la distribución geográfica de los ingresos e identificar las regiones más importantes para el negocio.

### 4. ¿Cómo han evolucionado las ventas a lo largo de los años?

Evaluar el crecimiento o disminución de las ventas mediante una comparación año contra año.

### 5. ¿Qué categorías de productos generan más beneficio?

Identificar qué categorías aportan la mayor rentabilidad al negocio.

### 6. ¿Qué productos tienen el mayor margen de beneficio?

Determinar qué productos son más eficientes generando ganancias en relación con sus ventas.

### 7. ¿Cómo afectan los descuentos a la rentabilidad?

Analizar el impacto de los diferentes niveles de descuento sobre los beneficios obtenidos.

### 8. ¿Qué clientes realizan más pedidos?

Identificar los clientes más frecuentes y comprender mejor el comportamiento de compra.

### 9. ¿Cuál fue el producto más vendido de cada año?

Detectar qué productos lideraron las ventas en cada periodo anual.

### 10. ¿Qué productos mantuvieron un crecimiento sostenido durante tres años consecutivos?

Identificar productos con tendencias de crecimiento consistentes a largo plazo.

---

## 📂 Estructura del Proyecto

```text
superstore-business-analysis
│
├── README.md
├── queries.sql
├── dataset
└── screenshots
```

---

## 👨‍💻 Autor

**Erick Quispe**

- LinkedIn: www.linkedin.com/in/erick-joseph-quispe-baca/
- GitHub: https://github.com/erickq11291
