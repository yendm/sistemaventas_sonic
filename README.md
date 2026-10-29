# SONIC – Catálogo y ventas para ferretería (versión 1.0)

Aplicación web de **un solo archivo** (`index.html`): HTML + CSS + JavaScript puro, sin servidor ni librerías.
Pensada para tablet (vendedor), celular y computadora (clientes de la tienda online). Idioma: español. Moneda: soles (S/).

## Qué incluye
- Búsqueda rápida sin escribir el nombre completo (categoría → subcategoría → filtros), tolera errores y tildes.
- **Modo cliente**: carrito y pedido por WhatsApp, retiro en tienda o despacho.
- **Modo vendedor** (con registro y PIN): precios público/mayorista, **negociación de precio por unidad** con límites de descuento, **nota de venta**, proforma, conversión a **boleta o factura**, impresión en **térmico 80 mm, 58 mm o A4**.
- **Panel de control**: vendedores, reglas de descuento, personalizar recibo (logotipo, cuentas bancarias, notas), importar productos por CSV, historial de ventas con búsqueda por fecha.
- Cada documento generado cuenta como **venta**: descuenta stock y se guarda en el historial.

## Usarlo en la nube (GitHub Pages)
1. Crea un repositorio en GitHub (por ejemplo `sonic-ferreteria`) y sube `index.html` y este `README.md`
   (botón **Add file → Upload files**).
2. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` y guarda.
3. En 1–2 minutos tendrás tu sitio en `https://TU-USUARIO.github.io/sonic-ferreteria/`.
4. Para actualizar la app: edita o vuelve a subir `index.html`. Los datos de cada tablet **no se pierden**, porque no están en el archivo.

Tienda para clientes: agrega `?tienda` al final de la dirección (`.../sonic-ferreteria/?tienda`) y se oculta el modo vendedor.
Para que esa sea la versión por defecto, cambia `MODO_PUBLICO=false` por `true` en el código.

## Usarlo en local (sin internet)
- Descarga el repositorio (**Code → Download ZIP**), descomprime y abre `index.html` con Chrome o Edge.
- También funciona copiando `index.html` a la tablet.

## Dónde se guardan los datos (importante)
Los datos viven en el **navegador de cada dispositivo** (localStorage): vendedores, PIN, ventas, stock, carrito y configuración.
- Son **independientes en cada tablet o navegador**; no se comparten entre equipos.
- Si se borran los datos del sitio, se pierden. Usa **Panel → Ventas y documentos → 💾 Respaldo** con frecuencia y **📂 Restaurar** para recuperarlos.
- El stock descontado por ventas se reinicia al importar un CSV nuevo.

## Seguridad: qué hace y qué no
- El PIN impide el uso casual, pero **no es seguridad real**: todo el código está en el archivo y cualquiera que abra la página puede leerlo.
- Si el repositorio o el sitio son públicos, cualquiera puede ver el código, incluidos los **precios mayoristas** del catálogo y los datos por defecto del negocio.
- Para control real de vendedores, descuentos y datos compartidos entre equipos hace falta un servidor (por ejemplo Firebase, Supabase o similar).

## Boleta y factura: aviso importante
En este programa son **documentos de control interno**. Para tener validez tributaria en Perú, los comprobantes deben emitirse
como comprobantes electrónicos desde un sistema autorizado por SUNAT (SEE-SOL, OSE o PSE).
La app guarda los datos necesarios (DNI/RUC, razón social, dirección, IGV 18%) y permite exportarlos en CSV
(**Panel → Ventas y documentos → ⬇ Excel**) para emitirlos en ese sistema.
La nota de venta tampoco es comprobante de pago electrónico. Confirma con tu contador los requisitos aplicables a tu negocio.

## Editar la app
El código está dividido en secciones comentadas: datos del negocio, productos (`BASE`), precios, búsqueda,
carrito, pantallas, acciones, importación y documentos. Las series de los documentos (`SER`) y los textos al pie
se pueden cambiar en el código o desde el panel.

## Inventario en CSV (ver `inventario-ejemplo.csv`)
Columnas: `nombre, categoria, subcategoria, marca, presentacion, sku, precioCompra, precio, precioMayorista, cantidadMayorista, stock, alias, masVendido, imagen`
- Obligatorias: `nombre`, `categoria` y `precio` (precio público). Si falta `precioMayorista` se usa el 90% del público; si falta `cantidadMayorista`, 10; si falta `stock`, 20.
- `precioCompra` es lo que te cuesta el producto: sirve para calcular la ganancia. Sin él, el producto no entra en la ganancia.
- Precios con **punto decimal** y sin "S/" (29.50). Stock en números enteros. `alias` separados con `|`. `masVendido`: si / no.
- Si el nombre lleva comillas de pulgadas, escríbelo entre comillas y duplica la comilla interna (mira el ejemplo del fierro).
- Guarda como **CSV UTF-8**. Los encabezados pueden escribirse como `precio_compra`, `costo`, `precio público`, etc. (no importan mayúsculas ni tildes). Acepta archivos separados por coma o por punto y coma.
- **Importar reemplaza el catálogo completo y el stock** por lo que trae el archivo. Antes, la app muestra un resumen con avisos (filas sin precio, mayorista mayor que el público, precio de compra mayor que el de venta, SKU repetidos).
- El catálogo importado queda **en ese equipo**. Para que la tienda pública lo muestre, los productos deben estar en el código (`BASE`) o en un servidor.

## Resumen y ganancias (solo administrador, con PIN)
**Panel → 📊 Resumen y ganancias** pide el PIN del administrador cada vez. Muestra ventas, ganancia real, margen, costo de lo vendido, descuentos, documentos y unidades del día elegido, un gráfico de ventas y ganancia (7 días, 30 días o 12 meses), el detalle por vendedor y los productos del día. Ganancia = precio vendido menos precio de compra (guardado al momento de la venta). Las ventas anuladas no cuentan.

## Ingresar producto nuevo
**Panel → 📦 Ajustar stock → ➕ Ingresar producto nuevo** agrega un producto al catálogo con sus precios, precio de compra (solo administrador) y stock inicial.

## Cobro, cuadre de caja y deudas
- Al generar una nota de venta o proforma se abre **💵 Cobrar**: efectivo recibido (con **vuelto** automático), **Yape / Plin** (un solo campo), depósito / transferencia y crédito. Se pueden combinar. El botón **↳ resto** completa el saldo en el medio elegido.
- **Panel → 💰 Cuadre de caja** (admin y vendedores; cada vendedor ve solo lo suyo): venta total del día, efectivo (ya sin vuelto), Yape + Plin, depósito, crédito, efectivo esperado en caja, lista de ventas con cliente, DNI/RUC y forma de pago, y **clientes que deben** resaltados con su deuda. Se exporta a CSV y se imprime.
- Las ventas **a crédito** exigen nombre o DNI/RUC del cliente. Los **abonos** se registran con **💵 Abono** y entran a la caja del día en que se cobran.
- El recibo muestra el efectivo recibido, el vuelto y cada medio de pago.

## Nota de venta web (pedidos desde la tienda)
El cliente arma su pedido en la tienda, escribe su nombre y celular y toca **🧾 Generar mi nota de venta**: obtiene un número (`NW-ddmmaa-nnnn`) y la envía por WhatsApp. El mensaje lleva un enlace con el pedido dentro. El vendedor abre el enlace (o pega el mensaje en **📥 Pedidos web**) y el pedido se carga en el carrito con el cliente, el tipo de precio y la entrega.
- No usa servidor: el pedido viaja en el mensaje. Los productos se identifican por **SKU**, así que el catálogo del vendedor y el de la tienda deben tener los mismos SKU.
- El sistema avisa si un pedido ya se vendió y marca cada pedido como pendiente, cargado o vendido.

## Reportes en Excel con vista previa
Cuadre de caja, ventas y documentos, balance de almacén y resumen de ganancias tienen el botón **👁 Vista previa**. Desde ahí se ve el reporte tal como saldrá y se elige **⬇ Descargar Excel (.xlsx)**, **🖨 Imprimir** (A4 horizontal) o CSV.
El Excel es un archivo real con varias hojas, banner azul con tu logo y título, encabezados celestes, filas con borde, totales y filas resaltadas para las ventas a crédito. Las columnas siguen este orden: serie, número, tipo, sucursal, cliente doc, cliente nombre, fecha de emisión, fecha de creación, forma de pago, efectivo, Yape, Plin, depósito, crédito, total, estado y usuario.
