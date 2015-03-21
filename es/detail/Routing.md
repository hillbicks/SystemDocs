## Descripción general librería Routing

El Routing es una librería de Tabla de Hash Distribuida basada en tablas de enrutamiento similares a [Kademlia] (http://es.wikipedia.org/wiki/Kademlia). El Routing especifica la estructura de la red y determina el camino entre el par de nodos en la red mediante el uso de la información local en cada nodo intermedio.
Cada nodo de enrutamiento local almacena información acerca de los nodos a los que está directamente conectado. Por otra parte, cada nodo tiene un conocimiento parcial de la información local de sus nodos cercanos (ID de espacio vecino).
La información guardada en cada nodo, contribuye a la infraestructura de envío de mensajes de enrutamiento. Intercambiar información de ruta supone, habitualmente, atravesar un número indeterminado de nodos. La comunicación, entre cada par de nodos, se logra empleando [MaidSafe-RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki). Un mecanismo de reconocimiento/retransmisión se proporciona, tambien, para asegurar la entrega fiable de mensajes.
Su capacidad para ofrecer una plataforma eficiente, para el intercambio de mensajes entre pares, lo hace ideal como componente de enrutamiento de cualquier sistema P2P.


El Routing ofrece las siguientes características:
* Unión a la red
* NAT transversal
* Modos de operación
* Mensajería fiable directa y de grupo
* Evaluación de proximidad
* Identificación de nodo a través de PKI proporcionada por MaidSafe-Passport
* Mecanismo de almacenamiento en caché
* Gestión de la rotación
* Tabla de enrutamiento y grupo matriz

La interfaz de la librería está disponible en los siguientes archivos:
* [routing_api.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/routing_api.h) - esta es la API principal y se discute en más detalle más adelante
* [api_config.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/api_config.h) - 
proporciona declaración de tipos que se puede usarse al utilizar la librería
* [node_info.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/node_info.h) - proporciona información acerca de un nodo
* [parameters.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/parameters.h) - proporciona las variables de configuración de la librería
* [return_codes.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/return_codes.h)

### Sinopsis

### Unirse a la red
Un nodo puede unirse a la red como
1. un nodo normal cuando la red ya exista,
2. como un nodo de Estado Cero, cuando el nodo es uno de los dos primeros nodos que crean la red.

####Unirse como un nodo normal

Para unirse a una red existente, un nodo debe estar provisto de una lista de puntos de conexión que son una serie de nodos que ya forman parte de la red. El nodo intenta conectarse a cualquier punto de conexión de la lista. Una vez que un intento de conexión tiene éxito, el nodo que se une localiza y se conecta a su nodo más cercano [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc). Conectarse a su nodo más cercano [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) es vital para el funcionamiento fiable de un nodo. El resto de la tabla de enrutamiento está poblado principalmente por nodos al azar para permitir el acceso uniforme a diferentes partes de la red.

####Unirse como un nodo de Estado Cero

El ajuste inicial de la red, que se denomina Estado Cero, implica dos nodos. Estos dos nodos se ofrecen con los puntos finales de cada uno y conectándose uno a otro forma la red. Los nodos de Estado Cero son idénticos a otros nodos de la red y pueden abandonar/unirse a la red de forma similar al resto de los nodos.

###NAT Traversal

Un objetivo del Routing ha sido permitir la comunicación entre cada par de nodos en la red, independientemente de sus ajustes de configuración de red. El Routing en combinación con [MaidSafe-RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki) ejecuta [hole punching](http://www.brynosaurus.com/pub/net/p2pnat/) para permitir la comunicación directa entre cada par de nodos.
El hole punching se puede conseguir siempre y cuando los dos nodos no estén detrás de routers simétricos. Si ambos nodos están detrás de routers simétricos, el Routing logra la comunicación entre los dos nodos gracias a la selección de un tercer nodo que actúa como proxy entre ambos nodos situados detrás de routers simétricos.


###Modos de Operación
####No-cliente

Un nodo no-cliente es un nodo de enrutamiento completo que contribuye a la operación y mantenimiento de la red. Los nodos no-clientes son parte del DHT y son parte activa en la toma de decisiones de enrutamiento.
Un nodo no-cliente puede :
* Enviar solicitudes a cualquier nodo no clientes
* Enviar solicitudes a nodos cliente conectados
* Recibir las solicitudes entrantes desde cualquier nodo de la red

####Cliente

En contraste con los nodos que son no-clientes, un nodo cliente no contribuye a la infraestructura de enrutamiento de red.
Los nodos cliente son nodos de enrutamiento ligeros que usan los mínimos recursos de la red.
Estos nodos tendrán acceso a toda la red mediante el establecimiento de la conexión  [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) con los nodos cercanos a su propia ID.
Un nodo cliente puede :
* Enviar solicitudes a cualquier nodo no-cliente
* Enviar solicitudes a los clientes con la misma ID
* Recibir las solicitudes entrantes sólo de nodos no-clientes conectados

###Mensajería fiable directa y de grupo

Routing ofrece dos tipos de comunicación; directa y de grupo.

####Mensajería directa

En la mensajería directa el destino es un nodo conocido en la red y es el destinatario final del mensaje.

####Messajería de grupo

Un grupo se compone de un número de nodos que están próximos a una cierta ID. El número de miembros del grupo
se define por [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc).

En la comunicación de grupo, el destino del mensaje, es el grupo de nodos que más cerca está del destino. Un nodo que comparta ID con el destino no será parte de una comunicación en grupo.

###Tabla de enrutamiento y Matriz de Grupo
La tabla de enrutamiento es el componente principal de la biblioteca de enrutamiento. La tabla de enrutamiento almacena información sobre un número de nodos en la red con el fin de realizar decisiones de enrutamiento para entregar mensajes. Cada entrada de la tabla de enrutamiento corresponde a una conexión directa desde el nodo de enrutamiento para ese nodo.
Cada nodo de la red tiene una ID de 512 bits. La distancia entre cada par de nodos de la red se calcula calculando el XOR del par.
Un funcionamiento fiable y eficiente del enrutamiento de red requiere que cada nodo i) sea consciente de los nodos próximos y, ii) tener acceso a las diferentes partes de la red. La tabla de enrutamiento almacena información sobre los nodos más cercanos*[Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc):
* Escogiendo al azar nodos en la red
* Cualquier otro nodo que encuentre el nodo actual como uno de sus [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc). nodos más cercanos

La información anterior almacenada en la tabla de enrutamiento es suficiente, en la mayoría de los casos, para permitir las tomas de decisiones correctas. Sin embargo, podría ser insuficiente, para tomar decisiones precisas, cuando dos nodos tienen diferentes puntos de vista con respecto a la cercanía entre sí o de otro nodo. Para manejar estas situaciones, la tabla de enrutamiento está equipado con una matriz de grupo.
La idea detrás de la matriz del grupo es proporcionar a los nodos más conocimiento de la zona de la red en la que residen. Esto se realiza haciendo que cada nodo sea parcialmente consciente de los nodos en la tabla de enrutamiento de sus vecinos más cercanos. Una matriz de grupo de un nodo contiene:
* (i) Los nodos más cercanos de un nodo [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc)
* (ii) Otros nodos que se consideren próximos [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc)
* (iii) Los nodos más cercanos de cada entrada en (i) y (ii)

Se necesita información actualizada, en la tabla de enrutamiento y la matriz del grupo, para poder tomar decisiones correctas. Por lo tanto, el enrutamiento ofrece algunos servicios; para reflejar rápidamente las actualizaciones en la tabla de enrutamiento de un nodo, y la tabla de enrutamiento o grupo matriz de todos los nodos que deban estar al tanto de las actualizaciones.


###Evaluación de proximidad

La mayoría de las operaciones en la red se realizan en el grupo de nodos que están próximos a una ID dada. La tabla de enrutamiento, junto con la matriz del grupo, facilita nodos con un conocimiento suficiente de la parte de la red (al que pertenecen) para identificar con precisión otros nodos que comparten la misma ID de grupo.
El nodo que es parte de un grupo siempre será consciente de otros nodos en el grupo. Basándose en este conocimiento, el api de enrutamiento proporciona métodos para trabajar:
* Si un nodo está cerca de una determinada ID
* Si un nodo es parte de un grupo dado
* Los nuevos y antiguos miembros del grupo despues de una rotación de red

Basadose en la media distancia/población de la red, proporciona, tambien, un método para estimar si un nodo está lo suficientemente cerca de una ID dada.

###Gestión de las Rotaciones

En una red P2P, la unión y el abandono, son eventos comunes. El cambio de par, a menudo referido como rotación, se gestiona de manera eficiente por la biblioteca de enrutamiento. Por ejemplo, el alta o baja de un nodo en la red se refleja en las tablas de enrutamiento de sus nodos más próximos en unos pocos segundos.

###Cambio de Matriz (Evento de Rotación)

En la red de encaminamiento, los datos normalmente se almacena en un ID de grupo lógico. Esto significa que los datos se almacenan en los nodos que , primeramente, se encuentran  primera[Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) más cerca a una ID de grupo dado.
Cualquier rotación de red puede dar lugar al movimiento de un nodo acercándose o alejándose de una ID de grupo en ese segmento de la red. Esto significa que muchos de los grupos lógicos son obligados a reconfigurarse con nuevos miembros en el grupo y la pérdida de algunos de los antiguos miembros del grupo.

En el caso que:

1. Uno o varios nodos desaparecen de un grupo lógico: Nuevos nodos estarán cerca del grupo ID y se convertirán en los más cercanos a la ID de grupo [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc). Después de esto, todos los mensajes de grupo enviados al grupo ID, serán recibidos, también, por los nuevos nodos. Es responsabilidad de los demás nodos restantes de ese grupo el replicar datos rápidamente en los nuevos nodos.
2. Nuevos nodos aparecen en un grupo lógico : Alguno de los nodos miembros del grupo se moverá lejos de la ID de grupo y no permanecerán en [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) cercanos a la ID de grupo. Además de esto, estos nodos no recibirá ningún mensaje de grupo destinado a la identificación de dicho grupo. En estos nodos se debe suprimir los datos sobre los que ya no son responsables.


Para mantener de forma fiable los datos con vida y accesibles en esta dinámica de grupo, todos los nodos responsables deben replicar los datos tan pronto como tengan conocimiento de la rotación de red. El Routing ofrece la clase MatrixChange para detectar tal rotación de forma rápida y realizar, de forma fiable, el trabajo requerido para replicar los datos en cualquier nuevo nodo que aparece en el grupo. Los nodos cerca de un segmento de red sabrán de un evento de rotación observando un cambio en su matriz de grupo. Cuando la matriz de grupo cambia en cualquier rotación, el enrutamiento crea un objeto MatrixChange, que contiene la lista de nodos de la matriz del grupo nuevos y viejos. Esta clase proporciona una función que ayuda a evaluar a cualquier ID de grupo dado en el caso que un nodo reciba un evento de rotación:

* Todavía es parte del grupo y responsable de los datos almacenados en el grupo.
* Ya no es parte del grupo y hay que borrar los datos almacenados en relación con el grupo.
* Necesidad de replicar los datos en un nuevo nodo.

```C++
  CheckHoldersResult CheckHolders(const NodeId& target) const;
```

CheckHoldersResult proporciona la lista de los nuevos y antiguos titulares del grupo y proximity_status del nodo que recibe el evento de rotación del grupo que recibió ID.


```C++
struct CheckHoldersResult {
  std::vector<NodeId> new_holders;  // New holders = All 4 New holders - All 4 Old holders
  std::vector<NodeId> old_holders;  // Old holders = All 4 Old holder ∩ All Lost nodes
  routing::GroupRangeStatus proximity_status;
};
```




###Mecanismo de caché

Para permitir el acceso rápido a los contenidos populares, Routing ofrece algunos servicios que permiten que los datos se almacenen en el caché de los nodos intermedios o para poder leer los datos almacenados en caché de los nodos intermedios si hay datos disponibles. La funcionalidad se logra mediante el establecimiento de una bandera en el mensaje de enrutamiento. Si el tipo de mensaje es una solicitud, la memoria caché, en cada nodo intermedio hasta el destino final, está marcada por los datos. Si se dispone de datos en caché, el nodo intermedio servirá la solicitud y se envía una respuesta al remitente. Si el tipo de mensaje es una respuesta, los datos se almacenan en cada nodo intermedio desde el destino a la fuente.


###Devolucion de llamada

El ofrecer una plataforma eficiente para el intercambio de mensajes entre pares hace del routing un componente de comunicación ideal de cualquier sistema P2P. Para permitir la utilización fléxible y sencilla de los componentes de enrutamiento, el enrutamiento proporciona una serie de funciones de devolución de llamada a los componentes del sistema principal. Algunas de estas devoluciones de llamada son obligatorias y deben figurar en la aplicación host. Otras devoluciones de llamada podeen características adicionales y son opcionales para la aplicación host.

Funciones de devolución de llamadas de Routing:

* **MessageReceivedFunctor** : Se llama cuando se recibe un mensaje a nivel de nodo. Un mensaje de petición de nivel de nodo es un mensaje entrante destinado a la capa de aplicación usando un nodo como enrutamiento.

* **NetworkStatusFunctor** : Se llama cuando se establece una nueva conexión o sucede una caída de conexión. Se muestra el porcentaje de salud en términos de conexión del nodo a otros pares de nodos válidos en la red o, alternativamente, puede hacer referencia a la salud de la tabla de enrutamiento.

* **MatrixChangedFunctor** : Cualquier evento de rotación, que resulte en el cambio de matriz de un grupo de nodos, desencadena una llamada a este funtor.

* **RequestPublicKeyFunctor** : Como parte del proceso de conexión a un compañero no cliente, el enrutamiento debe ser proporcionado con la clave pública del par. Esto se logra por la infraestructura de PKI. La devolución de llamada proporciona otra devolución de llamada (GivePublicKeyFunctor) que debe ser llamado con la clave pública válida de la conexión entre los pares no cliente

* **HaveCacheDataFunctor** & **StoreCacheDataFunctor** : Se llaman para buscar datos en el caché o almacenar datos en la memoría caché de un nodo intermedio

* **NewBootstrapEndpointFunctor** : Es llamada cuando un nodo de enrutamiento se conecta a un compañero cuyos puntos finales son nodos capaces de bootstrapping. Dado que la red es muy dinámica, esta información es importante para volver a reconectarse a la red. Una lista de estos puntos finales se debe suministrar al enrutamiento para volver a conectarse a la red.

