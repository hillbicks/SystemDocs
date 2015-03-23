# Conexiones en los routers
La red SAFE permite la conexión entre diferentes Vaults.

La mayoría de las conexiones en los hogares se realizan usando routers que proveen de conexiones privadas que no pueden retransmitirse directamente al exterior. Para ello, un mecanismo estándar es el uso de servidores [STUN](http://es.wikipedia.org/wiki/STUN).

Este mecanismo no es aceptable en una red descentralizada. En la red SAFE, el protocolo RUDP (reliable UDP) se usa como mecanismo para emular un servidor STUN descentralizado. Esto, junto a la tabla de Hash distribuida (DHT), proporciona la información de conexión para la negociación entre los diferentes Vault.

La red SAFE puede gestionar las conexiones sin que el usuario tenga que realizar ningún cambio o ajuste en su router.
