### Descripción general librería Encrypt

MaidSafe-Encrypt implementa la función relacionada con la autoencriptación de ficheros y carpetas.

La interface de la librería proporciona los siguientes ficheros:

* [self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h) - Esta es el API principal y se comenta con detalle más abajo
* [data_map.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/data_map.h) - una estructura que mantiene los detalles de encripción de un fichero único
* [config.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/src/maidsafe/encrypt/config.h) - proporciona variables de configuración de librería const y códigos de retorno. En el futuro los codigos de retorno se cambiarán por un mecanismo de gestión de errores proporcionado por [MaidSafe Common library](https://github.com/maidsafe/MaidSafe-Common/wiki).

La librería hace uso de [OpenMP](http://en.wikipedia.org/wiki/OpenMP) donde sea posible para paralelizar las tareas.  También depende de la librería [Common](https://github.com/maidsafe/MaidSafe-Common/wiki), [Private](https://github.com/maidsafe/MaidSafe-Vault-Manager/wiki), [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki), [RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki), [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) and [Network-Filesystem](https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki).


### Antecedentes
En la [Auto-encriptación] (http://maidsafe.net/images/opportunistic-caching.png)
un archivo no necesita ninguna otra entrada que el propio archivo. Da como resultado varios trozos cifrados de datos que hay que poner a un almacén persistente de clave-valor (por ejemplo, un Vault de la Red MaidSafe) y un Mapa de datos.

El Mapa de datos contiene dos listas; una que contiene la codificación previa [hash SHA512] (https://es.wikipedia.org/wiki/SHA-2) de los trozos del archivo y la otra que contienen los hashes post-cifrado. Los hashes pre-cifrado de otros dos trozos del archivo se utilizan en el cifrado/descifrado de un solo trozo. Los hashes post-cifrado son los nombres de los trozos cifrados, es decir, la clave en las que se almacenan en la DHT. El resultado es que con un mapa de datos, cualquier usuario puede recuperar los trozos cifrados y descifrar el archivo original.

En el caso de los directorios, el supuesto es que el contenido (una colección de Mapa de datos y meta-información) pueden ser serializado en un solo archivo. Este archivo, si está autoencriptado, da su propio Mapa de datos que también necesita ser puesto en el almacén de claves-valor. Sin embargo, el mapa de datos no se puede almacenar sin cifrar ya que el contenido debe ser secreto. Estos mapas de datos de directorio son encriptados/desencriptados utilizando dos piezas de información adicional. En el caso de la Red MaidSafe, la primera pieza es normalmente un ID aleatorio que se atribuye a esa carpeta. El segundo es la ID de la carpeta principal. Las identificaciones son adecuados para su uso como claves en el almacén de claves y valores, es decir, son hilos del mismo tamaño que un hash SHA512. La ID de un directorio dado se convierte en la clave en la que el Mapa de datos cifrada se almacena.


### Detalles

[self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h) Proporciona functores libres para la encriptación y desencriptación de los Mapas de datos. Tambien declara la clase `SelfEncryptor` que se usa en la encriptación/desencriptación de un fichero.

La clase `SelfEncryptor` está diseñada para ser compatible con los tipos de llamada que se esperan bajo un sistema de ficheros virtual (VFS) como [FUSE](http://fuse.sourceforge.net/).  Está diseñada para ser una clase de usar y tirar; Se construirá una nueva instancia cuando un identificador de archivo se abre en la VFS y será destruido cuando se cierre. Como tal, la clase es [segura en hilos](http://es.wikipedia.org/wiki/Thread_safety#Levels_of_thread_safety).

El constructor requiere un puntero a un mapa de datos, el acceso de la DHT a través de una ref no const a una` NFS :: ClientMaidNfs` y el acceso a una memoria intermedia, para trozos temporales, a través de una ref no const a un `data_store :: PermanentStore `. Por otra parte, el número de hilos que se utiliza en la encriptación/desencriptación puede ser proporcional.

Mas detalles de las funciones individuales se pueden encontrar en el fichero [self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h).


[se]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h "#include "maidsafe/encrypt/self_encryptor.h""
[dm]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/data_map.h "#include "maidsafe/encrypt/data_map.h""
[config]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/src/maidsafe/encrypt/config.h "#include "maidsafe/encrypt/config.h""

[common]: https://github.com/maidsafe/MaidSafe-Common/wiki "MaidSafe-Common library"
[private]: https://github.com/maidsafe/MaidSafe-Private/wiki "MaidSafe-Private library"
[passport]: https://github.com/maidsafe/MaidSafe-Passport/wiki "MaidSafe-Passport library"
[rudp]: https://github.com/maidsafe/MaidSafe-RUDP/wiki "MaidSafe-RUDP library"
[routing]: https://github.com/maidsafe/MaidSafe-Routing/wiki "MaidSafe-Routing library"
[nfs]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki "MaidSafe-Network-Filesystem library"
[se_video]: http://maidsafe.net/self-encrypt-video

[omp]: http://en.wikipedia.org/wiki/OpenMP
[sha2]: https://en.wikipedia.org/wiki/SHA-2 "Secure Hashing Algorithm v2 - SHA512"
[fuse]: http://fuse.sourceforge.net/ "Filesystem in Userspace"
[thread_safety]: http://en.wikipedia.org/wiki/Thread_safety#Levels_of_thread_safety "Levels of thread safety"

