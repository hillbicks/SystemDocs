# Un ISP simula la red SAFE

##Descripción del ataque

En este ataque un ISP crea o simula una red SAFE falsa. En esta red, el ISP recibe los datos de un usuario pero no guarda los datos que el usuario demanda.

##Propósito del ataque

El propósito de este ataque es forzar la perdida de datos de un usuario o desacreditar a la red SAFE.

##Evitar el ataque

En este caso, y durante la conexión de inicio del cliente Maidsafe o Vault, no será posible conectar con la lista de Vaults de inicio (bootstrapping) listados en el fichero caché. Este fichero contiene la IP, puerto y clave pública de diferentes Vaults para el inicio de conexión.

Como la sesión de inicio está encriptada con la clave pública de los diferentes Vaults de bootstrapping, la petición de esta conexión será errónea. El cliente Maidsafe detectará que no estamos en una red válida.

##Descripción de ataque mejorado

La máquina de un ISP se hace pasar por la máquina del usuario y pide la información de arranque en el lugar del usuario.

##Evitar ataque mejorado

En este caso la máquina de un ISP puede pedir la información y pasársela al usuario. Pero esta información está encriptada para el usuario por lo que realmente es inaccesible para el ISP (a no ser que rompa la encriptación RSA 4096). Luego, el usuario se conecta a sus Vaults más cercanos. Si los Vaults más cercanos son falsos no serán capaces de procesar la solicitud de conexión, que se cifra en cada extremo. Así que de nuevo este ataque falla.

Esto sólo es aplicable para los clientes MaidSafe que aún no han iniciado sesión. Al recibir el paquete de inicio de sesión, el cliente MaidSafe encripta una solicitud a un Vault cercano para el bootstrapping. De nuevo, esto se devuelve cifrado, por lo que el ataque se elude.
