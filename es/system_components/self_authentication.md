# Eficiencia - Self authentication

La red SAFE posee self authentication (autentificación propia). Este es un mecanismo que permite a los usuarios crear cuentas en la red SAFE e iniciar la sesión sin necesidad o conocimiento de un tercero.

Para lograr esto, los privilegios de autenticación (token de inicio de sesión) tienen que ser almacenados en la red de forma segura como parte de dicho proceso.

Un usuario crea su propia clave (K) y Password (W). Una serie de bits llamados [sal] (http://es.wikipedia.org/wiki/Sal_%28criptograf%C3%ADa%29A) son creados (de forma reproducible) de la clave y la contraseña.

Entonces se crea un identificador único, un hash de la concatenación de la clave y la sal, H(K + S).

Se realiza una derivación de clave [(PBKDF2)] (http://en.wikipedia.org/wiki/PBKDF2) para fortalecer el password. Esto ocurre porque normalmente los usuarios suelen usar password débiles.

Finalmente el permiso de acceso encriptado se guarda en la red SAFE usando la siguiente estructura:

Guardado en la red [H(K+S)] Encriptación simétrica [PBKDF2[P][S]] Cuenta

Self authentication se basa en un sistema donde un Vault puede crear una clave única, para almacenar un valor en la Red SAFE. El valor almacenado con esta clave debe contener un pasaporte cifrado a los datos.

Este pasaporte contiene claves criptográficamente seguras y / o una lista de otras claves usadas para que la información sea almacenada o compartida.

La situación de esta clave inicial se enmascara o, por lo menos, su situación no es obvia dentro de la red SAFE. Este enfoque es la base de la self authentication y se extiende por toda la red SAFE para permitir acceder a los datos guardados de forma pública sin necesidad de requisitos adicionales como cortafuegos o controles de acceso.

##Adivinando la información de acceso

Un intruso o persona hostil en la red pueden detectar la presencia de un usuario y tratar de descargar el token de inicio de sesión. Esto es similar a probar continuamente diferentes contraseñas en un servidor centralizado. En servidores centralizados esto se trata de evitar restringiendo los intentos de registro.

En la red SAFE, cuando un usuario solicita un token de inicio se le proporciona uno. En cada intento que realice se le proporcionará un token inválido. Esto hace que un atacante tenga que desencriptar millones de tokens fuertemente cifrados. Este proceso hace que este tipo de ataque sea computacionalmente irrealizable.

El token de inicio de sesión sólo es útil para la autentificación después de que se validen con las diferentes contraseñas de usuario que nunca se envían a la red SAFE.

