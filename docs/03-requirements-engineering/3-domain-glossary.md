# Glosario de Dominio (Entregable 3)

**Proyecto:** Adí (Gestión de Inventario y CRM de Calzado Deportivo)  
**ID del Documento:** D3-DOMAIN-GLOSSARY  
**Fase:** 3 — Ingeniería de Requisitos (Capa 1)  

---

## 1. Términos del Dominio de Calzado e Inventario

1. **SKU de Proveedor Internacional (SKU-PROV):** Identificador alfanumérico único asignado por el fabricante o distribuidor internacional (ej. `NK-AF1-001-42-BLK`).
2. **Colorway:** Combinación específica de colores y acabados de un modelo de calzado (ej. "Triple White", "Bred", "UNC Blue").
3. **Equivalencia de Talla (US / EU / COL):** Mapeo tridimensional entre los sistemas de talle americano (US), europeo (EU) y colombiano (COL), garantizando que las consultas de inventario reconozcan equivalencias (ej. US 9 = EU 42.5 = COL 40).
4. **Tipo de Par (Especificación de Variante):** Clasificación del inventario físico en cuatro categorías discretas:
   - **Par Completo:** Ambos pies (izquierdo y derecho) listos para la venta al público.
   - **Pie Izquierdo:** Existencia de una sola zapatilla del lado izquierdo (utilizada en exhibición o reemplazo).
   - **Pie Derecho:** Existencia de una sola zapatilla del lado derecho.
   - **Muestra / Exhibición:** Calzado utilizado para vitrina o probador con restricción de venta normal.
5. **Lote de Ingesta (Batch Import):** Contenedor lógico de mercancía recibida en una misma importación o envío, asociado a una fecha, costo unitario en divisa de origen y manifiesto de transporte.
6. **Colilla Transaccional (Ticket de Comprobante):** Documento físico o digital impreso/generado al confirmar una venta, que contiene el detalle de SKUs, tallas, tipo de par, valor total, código de barras y términos de garantía.
7. **Stock Mínimo / Umbral de Reabastecimiento:** Cantidad mínima parametrizada de pares por SKU/talla por debajo de la cual el sistema dispara una alerta visual de inventario bajo.
8. **Descuadre de Stock:** Discrepancia entre la cantidad física de calzado en bodega y la cantidad registrada en la base de datos relacional.
