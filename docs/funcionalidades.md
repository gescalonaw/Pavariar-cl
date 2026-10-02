# Funcionalidades — PaVariar

Detalle técnico del alcance del proyecto, organizado por área.

---

## 1. Tema de WordPress a medida

Child theme construido sobre Storefront, pero con layout, estilos y lógica propios en todas las plantillas principales — no es una personalización superficial de la plantilla base.

- **Nav y footer unificados**: componentes PHP reutilizables (`pavariar_render_nav()` / `pavariar_render_footer()`) consumidos por todas las plantillas, evitando markup duplicado y desincronización entre páginas.
- **Home**: hero, categorías leídas dinámicamente desde WooCommerce (no hardcodeadas — se actualizan solas si el catálogo cambia), sección de preguntas frecuentes en acordeón accesible (`<details>`/`<summary>`, sin dependencia de JS para la funcionalidad base).
- **Buscador de productos**: componente reutilizable embebido en el header (desktop) y en la banda de categoría (móvil), con comportamiento responsive para no duplicar la UI en pantallas chicas.
- **Tienda y categorías**: grid de productos con filtro de categorías dinámico (jerárquico, respeta subcategorías) y degradación sin JavaScript vía formulario GET nativo.
- **Ficha de producto**: galería de imágenes convertida a carrusel horizontal con scroll-snap nativo de CSS (sin librerías de slider), navegación por flechas, puntos indicadores y soporte de teclado.
- **Carrito y checkout**: layout en dos columnas con resumen de pedido fijo (sticky) durante el scroll; carrito vacío con estado custom; checkout con cupón inline vía AJAX.
- **Cuenta de usuario**: login, registro y edición de datos unificados en una sola pantalla; historial de pedidos con estados diferenciados por color.
- **Optimización de rendimiento**: eliminación de assets de WooCommerce/Storefront no utilizados por el tema (CSS, JS y fuentes que no correspondían a la página activa), versionado de archivos estáticos por `filemtime()` para invalidación de caché automática, y reorganización de la carga crítica — resultado medido con Google PageSpeed Insights.

## 2. Pagos y checkout

- Integración de **Webpay Plus (Transbank)** como método de pago.
- Detalle del comprobante de pago (estado, método, monto, fecha) inyectado automáticamente en el correo de confirmación al cliente, filtrando campos sensibles o técnicos (tokens, IDs internos) antes de enviarlos.
- **Guardia de stock para pedidos pendientes**: un pedido que quedó sin pagar y cuyo producto se agotó después no puede completarse — se bloquea el flujo de pago y se notifica al cliente, evitando vender stock que ya no existe.
- **Upsell de "caja de regalo"**: complemento opcional de bajo costo ofrecido en el checkout, con lógica de precio independiente del producto base.

## 3. Despacho

- Integración con **Blue Express** para cotización de envío según comuna de destino y generación de etiquetas de despacho desde el panel de pedidos de WooCommerce.
- Flujo de retiro en tienda como alternativa al despacho, con estado de pedido dedicado y notificación automática al cliente cuando el pedido está listo.

## 4. Infraestructura y entrega

- Configuración de correo transaccional (WP Mail SMTP) con registros **SPF, DKIM y DMARC** para asegurar la entregabilidad de los correos de pedido.
- Diagnóstico y resolución de bloqueos de envío de correo a nivel de relay SMTP.
- Checklist de puesta en producción: indexación SEO, políticas legales (privacidad, cookies, envíos y devoluciones conforme a la Ley 21.719 de protección de datos de Chile), verificación de propiedad en Google Search Console.

---

*Este documento describe el alcance funcional del proyecto. El código fuente no es público por tratarse de un desarrollo comercial para un cliente.*