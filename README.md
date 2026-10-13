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
<img width="404" height="208" alt="ask1" src="https://github.com/user-attachments/assets/05b9aa05-a1bb-44c2-ae01-453692e8b206" />


### 2. ¿Qué clientes generan más ingresos para la empresa?

Determinar qué clientes tienen el mayor impacto en las ventas totales.
````sql
SELECT TOP 5 Customer_Name, SUM(Sales) AS Monto_Gastado
FROM Orders
GROUP BY Customer_Name
ORDER BY Monto_Gastado DESC;
````
<img width="222" height="115" alt="ask2" src="https://github.com/user-attachments/assets/c99bb3e0-c6cd-48cf-80ca-f29866bd8020" />


### 3. ¿Qué porcentaje de las ventas totales aporta cada región?

Analizar la distribución geográfica de los ingresos e identificar las regiones más importantes para el negocio.
````sql
SELECT Region, SUM(Sales) AS Total_Ventas, ROUND(SUM(Sales)*100/SUM(SUM(Sales)) OVER(),2) AS Porcentaje_Ventas
FROM Orders
GROUP BY Region
ORDER BY Porcentaje_Ventas;
````
<img width="270" height="95" alt="ask3" src="https://github.com/user-attachments/assets/c4194245-86bd-49d4-8ae2-723df2a483fc" />



### 4. ¿Cómo han evolucionado las ventas a lo largo de los años?

Evaluar el crecimiento o disminución de las ventas mediante una comparación año contra año.
````sql
WITH Ventas_Anuales AS(
	SELECT YEAR(Order_Date) AS Año, SUM(Sales) AS Ventas_Totales
	FROM Orders
	GROUP BY YEAR(Order_Date)
)
SELECT Año, Ventas_Totales,
		LAG(Ventas_Totales) OVER(ORDER BY Año) AS Año_Anterior,
		Ventas_Totales - LAG(Ventas_Totales) OVER(ORDER BY Año) AS Diferencia_Ventas,
		ROUND(
			(Ventas_Totales-LAG(Ventas_Totales) OVER(ORDER BY Año))/LAG(Ventas_Totales) OVER(ORDER BY Año)*100,2
		) AS Porcentaje_Crecimiento
FROM Ventas_Anuales;
````
<img width="471" height="96" alt="ask4" src="https://github.com/user-attachments/assets/4d2a409d-755e-4feb-9154-c1d7c8d1f2a5" />


### 5. ¿Qué categorías de productos generan más beneficio?

Identificar qué categorías aportan la mayor rentabilidad al negocio.
````sql
SELECT Category, SUM(Profit) AS Beneficio_Total
FROM Orders
GROUP BY Category
ORDER BY Beneficio_Total;
````
<img width="211" height="76" alt="ask5" src="https://github.com/user-attachments/assets/6c6603a9-9358-4b1a-825d-b4be1c98d9a6" />


### 6. ¿Qué productos tienen el mayor margen de beneficio?

Determinar qué productos son más eficientes generando ganancias en relación con sus ventas.
````sql
SELECT  TOP 10 Product_Name, SUM(Sales) AS Ventas_Totales, SUM(Profit) AS Ganancia, ROUND(SUM(Profit)*100/SUM(Sales),2) AS Ganancia_Total
FROM Orders
GROUP BY Product_Name
ORDER BY Ganancia_Total DESC;
````
<img width="569" height="209" alt="ask6" src="https://github.com/user-attachments/assets/5d896514-ea0e-4ff3-85cc-9f8969390ecd" />


### 7. ¿Cómo afectan los descuentos a la rentabilidad?

Analizar el impacto de los diferentes niveles de descuento sobre los beneficios obtenidos.
````sql
SELECT CASE
			WHEN Discount = 0 THEN '0%'
			WHEN Discount <= 0.1 THEN '0% - 10%'
			WHEN Discount <= 0.2 THEN '11% - 20%'
			WHEN Discount <= 0.3 THEN '21% - 30%'
			ELSE 'Más del 30%'
		END AS Porcentaje_Descuento, COUNT(*) AS Cantidad_Pedidos, ROUND(AVG(Profit),2) AS Promedio_Ganancia, ROUND(SUM(Profit),2) AS Total_Ganancia
FROM Orders
GROUP BY CASE
			WHEN Discount = 0 THEN '0%'
			WHEN Discount <= 0.1 THEN '0% - 10%'
			WHEN Discount <= 0.2 THEN '11% - 20%'
			WHEN Discount <= 0.3 THEN '21% - 30%'
			ELSE 'Más del 30%'
		END
ORDER BY Porcentaje_Descuento;
````
<img width="433" height="115" alt="ask7" src="https://github.com/user-attachments/assets/fbb45ab2-bb8f-4ce4-9d58-1bf3a16c9e3e" />


### 8. ¿Qué clientes realizan más pedidos?

Identificar los clientes más frecuentes y comprender mejor el comportamiento de compra.
````sql
SELECT TOP 10 Customer_Name, COUNT(DISTINCT Order_ID) AS Cantidad_Pedidos
FROM Orders
GROUP BY Customer_Name
ORDER BY Cantidad_Pedidos DESC;
````
<img width="242" height="209" alt="ask8" src="https://github.com/user-attachments/assets/bf8a03f9-53a1-4995-b60f-1565ac646097" />


### 9. ¿Cuál fue el producto más vendido de cada año?

Detectar qué productos lideraron las ventas en cada periodo anual.
````sql
WITH Ventas_Anuales AS(
	SELECT YEAR(Order_Date) AS Año, Product_Name, SUM(Sales) AS Ventas_Totales
	FROM Orders
	GROUP BY YEAR(Order_Date), Product_Name
), Ranked_Productos AS (
	SELECT Año, Product_Name, Ventas_Totales, ROW_NUMBER() OVER(
		PARTITION BY Año
		ORDER BY Ventas_Totales DESC) AS Ranking 
 	FROM Ventas_Anuales
)
SELECT Año, Product_Name, Ventas_Totales
FROM Ranked_Productos
WHERE Ranking =1
ORDER BY Año;
````
<img width="445" height="95" alt="ask9" src="https://github.com/user-attachments/assets/ac51b18e-22d9-4867-84a8-2066ce878bc6" />


### 10. ¿Qué productos mantuvieron un crecimiento sostenido durante tres años consecutivos?

Identificar productos con tendencias de crecimiento consistentes a largo plazo.
````sql
WITH Ventas_Anuales AS (
	SELECT Product_Name, SUM(Sales) AS Total_Ventas, YEAR(Order_Date) AS Año
	FROM Orders
	GROUP BY Product_Name,YEAR(Order_Date)
),
Historial_Ventas AS (
					SELECT Product_Name, Año, Total_Ventas,
							LAG(Total_Ventas, 1) OVER(PARTITION BY Product_Name ORDER BY Año) AS Hace_Un_Año,
							LAG(Total_Ventas, 2) OVER(PARTITION BY Product_Name ORDER BY Año) AS Hace_Dos_Años
					FROM Ventas_Anuales
)
SELECT Product_Name, Año, Total_Ventas, Hace_Un_Año, Hace_Dos_Años
FROM Historial_Ventas
WHERE Total_Ventas>Hace_Un_Año AND Hace_Un_Año>Hace_Dos_Años;
````
<img width="621" height="377" alt="ask10" src="https://github.com/user-attachments/assets/4059d6e3-b5ce-4dba-b301-1c126f9b2eed" />


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

- LinkedIn: [www.linkedin.com/in/erick-joseph-quispe-baca/](https://www.linkedin.com/in/erickjquispe/?isSelfProfile=true)
- GitHub: https://github.com/erickq11291
