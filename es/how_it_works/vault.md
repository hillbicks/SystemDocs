# Vaults
Los Vaults se crean en el ordenador del usuario cuando se instala el cliente Maidsafe y se une a la red SAFE.

El Vault de un usuario no puede ser visto por este. En lugar de ello, un usuario ve un nuevo disco virtual montado en su ordenador que proporciona acceso a sus datos distribuidos.

Cuando un usuario crea o altera un fichero de su disco virtual, este fichero es sometido a diversos procesos para asegurar su seguridad y hacer el mejor uso de los recursos de la red SAFE.

## Roles en cada Vault
Un Vault puede tener diferentes roles (llamados "persona") que realizan diferentes gestiones de los datos dentro de la red SAFE.
* **Client managers (Gestor de cliente)**<br/>
Un Vault Client manager recibe los chunks de datos de los Vault de usuarios.
* **Data managers (Gestor de datos)**<br/>
Estos Vaults gestionan los chunks de datos de los Vault Client manager. Tambien monitorizan la situación de la red SAFE.
* **Data holders (Depositario de datos)**<br/>
Los Vault Data holder se usan para guardar chunks de datos.
* **Data holder managers (Gestor de depositario de datos)**<br/>
Los Data holder managers monitorizan los Vaults de los Data holder. Informan a los Data Manager si alguno de los chunks está corrompido o ha sido cambiado. Tambien informan si algún Data Holders está fuera de linea.
* **Vault managers (Gestor de Vaults)**<br/>
Los Vault manager mantiene el software actualizado y el Vault ejecutandose; reiniciandolo en caso de error.
* **Transaction managers (Gestor de tranferencia)**<br/>
Los Transaction manager ayudan a gestionar las transferencias de safecoin.

## Datos en la red SAFE

Hay dos posibles mecanismos que utiliza la red para autorizar a un usuario final llevar a cabo algunas acciones via el programa Cliente. Cuando el usuario guarda datos, la autoridad se obtiene por consenso del grupo donde se guardan dichos datos. Si se desea modificar datos guardados anteriormente o enviar safecoin se requirirá de una [firma digital](http://es.wikipedia.org/wiki/Firma_digital).

**Consenso de grupo**<br/>
Cuando un usuario final intenta guardar un nuevo dato, el fichero es encriptado y dividido en trozos llamados chunks como parte del proceso de autoencriptado. Posteriormente se traslada al grupo de Client managers mas cercano. Esta cercania está medida por la distancia entre la ID del Vault cliente y los Vault mas cercanos dentro del espacio [XOR](http://es.wikipedia.org/wiki/Disyunci%C3%B3n_exclusiva). Esta es una distancia medida en terminos matemáticos y no geográficos. Por lo menos veintiocho de los treinta y dos Client manager deben alcanzar un consenso antes que la operación avance.

Los Client managers trasladan los chunks a los treinta y dos Data manager elegidos por la red por la cercania de su ID con la ID de los diferentes chunks. Por lo tanto la ID de los chunks determina su posición dentro de la red.

La red utiliza un enfoque de scatter/gather basada en el [Algoritmo de información dispersa de Rabin](http://people.seas.harvard.edu/~salil/rabin2011-slides/rabin2011-mitzenmacher.pdf), permitiendo pequeñas perdidas de datos (hasta 4 piezas) que no necesitan ser retrasmitidas

Una vez que se alcanza el consenso, los Data manager trasladan los chunks a los treinta y dos Data holder managers, los cuales a su vez lo trasladan a los Data holder para su guardado. Si un Data holder manager informa que un Data holder está fuera de linea, el Data manager decide, basado en los rangos asignados a los diferentes Vaults, en que otro Vault pondrá el trozo de dato.

De esta manera los chunks de datos del fichero original son constantemente monitorizados para asegurar en todo momento que los datos originales estén siempre accesibles para que puedan ser desencriptados por el usuario.

Cualquier movimiento de los chunk de datos solo será posible si se alcanza el consenso necesario (28 de 32) de los Vault circundantes. Los Vaults no pueden actuar en solitario.

Todas las comunicaciones en la red SAFE se llevan a cabo a través de grupos cercanos de 32 nodos. Esto previene que un nodo desobediente se comporte mailiciosamente. No es posible, para un usuario, elegir la ID de su nodo o decidir donde guardar su información, ya que la red se encarga de ello. Cada vez que un nodo se desconecta de la red y se reconecta se le asigna una nueva y aleatoria ID.

[Click para ver un pequeño video de como funcionan los Vaults](https://www.youtube.com/watch?v=txvKSeCaEP0)

**Firma digital**<br/>
Cuando un usuario final desea realizar cambios de datos existentes, como el contenido de un fichero, o enviar safecoin a otro usuario, la red no utiliza el sistema de consenso ya que no es necesarío este nivel de complejidad y carga de red.

Una firma digital valida matemáticamente al titular de cualquier trozo de datos que puede probar, más allá de cualquier duda, que es su poseedor. Y esto siempre y cuando el usuario haya mantenido su clave privada segura. Si el usuario final es el propietario de cualquier pedazo de datos, y puede probar esto firmando digitalmente su solicitud con su clave privada, la red le permite el acceso para cambiar los datos.

