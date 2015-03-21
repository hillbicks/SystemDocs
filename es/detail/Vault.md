## Descripción general libreria Vault

La Red MaidSafe consiste en procesadores de software (nodos), conocidos como Vaults. Estas Vaults realizan muchas funciones en la red y estos sus componentes funcionales se conocen como "personas". La red subyacente, cuando se une con [MaidSafe-Routing] (https://github.com/maidsafe/MaidSafe-Routing/wiki), es una red XOR y como un tal, un nodo puede expresar la cercanía o responsabilidad repecto a cualquier otro nodo o elemento en la red, si este nodo se encuentra lo suficientemente próximo. En este resumen, la frase ** ** NAE (Net Address Element) se utiliza para referenciar a cualquier cosa con una dirección de red incluyendo datos.

Los vaults cuentan con el [MaidSafe-Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) para calcular las responsabilidades sobre los NAE a través de [llamdas API](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/routing_api.h) como por ejemplo
```C++
GroupRangeStatus IsNodeIdInGroupRange(const NodeId& group_id, const NodeId& node_id) const;
GroupRangeStatus IsNodeIdInGroupRange(const NodeId& group_id) const;
bool EstimateInGroup(const NodeId& sender_id, const NodeId& info_id) const;
```
Estas llamadas nos permiten calcular la red desde el punto de vista de cualquier NAE del que podemos ser responsables. Hay que insistir en que la única manera de determinar la responsabilidad sobre un NAE es ver a la red desde el punto de vista del propio NAE. Si clasificamos el vector de nodos que conocemos y sus nodos cercanos (referido como la matriz del grupo) y no aparecemos en los primeros K nodos (contando la replicación) entonces no somos responsables del NAE. Este es un tema fundamental y la importancia de esto no puede enfatizarse lo suficiente.

En tanto en cuanto la red es muy fluida en términos de rotación, Los Vaults deben ser capaces de medir e informar sobre cada Vault individual y esto es importante para asegurarse que todos las "persona" de cualquier Vault están realizando sus tareas para el NAE del que son responsables. Para facilitar esto, usamos la característica de Routing [Matrix change (Churn Event)](https://github.com/maidsafe/MaidSafe-Routing/wiki/Documentation#matrix-change-churn-event). En el caso de una rotación, alrededor de un segmento de red determinado, se crea un objeto de cambio de matriz por el Routing y se transmite al Vault. Este objeto contiene la lista de los nodos viejos y nuevos de la matriz del grupo. Basándose en esta información, esto proporciona una función de ayuda para obtener cierta información relacionada con cualquier NAE dado.
Si el nodo que recibe el evento churn está entre los k nodos más cercanos al proporcionado por el NAE. En caso afirmativo, el nuevo nodo(s) necesitará información relacionada con el NAE proporcionado. Si no es así, se eliminará cualquier información almacenada relacionada con el NAE dado.

La rotación, la duplicación de datos y garantizar que todos los miembros de un grupo están de acuerdo es gestionado por una combinación de sincronización, el acumulador y los mensajes del grupo. Se trata de un complejo conjunto de normas que requiere considerable atención en los casos difíciles.

# Terminos y convenciones usados
**_Por favor, tener en cuenta que estos términos se usan ampliamente en este documento y su lectura será muy dificil sin que sean totalmente comprendidos._**

* Sy - [Sync](#sync), esta función sincroniza datos entre nodos del mismo grupo (conectados por proximidad).
* Sr - [Sync](#sync), esta función sincroniza los resultados de los mensajes entre nodos del mismo grupo (conectados por porximidad).
* So - Enviar On, esta función envía el mensaje a la siguiente persona.
* Ac - Accumulate, esta función acumula mensajes de personas previas (las personas que mandan el mensaje a las personas actuales).
* Fw - Firewall, esta función asegura que a los mensajes duplicados se les impida progresar y conteste a dichos mensajes con la una respuesta calculada (como success:error:synchronising etc. puede incluir un mensaje que contienen datos)
* Uf - Update Firewall, esta función actualizará el firewall con el código de retorno recién calculado y un mensaje opcional.
* **Bold** representa Indirect Network Addressable Entities INAE
* _Italic_ representa Direct Network Addressable Entities DNAE
* ->>>> representa un mensaje de grupo (sin retorno)
* -> representa un mensaje directo (sin retorno)
* =>>>> representa un mensaje de grupo (con retorno)
* => representa un mensaje directo (con retorno)

### Identidades Maidsafe
La red MaidSafe consiste en muchos tipos de datos así como muchos tipos de identidad. Estas identidades se describen en [MaidSafe-Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki). Las "Personas" están particularmente enfocados en 4 de estas identidades y asegura que las entidades correspondientes cumplen los requisitos impuestos por estas identidades.

* MAID (MaidSafe Anonymous ID) - La identidad cliente para manipular datos no estructurados. Un cliente solo puede tener una.
* PMID (Proxy Maidsafe ID) - La identidad cliente para guardar de forma segura datos no estructurados. Un cliente puede tener varios.
* MPID (Maidsafe Public ID)- La identidad cliente que permite id públicas (nombre públicos como nombres de persona o alias) para comunicarse de forma segura. Un cliente puede tener varios de estas.
* MSID (Maidsafe Share ID) - La identidad cliente para gestionar grupos de MPID que comparten datos privados (structurados y no estructurados). Un cliente puede tener varios de estas. Este tipo de identidad no tiene soporte NAE por motivos de seguridad.

### Vault "Personas"
Las "personas" empleadas por un Vault de MaidSafe entran en dos categorias distintas, a saber, Data Management y Node Management. Estas categorias define las acciones, la agrupación de los nodos y su conocimiento de los alrededores y mensajes.
* Data Management nodes son responsables de datos NAE específicos, por lo general punteros a los datos.
* Node Management "Personas" son responsables por una entidad "Persona" (nodo) y gestiona las acciones y peticiones de dicha entidad.

Los mensajes entrantes son [demultiplexados](https://github.com/maidsafe/MaidSafe-Vault/blob/master/src/maidsafe/vault/demultiplexer.h) para identificar a la persona y el tipo de dato que se envía. Los mensajes son dirigidos a esa persona.

# Gestión de datos Generalizado
___
###Datos no versionados

Almacenar un trozo de datos en la red requiere de la coordinación y la colaboración de varias Personas. Un chunk de datos a partir de datos de usuario se crea en MaidNode y es administrado por DataManagers. DataManagers asegura que varias copias de los datos se almacenan y están disponible en diferentes PmidNodes todo el tiempo. El siguiente diagrama muestra un flujo muy básico del proceso de almacenamiento de datos en la red.

![UnversionedData](./img/UnversionedData.png)


| Data Management| Node Management   | Nodes     |
| ---------------|:-----------------:| ---------:|
| [**DataManager**](#datamanager)    | [**MaidManager**](#MaidManager)| [_MaidNode_](#maidnode)  |
| [**VersionManager**](#versionmanager) | [**PmidManager**](#PmidManager)| [_PmidNode_](#pmidnode)  |

####`Put<Data>`
|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ |

####`Put<Unique>` _(a specialisation)_
|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]=>>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ |

####`Delete<Data>`
|  _MaidNode_ [So]=>>>> |[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>> * 4| [Ac, Fw] **PmidManager** [So, Sy] =>|[Ac, Fw]_PmidNode_ |

####`Get<Data>`
|  _MaidNode_ [So]=>>>> | [Ac, Fw] **DataManager** [So]=> * (# of live PmidNodes)|[Ac, Fw] _PmidNode_ |

##Anatomía de un gestor "Persona" con datos no versionados
Todos las "Personas" siguen un patrón muy similar, acumulan, firewall, sujetan y envian los mensajes y sincronizan sus resultados, con lo cual el acuerdo que registran es el resultado de una operación en una base de datos. Los resultados o valores almacenados por operación difieren según las diferentes "Personas", pero poco más.

En la práctica, se supone, a priori, que los nodos de la red son indignos de confianza, sin embargo, la cooperación/colaboración de los nodos son esenciales para la estabilidad y la salud de la red. Para asegurar que se cumplan las dos condiciones, al recibir de un INAE, el grupo de nodos, bajo el disfraz de una "Persona" específica, que rodea el elemento objetivo se convierten en responsable de la validación de la solicitud. La sincronización entre el grupo permite a cada uno acumular la de otro dando como resultado un número predeterminado suficiente para satisfacer la solicitud. El proceso, inherentemente, detecta la manipulación del mensaje o los aspirantes a hackers y, con un mecanismo de clasificación, los delincuentes son forzados al aislamiento de la red.

Cuando se recibe desde un DNAE cada "Persona" se acumula en un solo mensaje. Esto es posible ya que existe una conexión entre los nodos, y tanto el enrutamiento como el RUDP han confirmado la identidad de otros nodos de una manera criptográficamente segura.

La variedad de mensajes de "no datos" se gestionan de manera diferente por cada "Persona".

___

###Datos versionados
Almacenar un trozo de datos versionado en la red, como una versión del directorio, es gestionada por diferentes "Personas" basados en la configuración de privacidad del directorio definido por el usuario. Las configuraciones de privacidad incluyen privadas, públicas y datos versionados compartidos. Diferentes identidades se emplean en base a la configuración de privacidad asociados a los directorios. Aunque se crea diferentes versiones de datos y se envía por diferentes "Personas", todo ello se gestiona y se almacena en el VersionManager.
El siguiente diagrama muestra un flujo muy básico del proceso de almacenamiento de datos versionados en la red, sobre la base de diferentes configuraciones de privacidad.

![VersionedData](./img/VersionedData.png)


####`PutVersion<Data>`
`Put <Data>`|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ | +
* |  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw, Sy, Uf] **VersionManager** |


####`GetVersion<Data>`
* |  _MaidNode_ [So]=>>>> |  [Ac, Fw] **VersionManager** | +
* `Get<Data>` |  _MaidNode_ [So]=>>>> | [Ac, Fw] **DataManager** [So]=> * (# of live PmidNodes)|[Ac, Fw] _PmidNode_ |


#####`DeleteVersionTillFork<Data>`
* `Delete<Data>` |  _MaidNode_ [So]=>>>> |[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>> * 4| [Ac, Fw] **PmidManager** [So, Sy] =>|[Ac, Fw]_PmidNode_ | +
* |  _MaidNode_ [So]=>>>> | [Ac, Fw, Sy, Uf] **VersionManager** |

##Anatomía de un gestor "Persona" con datos versionados

Todos estos VM "personas" deben estar de acuerdo sobre la consulta en el momento que se recibe. Para lograr esto, los VM "Personas" mantienen la acumulación de los mensajes recibidos de las "Personas" anteriores. Una vez que se alcanza el límite de espera, la petición se sincroniza con otros VM "Personas" en el grupo. Cualquier VM "Persona" que recibe el número esperado de mensajes de sincronización intenta realizar la solicitud. Al realizar la solicitud, continuará devolviendo el resultado de la acción al acumulador y llamando a los funtores de contestación asociados a las peticiones de los personajes anteriores. Una vez más, un `GET` simplemente intenta devolver la respuesta autocalculada de velocidad, pero todas las llamadas mutadas requieren de un acuerdo con todos los personajes VersionManager involucrados en la llamada. Esta es una diferencia fundamental con los administradores de datos no versionados. Esta diferencia se debe a estas "Personas" son la última llamada a la solicitud, y la sincronización se utiliza para el acuerdo en lugar de auto-calcular la respuesta y dejarlo al acuerdo del solicitante.


## Gestión de comunicación generalizada
| Node Management      | Node      |
| ----------------------|----------:|
| [**MpidManager**](#mpidmanager)    | [_MpidNode_](#mpidnode)  |



---
### "Personas" Data Management
Data management "Personas" son responsables de mantener vínculos entre claves NAE y las direcciones NAE que contiene el contenido de la clave en cuestión. Estos Vault "Personas" pueden ser considerados como responsables de los punteros a los datos, así como de la duración de los datos y la gestión de la replicación, con capacidad para resistir la rotación de la red.

###Data Manager<a id="datamanager"></a>
The **DataManager** "Persona" gestiona punteros a datos no versionados, como contenido de ficheros, claves de red como las definidas en [passport](https://github.com/maidsafe/MaidSafe-Passport/wiki) y cualquier otro elemento de datos estático.

### Estructura de Contenedor
Usa un único ManagerDb

* Key : data key + type
* Value : DataManagerValue object (serialised)

### Mensajes entrantes
* `Put<Data>` Este mensaje es recibido por un grupo de MaidManager (de K) y se acumula. Se comprueba la base de datos para ver si esa clave existe. Si el registro no existe, entonces cada uno de los Data Managers de este grupo, cercanos al NAE que se quiere guardar, selecciona un nodo conectado como PmidNode para guardarlo (si el mensaje tiene una pista del Data Holder entonces el más cercano de los gestores de datos de este NAE intenta almacenarlo allí). Para guardar un chunk, un mensaje `PUT<Data>`(key, content) se manda al PmidManagers responsable del PmidNode seleccionado.
* * | **MaidManager** ->>>> | [Ac, Fw]**DataManager**[So, Sy]->>>>|

* `Delete<Data>` este mensaje es recibido por un grupo de MaidManagers group (de K) y se acumula. Se comprueba la base de datos para ver si esa clave existe. Si existe, el contador decrece. Cuando el contador llega a cero (si alguna vez lo hace) entonces un Delete<data>(key) es enviado a todos los PmidManagers para borrar los datos.
* * | **MaidManager** ->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>>|

* `Get<Data>` este mensaje no se acumula, solo se envía. Este mensaje puede venir de cualquier "Persona", aunque no debe llegar a este personaje muy frecuentemente debido al almacenamiento en caché. Si este mensaje no llegue aquí entonces el DataManager envía un GET <Datos> directamente a un PmidNode y recupera los datos, enviando de nuevo a través de la reply_functor enviada por [routing](https://github.com/maidsafe/MaidSafe-Routing/wiki).
* * |  _MaidNode_ ->>>> | [Ac, Fw] **DataManager** | (firewall)
* `Node Status Change`
| **PmidManager** ->|[Ac, Fw] **DataManager** [So, Sy]->>>> |


### Mensajes salientes
* `Put<Data>` Cuando un elemento de datos se almacena por primera vez o una DataManager requiere añadir otro PmidNode para almacenar datos (debido a que el PmidNodes falla, se desconecta o pierde datos de otra manera) se realiza esta llamada NFS para almacenar los datos.
* * | **DataManager** [So, Sy, Uf]->>>> | **PmidManager** |

* `Delete<Data>` Cuando el recuento de suscripción se aproxima a cero o la PmidNode se considera que deja de ser responsable de un elemento de datos, se hace esta llamada NFS.
* * | **DataManager** [So, Sy, Uf]->>>> | **PmidManager** ->|

* `Get<Data>` En respuesta a recibir un Get<data> este nodo intentará recuperar los datos desde cualquier PmidNode vivo y enviará los datos al solicitante a través del Routing reply_functor proporcionada con la solicitud.
* * | **DataManager** => * live PmidNodes| _PmidNode_ |

### Chequeo de integridad de datos
En respuesta a un evento de rotación, (detectado como un Node Status Change) el DataManager creará un trozo aleatorio de datos y lo enviará a cada PmidNode con una solicitud para añadir estos datos y hacer un hash de los nuevos datos y responder con el nuevo valor. Esto permite que el DataManager evalue que todos los PmidNodes tienen los mismos datos (no dañados o perdidos).
* * | **DataManager** [So, Sr]=>>>> * 4| **PmidManager** ->| _PmidNode_ |


### Version Manager<a id="versionmanager"></a>
The **VersionManager**, gestiona datos versionados. Esto, actualmente, incluye datos que puede ser definidos en [StructuredDataVersions](https://github.com/maidsafe/MaidSafe-Private/blob/master/include/maidsafe/data_types/structured_data_versions.h). Directorios de datos privados, directorios privados y directorios compartidos públicos se manejan actualmente en esta estructura. Esta "Persona" no se limita a las versiones requeridas y por consiguiente se puede utilizar para datos estructurados de muchos tipos con facilidad.

### Estructura del contenedor

Usa un único ManagerDb

* Key : data key + type + Entity ID
* Value : Objeto StructuredDataVersions (serializado)

### Mensajes entrantes
* `PutVersion<Data>`
* * | **MaidManager** =>>>> | [Ac, Fw] **VersionManager** [Sy, Uf] |
* `DeleteBranchTillFork<Data>`
* * | **MaidManager** =>>>> | [Ac, Fw] **VersionManager** [Sy, Uf] |
* `Get<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **VersionManager** |
* `GetBranch<Data>`
* * |  _MaidNode_ =>>>> |[Ac, Fw] **VersionManager** |

### Mensajes salientes
 [None] Todas las entradas son devoluciones de llamada

### Node Management "Personas"
Node management "Personas", gestiona entidades de "Personas" (nodos).

### Maid Manager<a id="MaidManager"></a>
La función **MaidManager** gestiona el MaidNode. Esto significa asegurarse que el cliente tiene una cuenta y al menos un PmidNode registrado.

### Estructura del contenedor
Usa una AccountDb

* Key : Hash of the data key (hashed to protect clients data)
* Value : Number of copies (int32) : total cost (int32)

### Estructura de cabecera

* Tamaño total de datos almacenados
* espacio disponible

### Mensajes entrantes
* `PUT<Data>` Un cliente puede almacenar un paquete MAID anónimo en la red que crea la cuenta. Entonces el MaidNode debe registrar un PmidNode y lo hace con un paquete de inscripción del Vault, que está firmado por el MAID con las claves privadas PMID (las cuales el cliente debe tener acceso). El MaidManager individual consultará el grupo PmidManager en el arranque, en un evento de rotación y cuando el cliente está bajo de recursos y no tiene suficiente espacio que garantizar al PmidNode. En caso de éxito, la MaidNode puede enviar los datos a la DataManager relevante y recuperar el costo a pagar. En cuanto se page el costo, la intención es que la entrada en la base de datos se sincronice con los otros MaidManagers. El MergePolicy en este caso se sumará la clave de la base de datos y/o incrementará el número de copias y el campo de coste total.

* * |  _MaidNode_ ->>>> | [Ac, Fw]**MaidManager** [So]|

* `Delete<Data>` El MaidManager buscará en el registro de cuentas del cliente MAID un PUT correspondiente de la clave de datos. En caso de éxito el MaidManager reducirá el número de trozos PUT de esa clave. Si varios trozos se almacenan en la misma clave el costo se reduce tomando un promedio del costo total pagado por esa clave. La eliminación siempre devuelve un mensaje de éxito por lo que el cliente sólo desperdicia su propio tiempo en tratar un ataque de esa naturaleza.
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `PutVersion<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `DeleteBranchTillFork<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `Register/Unregister vault`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |

### Mensajes salientes
* `PUT<Data>`
* * | **MaidManager** [So] ->>>> | **DataManager** |
* `Delete<Data>`
* * | **MaidManager** [So] ->>>> | **DataManager** |
* `PutVersion<Data>`
* * | **MaidManager** [So] =>>>> | **VersionManager** |
* `DeleteBranchTillFork<Data>`
* * | **MaidManager** [So] =>>>> | **VersionManager** |
* `GetPMIDHealth`
* * | **MaidManager** [So] =>>>> | **PmidNode** |

### Pmid Manager<a id="PmidManager"></a>
La función PAH (Pmid Account Holder) gestiona al Pmid client. Esto significa asegurar que el cliente tiene una cuenta y un registro exacto de los elementos de datos realizada y ninguna pérdida (versión inicial de rango).

### Estructura del contenedor

Una una AccountDb

* Key : Data key
* Value : Size (int32)

### Estructura de cabecera

* Tamaño total de datos almacenados
* Tamaño total de datos perdidos
* Espacio disponible


### Mensajes entrantes

* `Put<Data>` Guarda datos en PmidNode y actualiza el contador de almacenaje para ese PmidNode
* * | **DataManager** =>>>> | [Ac, Fw] **PmidManager** |
* `Delete<Data>` Manda un borrado a PmidNode and elimina el contador de almacenaje de ese Pmidnode
* * | **DataManager** ->>>> | [Ac, Fw] **PmidManager** |
* `Get<Data>` Devuelve dato o error
* * | **DataManager** =>>>> | [Ac(1), Fw] **PmidManager** |
* `Lose<Data>` Enviar un borrado a PmidNode y añade uno al contador de perdida
* `GetPMIDHealth`
* * | **MaidManager** =>>>> | [ **PmidManager** | (no Fw or Ac, devuelve cada llamada)
* 
### Mensajes salientes

* `SendAccount` Se envía a _PmidNode_ cuando se une al grupo. Esto se detectará por el **PmidManager** a la recepción de un evento de rotación. 
La cuenta en este caso son todos los datos que _PmidNode_ debe haber guardado y no incluirá ningún dato borrado o perdido que ocurra mientras el _PmidNode_ esté off line.
* *   | **PmidManager** [So]-> | **PmidNode**


### Mpid Manager<a id="mpidmanager"></a>

La función MPAH (Mpid Account Holder) gestiona el cliente Mpid. Eso significa asegurar que que cliente tiene una cuenta y listas blancas o negras si son requeridas. Esta "Persona" tambien guarda mensajes destinados a un cliente Mpid que esté off-line.

### Estructura del contenedor
Usa la AccountDb

* Key : MpidName + SenderMpidName + messageID
* Value : message contents

Esta base de datos solo debe guardar mensajes mientras _MpidNode_ esté off-line.

### Estructura de cabecera

* Lista blanca
* Lista negra


### Mensajes entrantes

* `Message`
* * | _MpidNode_ =>>>> | this**MpidManager** [So]|
* * | **MpidManager** =>>>> | this**MpidManager** [So]|
* `AddToWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `WhiteList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(reply with whitelist)
* `AddToBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `BlackList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(reply with blacklist)

### Mensajes salientes
* `Message` A la recepción de un mensaje la lista es chequeada y el mensaje enviado. Si el nodo está off-line entonces el mensaje se guarda localmente, cuando se recibe un evento de rotación la lista off-line se chequea y se envía al cliente.
* * | this**MpidManager** [So] -> | _MpidNode_ | (puede venir de la base de datos)
* * | this**MpidManager** [So] =>>>> | **MpidManager** [Sy]| (mensaje actual de un _MpidNode_)

## "Personas" Entity
### Pmid Node<a id="pmidnode"></a>

### Mensajes entrantes

* `Put<Data>`
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**
* `Delete<Data>`
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**
* `Get<Data>`
* *   | **DataManager** => |[Ac, Fw] **PmidNode**
* `SendAccount` Esto se envía a _PmidNode_ cuando se une al grupo. Esto se detecta por el **PmidManager** a la recepción de un evento de rotación. La cuenta, en este caso, son todos los datos que _PmidNode_ debe haber guardado y no incluirán ningún dato borrado o perdido ocurrido mientras _PmidNode_ estaba off line.
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**


### Mensajes salientes

[none]

### Maid Node <a id="maidnode"></a>

### Mensajes entrantes

[none]

### Mensajes salientes

* `Put<Data>` Devuelve error_code y PmidHealth
* * |  _MaidNode_ [So]=>>>>| **MaidManager** |
* `Delete<Data>` Returns error_code
* * |  _MaidNode_ [So]=>>>>| **MaidManager** |
* `Get<Data>` Returns error_code and data (if success)
* * |  _MaidNode_ [So]=>>>>| **DataManager** |


### Mpid Node<a id="mpidnode"></a>

### Mensajes entrantes

* `Message`
* * | _MpidNode_ =>>>> | this**MpidManager** [So]|
* * | **MpidManager** =>>>> | this**MpidManager** [So]|
* `AddToWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `WhiteList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(devuelve la lista blanca)
* `AddToBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `BlackList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(devuelve la lista negra)

### Mensakes salientes
[none]

# <a id="sync"></a>Sincronización

La red está en constante flujo, con nodos apareciendo y desapareciendo continuamente. Esto fuerza a un mecanismo de cambio que pueda reconocer que nodos son responsable de que en tiempo real y reconocer eventos de rotación cerca de un nodo. Usando [MaidSafe-Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) la red puede detectar cambios en los nodos en un pequeño espacio de tiempo. El Routing tambien provee de un mecanismo que garantiza con gran precisión que nodos son responsables de que NAE (incluyendo el nodo peticionario de la red).

Para resolver un elemento de datos, ya sea un `transfer` de cuenta o `action` sin resolver, el nodo requiere de un acuerdo de una mayoría de nodos cercanos al elemento. Como puede haber varios eventos de rotación durante una transferencia de estos datos, se requiere que el nodo maneje, por si mismo, estos eventos multiples de rotación. Para hacerlo, con cada evento de rotación, el nuevo nodo y los nodos antiguos verifican, mediante la comprobación, si el nodo antiguo habría dado un elemento de datos y reemplazan este node_id con la nueva ID del nodo. Si el nuevo nodo no contiene el elemento de datos pero debería (es decir, el nuevo nodo habría sido responsable de un elemento sin resolver), entonces el nuevo node_id se añade como si hubiera enviado los datos. Para hacer esto un poco más simple, todos los datos sincronizan al nodo añadido como habiendo "visto" el elemento actual, esto ayuda a conseguir el número de mayoría requerida. Cuando un nodo se agrega a los datos no resueltos de esta manera, se agrega al final del contenedor. Esto evitará que el nodo envíe este elemento en un mensaje de sincronización.

Cuando un nodo envía un mensaje de sincronización, envía un identificador único y su propia ID junto con los datos sin resolver. Cada nodo de recepción crea un contenedor de la node_id y entry_id junto con los datos sin resolver. Cuando el tamaño de este recipiente es 0,5 * tamaño de los nodos cercanos +1, entonces los datos están resueltos y se escriben en el almacén de datos.

Hay dos tipos de datos que requieren datos acumulativos (de un grupo) para resolver:

* `synchronise` Estos son datos que todavía no se ha sincronizado (sin resolver), pero este nodo se ha "visto" a sí mismo. Estos datos contienen la acción requerida para ser tomado con los atributos para tomar esa acción, esto puede ser pensado como una función (tipo de mensaje) y los parámetros que se aplicarán.
* `account transfer`, este tipo de datos ha sido resuelto y almacenada por el nodo. Se envía como el valor de base de datos para ser almacenado.

##Sincronizar para validar acciones

El sincronizar se utiliza para asegurar que todos los nodos cercanos del mismo grupo responsable de una NAE llegan a un acuerdo sobre los datos relativos a este NAE antes de guardar (o realizar) estos datos. Los nodos logran esto mediante la creación de un contenedor de datos a resolver. Para esos datos una entrada sin resolver se puede añadir al contenedor cuando
* el mensaje se recibe de la "Persona" previa
* una entrada sin resolver se recibe de una "Persona similar" (en un grupo) para el que un nodo es responsable objetivo

Las entradas no resueltas, asociados con los datos, se resuelven una vez que el número de entradas no resueltos recibidas por ese dato (de personajes anteriores y similares) alcanza un número esperado. Un nodo que haya resuelto una entrada podrán participar en el envío a otro(s) nodo(s) y considerado responsable de los datos, SÓLO si el nodo ha recibido los datos de una persona anterior.

Usando el routing para confirmar la responsabilidad a un **manager node** comprobará todos los datos mantenidos para asegurarse de que sigue siendo responsable de los datos (via el API routing). El nodo comprueba entonces los datos que el nuevo nodo debe tener (si tiene alguno). El dato se envía a un nuevo nodo como `unresolved data`.


Este proceso continúa y cada vez que se envían los datos se incrementa un contador, cuando este contador llega a un parámetro (inicialmente 100) los datos se considera no resoluble y se retira. Cuando los datos no se resuelven no se eliminan de la lista ya que podría haber más nodos que se añaden al componente contenedor de pares (resolvemos con una mayoría y esperamos un número unánime los nodos que se agregarán y no queremos crear una nueva entrada sin resolver). Cuando hay un conjunto completo de compañeros que han enviado los datos (acuerdo unánime) a continuación, se elimina la entrada sin resolver.

### Transferencia de cuentas

Managers "personas" almacenan la información para los nodos/datos y normalmente entregan su funcionalidad basada en la acumulación en la toma de decisiones, donde un grupo de nodos cercanos cooperan para realizar la funcionalidad deseada. Obviamente, las decisiones correctas, en gran medida, dependen de lo bien que se sincronizan los miembros del grupo. La naturaleza dinámica de la red, donde un nodo de un grupo puede abandonar o unirse, requiere la implementación de mecanismos eficientes para asegurar que cada miembro de cualquier grupo se sincroniza con otros miembros del grupo y mantiene la información actualizada para cualquier nodo/datos del que es responsable.

Cuando un nuevo nodo entra en un grupo, envía un mensajes de transferencia de cuenta, su propia dirección se agrega a un registro de transferencia de la cuenta sin resolver (es el grupo cercano -1 nodo) para asegurar que el calculo del tamaño del grupo no cambian aunque el tamaño del grupo no sea igual el tamaño del grupo del sistema cuando este nodo sea reemplazando por un nodo ahora ausente de ese grupo.

El registro de transferencia la cuenta es una entrada de una base de datos, requiere de un acuerdo para luego escribir directamente en la base de datos.

Además, se proporcionan datos sin resolver a este nodo y otra vez este nodo añade su propia Id a estos datos, sin embargo, no se sincroniza estos datos de nuevo ya que los nodos del grupo cercano ya se han ocupado de esto, ya que recogen este nuevo nodo en el evento de rotación. Los datos no resueltos ahora se deben resolver de acuerdo con el proceso anterior.


### Acumulador

Cuando se reciben mensajes de un grupo de nodos, la red requiere autorización para que el mensaje sea validado. Esto se hace mediante la acumulación de mensajes a medida que llegan después de comprobar que proceden de un grupo que se encarga de enviar los datos (validado a través de la API de enrutamiento) y la acumulación de los mensajes hasta que recibamos al menos grupo cercano -1 mensajes si el mensaje proviene de un grupo **manager node**. Los mensajes que vienen de nodos DNAE se acumulan en ese único mensaje. Estos nodos son verificables ya que están conectados y se han llevado a cabo una acción para validar que están autorizados a enviar ese mensaje.

### Firewall

Cuando un mensaje se acumula se añade al firewall. Este firewall comprobará todos los mensajes entrantes y responderá con el actual error_code (y el mensaje si es requerido por ese tipo de mensajes). El error_code puede ser `synchronising` en los casos en que el nodo ha acumulado suficientes mensajes para asegurarse de que son válidos, pero no puede tomar una decisión todavía sobre la" respuesta "al mensaje (que puede requerir sincronizar o incluso enviar un mensaje a otro persona para recuperar más información).

### Almacenamiento en Caché

El almacenamiento en caché es una faceta muy importante de las redes distribuidas. Los datos de la caché, probablemente, se almacenen en la memoria en lugar de disco. Actualmente hay definidos, en los Vaults, dos mecanismos de mantenimiento de caché. Las siguientes secciones presentan una discusión general, para la descripción detallada de aplicación, por favor, eche un vistazo a [Caching](https://github.com/maidsafe/MaidSafe-Vault/wiki/Caching) .

### Almacenamiento en Caché oportunista

El almacenamiento en Caché oportunista es siempre un mecanismo FIFO (primero entra, primero sale). En respuesta a un `Get` cada nodo en la cadena añadirá cualquier dato cacheable a su FIFO (como autocomprobación o inmutable). Esto tiene muchas ventajas:

* Múltiples solicitudes de los mismos datos se acelerarán automáticamente.
* Se hacen dificil los ataques de denegación de servicio, ya que los datos simplemente rodean al solicitante de datos si se le pregunta continuamente.
* Los datos importantes van más rápido cuando más importante y menos cuando menos, haciendo un buen uso de los recursos de red.
* Nodos cercanos pueden convertirse en guardianes temporales de datos muy fácilmente, lo que permite muchas ventajas a los diseñadores de sistemas.
* Por ejemplo, un sitio web público acelerará su velocidad en relación al numero de usuarios, se trata de un enfoque más lógico que los sitios web de hoy en día que puede correr el riesgo de sobrecarga y cuelgue.

### Almacenamiento en Caché determinista

Este tipo de caché no está todavia implementado.

El almacenamiento en caché determinista permite que la red expanda, de manera efectiva, el número de nodos que mantienen datos y punteros a los datos cuando se accede a estos datos por un gran número de nodos o usuarios. Esto permitría a la red hacer frente a los datos extremadamente populares como los que pueden representar la tecnología de microblogging con cuentas extremadamente populares.


Además de este tipo de almacenamiento en caché, la red permite las listas de suscripción, donde las identidades pueden inscribirse para recibir una alerta cuando ocurre un cambio que desean seguir, como un sitio de microblogging o un sitio web de noticias, etc.

### Políticas NFS

El API a la red del Vault se hace via Nfs cuyas políticas se pueden encontar en [nfs library](https://github.com/maidsafe/MaidSafe-Network-Filesystem).

### Ranking

Los nodos con mal comportamiento tienen que ser reconocido por la red y actuar sobre ellos, de la misma manera aquellos serviciales y con buen comportamiento tambien deben tener un mayor respeto de los otros nodos. Para lograr esto se emplea un mecanismo de ranking. Este mecanismo de ranking, de los Vaults de red, maneja los nodos que pierden datos u otra forma de corrupción. Esto puede ser por muchas razones, como:

* Pobre ancho de banda
* Cpu lenta
* Disponibilidad infrecuente (la máquina está apagada la mayor parte del tiempo por alguna razón)
* Corrupción del disco duro
* Disco duro ha sido manipuladoh (el usuario ha borrado el sistema de datos)
* Y muchos más

En lugar de medir cada elemento, lo que sería muy caro y complejo, la red mide la cantidad de datos que un nodo puede almacenar y compara esto con los datos que pierde. Estas cifras juntas permiten calcular el espacio disponible y su solvencia. Una alta tasa de perdida de almacenamiento hace que el valor del Vault disminuya.
