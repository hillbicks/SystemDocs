# Solicitudes Safecoin y roles
Hay diferentes tipo de solicitud para los safecoin.
* PUT - utilizado para poner safecoins (datos) en un lugar determinado
* GET - se utiliza para obtener safecoins (datos) desde una ubicación especificada
* EXCHANGE - se usa para cambiar safecoins (datos) entre dos ubicaciones especificadas

Los safecoin son otro tipo de datos y disponen de peticiones PUT y GET definidas para ellos en la red SAFE. Sin embargo, a diferencia de los datos normales, no hay petición de borrado (DELETE) disponible.

La solicitud PUT para los safecoins tiene una propiedad de "no permite duplicación". Esto significa que si ya hay una safecoin con el mismo nombre (primeros 32 bits), la petición PUT es rechazada. Esta comprobación es gestionada por el Data manager que recibe la solicitud.

Un orden EXCHANGE permite a un solicitante actualizar los detalles del safecoin pero sólo si sigue las normas necesarias.

* El propietario es aprobado por la mayoría de los Vaults de un tercer grupo (escrow).
* El propietario puede (el dueño anterior y actual se consideran aprobados por si mismos) actualizar todos los campos.
* Cada Vault del tercer grupo (escrow) solo puede actualizar su campo correspondiente una sola vez.
* Cada vez, que los campos del propietario anterior y el actual se actualizan, la versión de safecoin se incrementa en uno y todos los campos de escrow se borran.

Las reglas anteriores son impuestas por el PMID manager que guarda los datos safecoin. A medida que el campo de propiedad, junto con el tercer grupo de Vaults (escrow), se utiliza como una «transacción», el PMID manager se convierte, de forma efectiva, en Transaction manager. En este caso, los datos safecoin pueden ser considerados, tambien, como un recibo.
