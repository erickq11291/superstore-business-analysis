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

### 1. ¿Qué regiones generan más ventas?

````sql
SELECT 
  COUNT(DISTINCT id) AS unique_users
FROM health.user_logs
````

Analiza la distribución de ventas por región e identifica cuáles aportan una mayor participación en los ingresos totales.

### 2. ¿Qué categorías generan mayor beneficio?

Compara la rentabilidad entre categorías de productos para detectar las más valiosas para la empresa.

### 3. ¿Qué clientes generan más ingresos?

Identifica los clientes con mayores compras acumuladas.

### 4. ¿Qué productos tienen el mayor margen de beneficio?

Calcula el margen porcentual para encontrar los productos más rentables.

### 5. ¿Cuál fue el producto más vendido de cada año?

Utiliza funciones de ventana para identificar el producto líder por año.

### 6. ¿Cómo evolucionaron las ventas año tras año?

Analiza el crecimiento anual mediante comparaciones Year-over-Year.

### 7. ¿Qué clientes realizaron más pedidos?

Identifica los clientes con mayor frecuencia de compra.

### 8. ¿Cómo afectan los descuentos a la rentabilidad?

Evalúa la relación entre los niveles de descuento y las ganancias obtenidas.

### 9. ¿Qué ciudades generan más ventas?

Determina las ciudades con mejor desempeño comercial.

### 10. ¿Qué productos muestran crecimiento durante tres años consecutivos?

Detecta productos con tendencias sostenidas de crecimiento utilizando funciones analíticas avanzadas.

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
