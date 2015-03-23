## Descripción general sistema de ficheros de red

El sistema de ficheros de red MaidSafe, NFS, es una librería basada en políticas que implementa un API RESTful. En concreto, la plantilla de clase principal NFS acepta una plantilla de parámetros, los propios tipos de clase, ya sea de plantilla o de otra manera, que definen las operaciones de red put, get, delete and post. El diseño escala sobre diversos parámetros y simplifica la lógica necesaria para empaquetar los mensajes para su distribución, a través de [Routing] (https://github.com/maidsafe/MaidSafe-Routing/wiki), en nombre de un cliente o [Vault] persona (https://github.com/maidsafe/MaidSafe-Vault/wiki).

### Detalles

El API abarca una serie de archivos que definen la plantilla de clase principal NetworkFileSystem, la política de clases y diversas estructuras de soporte/ayuda y funciones. La lista completa de los archivos, junto con una breve explicación de cada uno, es la siguiente. Tenga en cuenta que los archivos denominados con el siguiente esquema * -inl.h implementan funciones miembro no especializadas, plantillas miembros y plantillas de funciones declaradas en el archivo .h * correspondiente y no están incluidos en la lista.

* [maid_node_nfs](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/maid_node_nfs.h) define la plantilla de clase NetworkFileSystem cuya plantilla argumentos determinan la política de put, get, delete and post como un objeto instanciado que se usará para operaciones de red.
* data_policies.h incluye una plantilla de clase DataPolicy utilizado por los clientes y las diferentes de personas definidas en [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki).
* client_post_policies.h usado por un cliente para comunicaciones de red.
* client_utils.h contiene algunas funciones y clases utiles disponibles para un cliente.
* structured_data.h proporciona el interface de clase para guaradar versiones de datos estructurados.
* [message.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/messages.h) define la clase de mensaje genérico cuyo contenido empaquetado es enviado a un nodo receptor en la red responsable, ya sea directa o indirectamente, por su contenido. Tenga en cuenta que un mensaje puede requerir un número de saltos para llegar a su destino, los detalles de esto son atendidos por la aplicación DHT proporcionado por [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki).
* reply.h define la clase Reply, que se inicializa para proporcionar información sobre el resultado de una operación de red.
* person_id.h define una estructura que contiene la identidad de un nodo destino en la red, así como el tipo "persona" objeto des mensaje.
* [data_getter.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/data_getter.h) define una clase usada para recuperar varios tipos de claves públicas guardadas en la red, ver [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki) para los tipos de claves disponibles.
* [pmid_registration.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/pmid_registration.h) contiene una clase cuyos miembros permite un registro autentificado de un [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki).
* [types.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/types.h) incluye la conveniente definicion de tipos y la enumeración de los utilizados para la mensajería.
* [utils.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/utils.h) incluye funciones y clases útiles usadas por la librería.

### Applicación

Las transacciones/comunicaciones del cliente LifeStuff se empaquetan, via un objeto ClientMaidNfs, de la siguiente manera.

```C++
typedef PutDataPolicy<Maid, Persona::kClientMaid> Put;
typedef GetDataPolicy<Maid, Persona::kClientMaid> Get;
typedef DeleteDataPolicy<Maid, Persona::kClientMaid> Delete;
typedef ClientPostPolicy<Maid, Persona::kClientMaid> Post;

typedef NetworkFileSystem<Put, Get, Delete, Post> ClientMaidNfs;
```

Aquí, los ClientMaidNfs se define como una instancia de NetworkFileSystem, con políticas PutDataPolicy, GetDataPolicy, DeleteDataPolicy y ClientPostPolicy, todas las plantillas de clase, cada uno con los argumentos de plantilla para una firma de identidad y el tipo "persona" del objeto, un Maid y Persona::kClientMaid, respectivamente, en este caso.

La plantilla de clase NetworkFileSystem y su política definida, expuesta por esta librería, también se utilizan para componer los distintos objetos Nfs "Persona" requeridos por un [Vault's](https://github.com/maidsafe/MaidSafe-Vault/wiki).


[0]: https://github.com/maidsafe/MaidSafe-Routing/wiki
[1]: https://github.com/maidsafe/MaidSafe-Vault/wiki
[2]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/maid_node_nfs.h
[3]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/messages.h
[4]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/data_getter.h
[5]: https://github.com/maidsafe/MaidSafe-Passport/wiki
[6]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/pmid_registration.h
[7]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/types.h
[8]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/utils.h
