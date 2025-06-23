# Modelado de requerimientos del proyecto

## Objetivos

- Ecommerce para programar pedidos de productos
- Sistema de distribuidores y referidos

## Actores

- Visitante - Navega el catálogo sin estar registrado
- Afiliado - Vende a terceros, registra más afiliados, recibe comisiones
- Administrador - Gestiona pedidos, stock y usuarios en su región.
- Administrador General - Acceso total; configura prpductos, reglas, premios y comisiones.
- Sistema de pagos - Proveedor externo para los pagos y validaciones de pago
- Repartidor - Agente que hace la distribución de los pedidos (Shalom x ejem)

Para los requerimientos funcionales/no funcionales usaremos el estandar de MoSCoW:

- Must (M) : Obligatorio
- Should (S): Importante
- Could (C): deseable
- Won’t (W): fuera de alcance (para el mvp al menos)

## Requerimientos funcionales

| ID | Pri | Descripción |
| --- | --- | --- |
| **RF‑01** | M | Autenticación de usuarios por DNI contraseña. |
| **RF‑02** | M | Registro manual de referidos por un afiliad; el patrocinador introduce DNI, nombre, celular y direccion (Region/ciudad/direccion/referencia) |
| **RF‑03** | M | Catalogo de productos con precios, stock |
| **RF‑04** | M | Carrito y proceso de checkout que acepta **códigos de pago BCP** (validacón automática). |
| **RF‑05** | M | Regla: **Compra mínima mensual** configurable (puede ser 1 frasco al mes). Si el afiliado no la cumple su cuenta se desactiva |
| **RF‑06** | M | Cálculo automático de comisión por venta directa (Precio al que el afiliado vende el producto - precio al que lo pidió en la plataforma) y ventas de referidos (10% de los productos que pidió su referido, por mes) |
| **RF‑07** | M | Panel Mis Comisiones” para Afiliado: total mes actual, histórico, desglose por referido y por venta directa. |
| **RF‑08** | M | Panel “Mi red de afiliados”: Cards con información de los afiliados, las comisiones que ganaron, la cantidad de productos que vendieron (que pidieron de la app) y cuanto de comision le está generando. |
| **RF‑09** | M | Panel “Mis Pedidos”: historial filtrable (último mes, rango de fechas). |
| **RF‑10** | S | Módulo **Proyección de Premios**: cálculo de puntos y catálogo de premios alcanzables. |
| **RF‑11** | S | Centro de **Capacitación,** Tutoriales por video o pdfs que explican el paso a paso para el uso de la app y las ventas. |
| **RF‑12** | M | **Notificaciones in‑app** y opcional correo email: pago exitoso, comisión generada, cuenta pendiente. |
| **RF‑13** | M | Vista de **Administrador**: pedidos, asignar guía Shalom, marcar pedidos como programados, ver comisiones pendientes y aprobadas, y aprobarlas. |
| **RF‑14** | M | Vista de **Administrador General**: tablero unificado, gestión de usuarios, productos y reglas de negocio. |
| **RF‑15** | W | Integración API con Shalom para generar etiquetas y seguimiento en tiempo real. |

## Requerimientos no funcionales

| ID | Pri | Descripción |
| --- | --- | --- |
| RNF-01 | M | El sistema deberá permanecer activo el 99.9% de las 24 horas |
| RNF-02 | C | Los pagos del sistema deberán admitir pagos por efectivo. |
| RNF-03 | M | Se requiere de un acceso fácil para dispositivos móviles  |
| RNF-04 | M | La interfaz debe ser intuitiva para nuevos usuarios que quieran integrarse al sistema de referidos |
| RNF-05 | W | La capacidad deberá ser suficiente para soportar pedidos desde toda latinoamerica |
| RNF-06 | S | Se deberá tener compatibilidad con el servidor del cliente |
| RNF-07 | C | Es deseable que exista posibilidad de traducción a idiomas como el portugués o ingles |
|  |  |  |

/t