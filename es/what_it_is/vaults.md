# Vaults
La red SAFE consiste en diferentes procesos (nodos) que se conocen como Vault. Cada Vault reside en el disco duro del usuario y cumple funciones múltiples, que se especializan y adaptan a las necesidades de la red. Una de las funciones principales es el almacenamiento de datos encriptados que otros Vault se encargan de hacérselos llegar.

Otra función, o "persona", de un Vault es gestionar otros Vault. En la medida que las piezas de datos se envían de un Vault a otro, una parte gestora del Vault detecta y monitoriza la integridad de dichos datos.

Un Vault también puede tener las identidades de gestor de transacciones o validador. Estas identidades se encargan de gestionar las transferencias de safecoin.

Los Vaults no saben nada de los datos que gestionan o que guardan (éstos solo son desencriptables por el cliente) y, debido a que los datos son solamente una pequeña porción del total, no es posible descifrar su fuente original, ya sea, por ejemplo, un documento o una comunicación.

Por cada Vault que guarda un dato, hay otros treinta y uno que también se encargan de guardar el mismo dato. De esta forma, si un Vault se apaga o sus datos se corrompen, no se pierde ningún dato. Inmediatamente, y de forma automática, se encuentra otro Vault que se encarga de guardar dichos datos.

Los Vaults no son visibles para los usuarios de la red SAFE. Lo que un usuario ve es un disco virtual montado en su ordenador. Mientras tanto, los diferentes Vaults se comunican y monitorizan unos a otros para que el acceso a los datos de dicho disco virtual sea instantáneo.
