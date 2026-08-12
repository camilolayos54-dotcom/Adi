# Requisitos No Funcionales Globales (Entregable 6)

**Proyecto:** Adí (Gestión de Inventario y CRM de Calzado Deportivo)  
**ID del Documento:** D6-GLOBAL-NFRS  
**Fase:** 3 — Ingeniería de Requisitos (Capa 1)  

---

## 1. Matriz Global de Restricciones No Funcionales

| ID NFR | Categoría | Meta Mensurable | Estrategia de Arquitectura |
| --- | --- | --- | --- |
| **`NFR-01`** | Rendimiento y Latencia | Consulta y filtrado de inventario en el gestor web < 100 ms. | Indexación compuesta relacional (SKU, marca, modelo, talla, color) en base de datos. |
| **`NFR-02`** | Consistencia y Concurrencia | Erradicación total (0%) de colisiones de stock o sobreventas. | Transacciones atómicas ACID con bloqueos de base de datos en operaciones de venta (`MOD-VEN`). |
| **`NFR-03`** | Ingesta por Lote | Procesamiento e indexación de un lote de 50+ pares con fotos en < 3 segundos. | Pipeline asíncrono de compresión de imágenes (WebP) e inserción en volumen (`bulk insert`). |
| **`NFR-04`** | Seguridad y Privacidad | Cifrado de datos de costos de importación y autenticación segura. | JWT con expiración de sesión, hashing de contraseñas con bcrypt/Argon2 y HTTPS TLS 1.3. |
| **`NFR-05`** | Disponibilidad y Portabilidad | Panel gestor 100% responsivo funcional en escritorio y dispositivos móviles. | Interfaz web responsiva (CSS flex/grid), empaquetamiento Docker y despliegue cloud. |

---

## 2. Criterios de Aceptación Globales

1. **Prueba de Carga de Consultas:** La búsqueda por código SKU o modelo debe retornar resultados paginados en menos de 100ms con una base de datos de 10,000+ registros.
2. **Prueba de Estrés Transaccional:** Dos peticiones concurrentes de compra sobre la última unidad de un SKU deben resultar en 1 transacción exitosa y 1 rechazo limpio por stock insuficiente.
3. **Prueba de Ingesta de Fotografías:** La subida de un paquete de 50 fotos en alta resolución debe ser procesada y convertida a WebP en menos de 3 segundos sin bloquear la UI principal.
