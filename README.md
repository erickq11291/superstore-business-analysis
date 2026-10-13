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
<img  alt="ask1" src="https://github.com/user-attachments/assets/ced82e0b-3867-4e06-9771-32a789a60eb7" />

### 2. ¿Qué clientes generan más ingresos para la empresa?

Determinar qué clientes tienen el mayor impacto en las ventas totales.
````sql
SELECT TOP 5 Customer_Name, SUM(Sales) AS Monto_Total_Gastado
FROM Orders
GROUP BY Customer_Name
ORDER BY Monto_Total_Gastado DESC;
````
<img width="248" height="114" alt="ask2" src="https://github.com/user-attachments/assets/580ca470-cfcd-4a4e-9202-3bea0bfa1e2d" />


### 3. ¿Qué porcentaje de las ventas totales aporta cada región?

Analizar la distribución geográfica de los ingresos e identificar las regiones más importantes para el negocio.
````sql
SELECT Category, SUM(Profit) AS Ganancia_Total
FROM Orders
GROUP BY Category
HAVING SUM(Profit)>0
ORDER BY Ganancia_Total;
````
<img width="212" height="80" alt="ask3" src="https://github.com/user-attachments/assets/093a6e54-86a0-4a9e-9abe-0c1561025b5f" />


### 4. ¿Cómo han evolucionado las ventas a lo largo de los años?

Evaluar el crecimiento o disminución de las ventas mediante una comparación año contra año.
````sql
SELECT Region, SUM(Sales) AS Ventas_Totales, CAST(100 * SUM(SALES) / SUM(SUM(Sales)) OVER () AS DECIMAL(5,2)) AS Porcentaje_Ventas
FROM Orders
GROUP BY Region
ORDER BY Porcentaje_Ventas;
````
<img width="281" height="94" alt="ask4" src="https://github.com/user-attachments/assets/fb1057e9-93f8-44d5-8247-312a810e3f76" />


### 5. ¿Qué categorías de productos generan más beneficio?

Identificar qué categorías aportan la mayor rentabilidad al negocio.
````sql
WITH ProductSales AS(
	SELECT YEAR(Order_Date) AS Año, Product_Name, SUM(Sales) AS Ventas_Totales
	FROM Orders
	GROUP BY YEAR(Order_Date), Product_Name
	),
RankedProducts AS(
	SELECT Año, Product_Name, Ventas_Totales, ROW_NUMBER() OVER(
		PARTITION BY Año
		ORDER BY Ventas_Totales DESC
		) AS RANKING
	FROM ProductSales
	)
SELECT Año, Product_Name, Ventas_Totales
FROM RankedProducts
WHERE Ranking = 1
ORDER BY Año;
````
<img width="439" height="94" alt="ask5" src="https://github.com/user-attachments/assets/271d9b22-76f2-43d0-ba8c-0203570529ec" />


### 6. ¿Qué productos tienen el mayor margen de beneficio?

Determinar qué productos son más eficientes generando ganancias en relación con sus ventas.
````sql
WITH Ventas_Anuales AS (
    SELECT YEAR(Order_Date) AS Año, SUM(Sales) AS Ventas_Totales
    FROM Orders
    GROUP BY YEAR(Order_Date)
)
SELECT Año, Ventas_Totales,
    LAG(Ventas_Totales) OVER (ORDER BY Año) AS Ventas_Año_Anterior,
    Ventas_Totales - LAG(Ventas_Totales) OVER (ORDER BY Año) AS Diferencia_Ventas,
    ROUND(
        ((Ventas_Totales - LAG(Ventas_Totales) OVER (ORDER BY Año))/LAG(Ventas_Totales) OVER (ORDER BY Año)) * 100,2) AS Porcentaje_Crecimiento_Ventas
FROM Ventas_Anuales
ORDER BY Año;
````
<img width="549" height="94" alt="ask6" src="https://github.com/user-attachments/assets/f2ab897b-f601-48a5-8029-e216354bf994" />


### 7. ¿Cómo afectan los descuentos a la rentabilidad?

Analizar el impacto de los diferentes niveles de descuento sobre los beneficios obtenidos.
````sql
SELECT TOP 10 Product_Name, SUM(Profit) AS Ganancia, SUM(Sales) AS Ventas_Totales, ROUND(SUM(Profit)*100/SUM(Sales), 2) AS Ganancia_Total
FROM Orders
GROUP BY Product_Name
HAVING SUM(Sales)>0
ORDER BY Ganancia_Total DESC;
````
<img width="565" height="212" alt="ask7" src="https://github.com/user-attachments/assets/4807d715-5a1e-40a2-a5c7-632d06d0b276" />


### 8. ¿Qué clientes realizan más pedidos?

Identificar los clientes más frecuentes y comprender mejor el comportamiento de compra.
````sql
SELECT TOP 10 Customer_Name, COUNT(DISTINCT Order_ID) AS Total_Pedidos
FROM Orders
GROUP BY Customer_Name
ORDER BY Total_Pedidos DESC;
````
<img width="221" height="208" alt="ask8" src="https://github.com/user-attachments/assets/feaf0fa5-a4fb-4c1a-9e05-da609667b0bb" />


### 9. ¿Cuál fue el producto más vendido de cada año?

Detectar qué productos lideraron las ventas en cada periodo anual.
````sql
SELECT CASE
			WHEN Discount = 0 THEN '0%'
			WHEN Discount <= 0.10 THEN '1% - 10%'
			WHEN Discount <= 0.20 THEN '11% - 20%'
			WHEN Discount <= 0.30 THEN '21% - 30%'
			ELSE 'Más del 30%'
		END AS Porcentaje_Descuento, COUNT(*) AS Total_Pedidos, ROUND(AVG(Profit),2) AS Promedio_Ganancia, ROUND(SUM(Profit),2) AS Total_Ganancia
FROM Orders
GROUP BY CASE
			WHEN Discount = 0 THEN '0%'
			WHEN Discount <= 0.10 THEN '1% - 10%'
			WHEN Discount <= 0.20 THEN '11% - 20%'
			WHEN Discount <= 0.30 THEN '21% - 30%'
			ELSE 'Más del 30%'
		END
ORDER BY Porcentaje_Descuento;
````
<img width="445" height="115" alt="ask9" src="https://github.com/user-attachments/assets/e24d2566-b1d4-4956-8ae2-7d021051dd33" />


### 10. ¿Qué productos mantuvieron un crecimiento sostenido durante tres años consecutivos?

Identificar productos con tendencias de crecimiento consistentes a largo plazo.
````sql
WITH ProductSalesByYear AS (
	SELECT YEAR(Order_Date) AS Año, Product_Name, SUM(Sales) AS Total_Ventas
	FROM Orders
	GROUP BY YEAR(Order_Date), Product_Name
),
SalesHistory AS (
	SELECT Product_Name, Año, Total_Ventas,
			LAG(Total_Ventas, 1) OVER( PARTITION BY Product_Name
			ORDER BY Año) AS Previous_Year_Sales,
			LAG(Total_Ventas, 2) OVER (PARTITION BY Product_Name
			ORDER BY Año) AS Two_Years_Ago_Sales
	FROM ProductSalesByYear
)
SELECT Product_Name, Año, Two_Years_Ago_Sales, Previous_Year_Sales, Total_Ventas
FROM SalesHistory
WHERE Total_Ventas > Previous_Year_Sales AND Previous_Year_Sales > Two_Years_Ago_Sales
ORDER BY Product_Name, Año;
````
<img width="682" height="494" alt="ask10" src="https://github.com/user-attachments/assets/9602f1c0-9427-4207-b6f2-090029b832a4" />


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
