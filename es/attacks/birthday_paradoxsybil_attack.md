# [Paradoja del cumpleaños](http://es.wikipedia.org/wiki/Paradoja_del_cumplea%C3%B1os)/[Sybil attack](http://en.wikipedia.org/wiki/Sybil_attack)

##Descripción del ataque
En este ataque, el atacante llena la red de Vaults que controla a fin de rodear a un Vault en concreto con tres o más Vault maliciosos que le permitirá tomar el control de dicho Vault.

##Propósito del ataque

Utilizando este proceso, un atacante podría actuar como un Data manager pidiendo el borrado de los chunk de datos de los Vault controlados. Esto podría hacer que los Data holders eliminaran los trozos en respuesta a una solicitud aparentemente correcta, e impidiendo el acceso a los datos para los usuarios legítimos.

Si bien no es posible colocar deliberadamente Vaults maliciosos en torno a un punto deseado en la red SAFE, con alrededor de 0,8% de los Vaults de la red bajo el control (temporal) de un atacante, es probable que el atacante tendría al menos uno de los Vault rodeado, lo que le permitiría ejercer el control sobre ese Vault y alcanzar el quórum necesario para realizar este tipo de acciones falsas.

##Evitar el ataque

La red SAFE requiere que todas las peticiones sean procesadas al menos por dos grupos de Vaults.

Un cliente Maidsafe envía una petición a sus cuatro Data managers, que verifican la petición basándose en la clave del cliente. La solicitud se pasa entonces a un grupo seleccionado de forma determinista de otros cuatro Vaults que también verifican la solicitud basándose en su firma.

Al seleccionar determinísticamente el segundo grupo de Data managers, este ataque ya no es válido para la red SAFE, ya que, para el atacante, no le es posible obtener el control de un Vault simplemente rodeándolo.

Para evitar esto, el atacante requeriría la capacidad de rodear Vaults específicos en la Red SAFE. Esto no se puede lograr, ya que requeriría ser capaz de generar, de forma efectiva, diferentes valores que, cuando se realice su hash con SHA-512, den como resultado hashes alrededor de un punto en particular.
