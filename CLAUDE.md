# Casa Rossi — administración

Sitio para administrar el alquiler temporario de Casa Rossi (Rafaela, Santa Fe): reservas con sus pagos, gastos, comisiones e informe trimestral en PDF.
Producción: https://casa-rossi-admin.vercel.app. Se publica solo con cada git push a main.

## Estructura
- index.html: toda la app (HTML, CSS y JS en un solo archivo).
- api/datos.js: función de Vercel. GET y PUT de los datos en un Vercel Blob privado, protegida con el header x-clave contra la variable CASA_CLAVE.
- package.json: dependencia @vercel/blob.

## Datos
- Blob privado "casa-rossi-datos", archivo casa-rossi/datos.json con {rev, updatedAt, data: {reservas, gastos}}. Copia diaria en casa-rossi/historial/.
- Control de concurrencia por rev. Copia local en localStorage "casa_rossi_v1".
- Variables en Vercel: CASA_CLAVE, BLOB_READ_WRITE_TOKEN, BLOB_STORE_ID (prefijo BLOB). Nunca escribir la clave en el código ni en el repo.
- Los nombres y teléfonos de huéspedes nunca van al repo (ni precargas ni backups).

## Reglas de negocio
- Todo se reporta en USD. Conversión con TC BNA vendedor de la fecha de cada operación (vendedor = el TC al que se compran dólares).
- Seña del 50% al reservar y saldo después. Los pesos pactados quedan fijos al TC del día de la reserva.
- Diferencia de cotización por pago = monto / TC del pago − monto / TC de la reserva.
- Algunos huéspedes pagan directo en USD, sin conversión.
- Comisión de administración: 10% del total de cada reserva, estado pendiente o compensada. Entra sola en el informe como gasto; no se carga también en Gastos.
- Ingresos devengados prorrateados por noche dentro del trimestre.

## Estilo visual
Noche con estrellas y silueta de montaña, cinta retro de cuatro franjas (#E3A33B, #D8662C, #B8322A, #5A3420), títulos en Ultra, cuerpo en Barlow, paneles de vidrio semitransparente, efecto holograma al pasar el mouse. El informe impreso va siempre en fondo claro.

## Forma de trabajo
- Windows y PowerShell. Las carpetas de usuario están en F:\Users\Usuario, no en C:.
- Hablar en español rioplatense, informal.
- Probar los cambios antes de proponer el push. Pedir confirmación antes de cualquier git push, porque publica en producción.