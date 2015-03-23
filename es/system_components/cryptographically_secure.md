# Cryptográficamente segura
En la red SAFE los Vault crean primeramente un [par de claves] (http://es.wikipedia.org/wiki/Criptograf%C3%ADa_asim%C3%A9trica) para unirse a la red.

En cuanto un Vault conecta con otro Vault, usa dicho par de claves para asegurar una conexión.

En contra del método tradicional, donde una conexión segura requiere del intercambio de claves como en el protocolo criptográfico [diffie Hellman](http://es.wikipedia.org/wiki/Diffie-Hellman), la red SAFE proporciona estas claves a los Vaults a través de la capa de [ruteo](https://github.com/maidsafe/MaidSafe-Routing).

El beneficio de esto es que incluso hardware comprometido puede ser atravesado por la red SAFE. Esto evita [Ataques Man In The Middle (MiTM)](http://es.wikipedia.org/wiki/Ataque_Man-in-the-middle).

En tanto en cuanto todos los mensajes entre Vault están encriptados, la seguridad de toda la red se ve enormemente reforzada.
