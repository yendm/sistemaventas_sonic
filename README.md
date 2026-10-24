# SONIC – Catálogo y ventas para ferretería

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

## Importar productos (CSV)
Columnas: `nombre,categoria,subcategoria,marca,presentacion,precio,precioMayorista,cantidadMayorista,stock,imagen,alias,masVendido,sku`
(alias separados con `|`, decimales con punto, guardado en UTF-8). Las categorías nuevas aparecen solas.

## Balance de almacén
- Botón **📦 Balance** (barra superior en modo vendedor, y en el Panel): unidades vendidas por producto, monto, desglose por vendedor y por sucursal, y stock actual.
- El día va de 00:00 a **23:59**. Hoy aparece como "en curso" y se puede actualizar en cualquier momento; los días anteriores quedan "cerrados". Se elige el día con el calendario.
- Se puede imprimir (A4), enviar por WhatsApp (al número de almacén configurado) o descargar el archivo del día.
- Roles: **Administrador** y **Almacén** ven las ventas de todos; cada **Vendedor** ve solo las suyas.

## Varios vendedores, tablets y sucursales
Cada tablet guarda sus propios datos (no hay base de datos compartida). Para trabajar con varios equipos:
1. En **Panel → Personalizar recibo y marca**, asigna a cada equipo un **código distinto** (001, 002, 003…) y su sucursal. Los números de documento llevan ese código (NV001-000001, NV002-000001…), así no se repiten entre equipos.
2. Al cerrar el día, cada equipo descarga **⬇ Archivo del día** desde el Balance y lo envía al almacén (WhatsApp, correo, etc.).
3. El equipo de almacén usa **📂 Importar ventas de otro equipo**. Se suman las ventas al balance y se **descuenta el stock** de esas ventas en ese equipo.
Esto es una consolidación manual: el stock de cada tablet solo se actualiza cuando se importan los archivos de los demás equipos.
Para stock **en tiempo real** compartido entre vendedores y sucursales hace falta una base de datos en la nube (por ejemplo Firebase, Supabase o Google Sheets).

## Ajustar stock (ingreso de mercadería)
En **Panel → 📦 Ajustar stock** (administrador y almacén) se busca el producto y se usa **+ Sumar** para registrar un ingreso o **Fijar** para corregir el stock. Cada venta descuenta stock automáticamente en ese equipo. Importar un CSV de productos reinicia el stock.

## WhatsApp con varios vendedores
En **Personalizar** se pueden registrar hasta 3 números (principal + 2). Cuando el cliente toca "Pedir por WhatsApp", elige con quién hablar o se le asigna uno al azar.
