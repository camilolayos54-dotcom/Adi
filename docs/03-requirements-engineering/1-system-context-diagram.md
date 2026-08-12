# Diagrama de Contexto del Sistema (Entregable 1)

**Proyecto:** Adí (Gestión de Inventario y CRM de Calzado Deportivo)  
**ID del Documento:** D1-SYSTEM-CONTEXT  
**Fase:** 3 — Ingeniería de Requisitos (Capa 1)  

---

## 1. Definición de Límites del Sistema

El **Sistema Adí** es una plataforma centralizada de gestión de inventario relacional y registro transaccional de ventas acoplada a la comercialización de calzado deportivo. El sistema se ubica entre las fuentes primarias de mercancía importada (lotes de proveedores internacionales) y los puntos de contacto con clientes (registro transaccional y emisión de colillas/comprobantes físicos y digitales).

## 2. Diagrama de Contexto del Sistema (Mermaid)

```mermaid
graph TD
    subgraph Sistemas Externos
        PROV["Proveedores Internacionales (Manifiestos y SKUs en CSV/Excel)"]
        CLOUD_IMG["Servicio de Almacenamiento Cloud / CDN (Imágenes WebP)"]
        PRINTER["Impresora Térmica Local / Generador PDF de Colillas"]
    end

    subgraph Límite del Sistema Adí
        MOD_CAT["MOD-CAT: Motor de Catálogo e Inventario Relacional"]
        MOD_IMP["MOD-IMP: Módulo de Ingesta y Carga Masiva por Lote"]
        MOD_VEN["MOD-VEN: Motor Transaccional de Ventas y Colillas"]
        MOD_ADM["MOD-ADM: Panel de Administración Web y Auditoría RBAC"]
        BD_POSTGRES[("Base de Datos Relacional PostgreSQL / SQLite")]
    end

    subgraph Actores Humanos
        ACT_ADM["Administrador del Sistema"]
        ACT_OPE["Operador de Inventario"]
        ACT_VEN["Cajero / Vendedor"]
    end

    %% Flujos de Datos
    PROV -->|Carga de Manifiesto / CSV| MOD_IMP
    MOD_IMP -->|Optimización y Carga de Fotos| CLOUD_IMG
    MOD_IMP -->|Creación Masiva de SKUs y Stock| MOD_CAT
    MOD_CAT <-->|Lectura/Escritura de Entidades| BD_POSTGRES
    MOD_VEN <-->|Descuento Transaccional de Stock| BD_POSTGRES
    MOD_VEN -->|Comando de Impresión / Descarga PDF| PRINTER

    ACT_ADM <-->|Gestión de Usuarios, Roles y Métricas| MOD_ADM
    ACT_OPE <-->|Ingesta de Lotes y Ajustes de Stock| MOD_IMP
    ACT_OPE <-->|Consulta de Existencias y Variantes| MOD_CAT
    ACT_VEN <-->|Registro de Venta y Emisión de Colillas| MOD_VEN

    MOD_ADM <-->|API REST / Middleware JWT| BD_POSTGRES
```

## 3. Catálogo de Interfaces Externas

1. **Interfaz de Ingesta por Lote (Proveedores):** Recibe manifestos de carga en CSV o JSON con información de proveedor internacional, marca, modelo, SKUs, colorways y desgloses de tallas (US/EU/COL).
2. **Interfaz de Almacenamiento CDN/Cloud:** Servicio de carga asíncrona que comprime imágenes de calzado al formato WebP y retorna URLs públicas/firmadas para su renderizado en el gestor.
3. **Interfaz de Impresión / Salida de Colillas:** Servicio local o del navegador que genera archivos PDF y comanda impresoras térmicas (POS) para la emisión de comprobantes físicos de compra con códigos de barras.
4. **Persistencia Relacional ACID:** Conexión con PostgreSQL o SQLite para garantizar 0 descuadres en el inventario relacional al momento de ejecutar transacciones de venta simultáneas.
