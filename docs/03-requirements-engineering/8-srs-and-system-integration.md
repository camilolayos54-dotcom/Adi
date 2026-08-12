# Especificación de Requisitos del Sistema e Integración (SRS) (Entregable 8)

**Proyecto:** Adí (Gestión de Inventario y CRM de Calzado Deportivo)  
**ID del Documento:** D8-SRS-INTEGRATION  
**Fase:** 3 — Ingeniería de Requisitos (Capa 3 — Integración)  
**Estado:** Completo — Compuerta 3 Formalmente Aprobada  

---

## 1. Resumen Ejecutivo y Visión Consolidada del Sistema

La presente Especificación de Requisitos del Sistema (SRS) consolida la fase de ingeniería de requisitos para el **Proyecto Adí**. El sistema combina un motor relacional de inventario y catálogo (`MOD-CAT`), un pipeline de ingesta por lote con compresión WebP (`MOD-IMP`), un punto de venta transaccional con emisión de colillas (`MOD-VEN`) y un panel administrativo web responsivo con auditoría RBAC (`MOD-ADM`).

Todas las especificaciones detalladas por módulo se encuentran almacenadas en el directorio:  
[`docs/03-requirements-engineering/7.modules-specification/`](file:///c:/PROGRAMMING/PROJECTS/Adi/docs/03-requirements-engineering/7.modules-specification/)

- [`1-MOD-CAT.md`](file:///c:/PROGRAMMING/PROJECTS/Adi/docs/03-requirements-engineering/7.modules-specification/1-MOD-CAT.md): Gestión de Catálogo e Inventario Relacional (SKUs, Tallas, Colores, Tipos de Par).
- [`2-MOD-IMP.md`](file:///c:/PROGRAMMING/PROJECTS/Adi/docs/03-requirements-engineering/7.modules-specification/2-MOD-IMP.md): Ingesta y Carga Masiva por Lotes de Mercancía e Imágenes WebP.
- [`3-MOD-VEN.md`](file:///c:/PROGRAMMING/PROJECTS/Adi/docs/03-requirements-engineering/7.modules-specification/3-MOD-VEN.md): Punto de Venta y Emisión de Colillas Transaccionales.
- [`4-MOD-ADM.md`](file:///c:/PROGRAMMING/PROJECTS/Adi/docs/03-requirements-engineering/7.modules-specification/4-MOD-ADM.md): Panel Administrativo, Control de Acceso JWT / RBAC y Dashboard.

---

## 2. Matriz de Trazabilidad Cruzada de Requisitos

| ID Requisito | Resumen del Requisito | Módulo Principal | Módulo Secundario | Caso de Prueba Objetivo |
|---|---|---|---|---|
| `CR-CAT-01` | Registro de Marcas, Modelos y SKUs | `MOD-CAT` | `MOD-ADM` | `TC-CAT-01` |
| `CR-CAT-04` | Clasificación por Tipo de Par (Par, Izq, Der, Muestra) | `MOD-CAT` | `MOD-VEN` | `TC-CAT-04` |
| `CR-CAT-06` | Alerta de Stock Mínimo por Variante | `MOD-CAT` | `MOD-ADM` | `TC-CAT-06` |
| `CR-IMP-01` | Carga Masiva de Manifiestos CSV/JSON | `MOD-IMP` | `MOD-CAT` | `TC-IMP-01` |
| `CR-IMP-02` | Pipeline de Compresión WebP de Fotos | `MOD-IMP` | CDN Cloud | `TC-IMP-02` |
| `CR-VEN-02` | Descuento Atómico de Stock ACID | `MOD-VEN` | `MOD-CAT` | `TC-VEN-02` |
| `CR-VEN-03` | Generación e Impresión de Colillas Transaccionales | `MOD-VEN` | Impresora POS | `TC-VEN-03` |
| `CR-ADM-01` | Autenticación JWT y Hash bcrypt | `MOD-ADM` | Todos | `TC-ADM-01` |
| `CR-ADM-02` | Control de Acceso Basado en Roles (RBAC) | `MOD-ADM` | Todos | `TC-ADM-02` |

---

## 3. Verificación y Resolución de Conflictos Inter-Módulo

- **Chequeo de Conflicto 1 (Concurrencia Venta vs Ingesta):** Se validó que las operaciones de inserción masiva en `MOD-IMP` utilicen transacciones aisladas que no bloqueen las lecturas de punto de venta en `MOD-VEN`.
- **Chequeo de Conflicto 2 (Descuento de Stock por Tipo de Par):** Se verificó que `MOD-VEN` descuente exactamente el tipo de par vendido (ej. pie izquierdo suelto vs par completo) evitando descuadres relacionales en `MOD-CAT`.
- **Chequeo de Conflicto 3 (Protección RBAC de Datos Financieros):** Se confirmó que la API REST de `MOD-ADM` filtre las respuestas de costos de importación provenientes de `MOD-IMP` si el token JWT pertenece a un perfil `ACT-VEN`.

---

## 4. Sign-Off Formal de Compuerta 3 (Gate 3 Sign-Off)

- **Capa 1 (Fundamentos Globales D1-D6):** APROBADO
- **Capa 2 (Especificaciones Modulares 1-MOD-CAT a 4-MOD-ADM):** CERRADO PERMANENTEMENTE
- **Capa 3 (SRS e Integración):** APROBADO

**DECISIÓN FINAL: FASE 3 COMPLETADA — AUTORIZADO EL AVANCE A FASE 4 (MODELADO DEL SISTEMA)**

---

* **Autorizador:** Arquitecto Principal de Sistemas & Sponsor Ejecutivo  
* **Fecha:** Agosto 2026  
