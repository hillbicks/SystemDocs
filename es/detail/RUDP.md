## Descripción general librería RUDP

RUDP (Reliable UDP) implementa pseudo-conexiones que utilizan UDP para lograr muchos de los beneficios de un protocolo de conexión como TCP, pero permitiendo algo crucial como permitir [NAT traversal](https://en.wikipedia.org/wiki/Network_address_translation#Type_of_NAT_and_NAT_traversal.2C_role_of_port_preservation_for_TCP), algo que TCP no puede hacer. Además, todos los datos se cifran punto a punto utilizando un mecanismo seguro y verificable de intercambio de clave pública RSA. Esto forma parte del [PKI](http://en.wikipedia.org/wiki/Public-key_infrastructure) proporcionado por la red SAFE.

La interfaz de la librería está disponible en los siguientes archivos:

* [managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h) - es el API principal y se analiza en más detalle a continuación
* [parameters.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/parameters.h) - proporciona las variables de configuración de la librería
* [return_codes.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/return_codes.h) - En el futuro, el uso de códigos de retorno será reemplazado por los mecanismos de control de errores previstos en el [MaidSafe-Common](https://github.com/maidsafe/MaidSafe-Common/wiki).
* [nat_type.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/nat_type.h) - Una enumeración de tipos relevantes [NAT](https://en.wikipedia.org/wiki/Network_address_translation) que la librería necesita identificar

La librería hace un uso intensivo de [Boost.Asio](http://www.boost.org/doc/libs/1_55_0/doc/html/boost_asio.html), tanto para las operaciones relacionadas con la red como para las operaciones asíncronas. También depende de las librerías MaidSafe [Common](https://github.com/maidsafe/MaidSafe-Common/wiki), [Private](https://github.com/maidsafe/MaidSafe-Vault-Manager/wiki), y [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki).


### Antecedentes

Dos nodos RUDP mantienen una pseudo-conexión o "conexión administrada" mediante el envío de pequeños mensajes de control continuamente entre sí. La conexión puede ser finalizada por cualquiera de los pares mediante el envío de un mensaje de control de apagado, por lo tanto, en circunstancias normales los pares son conscientes rápidamente de una conexión apagada. En caso que un número fijo de intentos de conexion queden sin respuesta, el nodo considera que la conexión está muerta. Este es un mecanismo más lento que usando un mensaje de apagado, sin embargo, debería ser mucho menos común, ya que suele ser causado, por ejemplo, por un cierre inesperado o por una caida de red.

Para cada conexión, un nodo tiene un `EndpointPair` asociado a sí mismo y otro para el par. El `EndpointPair` contiene el punto final "local" (IP/Puerto tal y como se ve en el interior o detrás del router) y el punto final "externo" (IP/Puerto tal y como se ve fuera del router). Se prefieren los puntos finales externos, los internos se utilizan solo en caso de fallar el del exterior y en caso que ambos pares estén detras de un router que no permite [hairpinning] (http://en.wikipedia.org/wiki/Hairpinning).

No puede establecerse una comunicación entre dos pares si ambos están detras de routers que proporcionan [NAT simétrico](https://en.wikipedia.org/wiki/Network_address_translation#Methods_of_port_translation). La librería [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) Maidsafe gestiona este caso proporcionando un nodo proxy para cada uno de esos nodos "ocultos", y, como tal, la biblioteca RUDP no trata de resolver este problema.


### Detalles

[managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h) define tres funtores; `MessageReceivedFunctor`, `ConnectionLostFunctor`, and `MessageSentFunctor`.  Los primeros dos deben ser propoporcionados por el funtor `ManagedConnections::Bootstrap`, pueden ser llamados varias veces por el RUDP y es autoexplicativo.

El `MessageSentFunctor` será invocado exactamente una vez por llamada a `ManagedConnections::Send`. Indica que el mensaje asociado ha sido recibido por el par de destino, (no sólo indica que está en cola para el envío, sino recibido realmente), o bien ha fracasado.  Se prevee (aunque no se requiere) que una instancia separada del `MessageSentFunctor` se enviará en cada llamada a `Send`.

Internamente, una clase `ManagedConnections` mantiene varios (hasta `Parameters::max_transports`) objeto transporte, cada uno con su propio socket de red.  Esta socket se utiliza para mantener varios, (actualmente hasta 50) (ver [transport.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/src/maidsafe/rudp/transport.h)), `Transport::kMaxConnections`) psuedo-conexiones.  Por tanto, es probable que tener muy pocos objetos `ManagedConnections` proporcionará un rendimiento óptimo.

Bootstrapping un objeto `ManagedConnections` puede ser un proceso lento, ya que las conexiones se intentan con diversos candidatos a punto final hasta que uno tiene éxito.

`ManagedConnections::kResiliencePort()` proporciona un puerto de red, ampliamente conocido, que se puede utilizar en una situación de recuperación de desastres.  Cada instancia `ManagedConnections` intenta abrir este puerto local. Después de la segmentación de red, por ejemplo, los nodos pueden tratar de volver a reunirse intentando conectarse a otros nodos locales, o, para los nodos de conexión directa, utilizando este puerto conocido.

Más detalles de las funciones individuales se pueden encontrar en el propio fichero [managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h).




