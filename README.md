 Poyecto-RetailPro: Extrayendo métricas clave con SQL
Descripción general
Este script SQL genera consultas de negocio para el equipo comercial de RetailPro a partir de la base de datos Ventas_Tech_DB (trabajando únicamente con la tabla ventas).
El objetivo es extraer métricas rápidas como totales, rankings y clientes recurrentes para usar posteriormente en Power BI.

Archivo / Autor
Archivo: m4_consultas_negocio.sql
Autor: Valdez Rocio
Fecha: 23/06/2026
Fuentes de datos
Se utiliza únicamente la tabla ventas, con las columnas:

id_cliente
id_producto
cantidad
precio_unitario
fecha_ventas
Nota: los nombres de clientes/productos se trabajan más adelante con JOIN (Módulo 5). En este script se trabaja con IDs.

Consultas incluidas
Consulta 1 — Resumen ejecutivo mensual
Genera, por mes:

Total facturado = SUM(cantidad * precio_unitario)
Cantidad de pedidos = COUNT(*)
Ticket promedio = SUM(cantidad * precio_unitario) / COUNT(*)
Agrupa por mes usando EXTRACT(MONTH FROM fecha_ventas).

Consulta 2 — Ranking de productos (Top 5)
Devuelve el Top 5 de id_producto por total generado, mostrando:

unidades_vendidas = SUM(cantidad)
total_generado = SUM(cantidad * precio_unitario)*
Ordena por total_generado DESC y limita a 5 filas.

Consulta 3 — Clientes recurrentes
Muestra los id_cliente que hicieron más de un pedido:

cantidad_pedidos = COUNT(*)
total_gastado = SUM(cantidad * precio_unitario)
Filtra con HAVING COUNT(*) > 1 y ordena por total_gastado DESC.*

Consulta 4 — Meses por encima/por debajo del promedio
Calcula:

total facturado por mes
un promedio mensual general y etiqueta cada mes como:
“Por encima” si el total supera el promedio
“Por debajo” si no lo supera
Se apoya en una CTE (WITH ventas_por_mes AS (...)).

Bloque de cierre — Hallazgos
(Se encuentra al final del archivo como comentarios)

Como todas las compras corresponden al mes 3, la comparativa “Por encima/Por debajo” no discrimina entre meses: el mes 3 queda etiquetado como “por debajo”. Total mes 3: 6444.00.
El producto 1 concentra el mayor volumen del Top 5: 3 unidades y 3600.00 de total generado.
El cliente 1 es recurrente: 2 pedidos y 2640.00 de total gastado.
Cómo ejecutar
Abrí tu entorno SQL (PostgreSQL).
Corré el script completo en el orden que figura:
Consulta 1
Consulta 2
Consulta 3
Consulta 4
Verificá que el bloque final de Hallazgos coincida con los resultados obtenidos.
