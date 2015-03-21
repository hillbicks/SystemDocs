# Infrastructura

Estas FAQs se relacionan con preguntas sobre los componentes y procesos que hacen funcionar de red.


### ¿La red SAFE requiere de una conexión continua a Internet de todos los participantes?

No, la Red SAFE clasificará cada tipo de conexión y ajustará los recursos que ofrece, con el fin de dar cuenta de su indisponibilidad (ya sea por apagando o fallo en el dispositivo).

No es un problema estar desconectado por períodos cortos de tiempo. Sin embargo, si su nodo no está disponible por períodos más largos, impactará negativamente sobre el servicio que recibe de la red.

### ¿Que significa descentralizado en la red SAFE?

La red SAFE, y en particular el enfoque que MaidSafe hace de la descentralización, es completamente descentralizado. Este termino se está usando mucho en la comunidad técnica. Para la red SAFE descentralizado significa lo siguiente:

1. Sin servidores 
2. Sin información de ninguna infraestructura de servidores (no DNS, no Servidores de tiempo...etc...)
3. Sin conexión a ninguna red basada en servidores
4. Sin estructuras de datos centralizadas en los nodos
5. Sin grupo (o grupos) controlando
6. Cero control humano en la administración del acceso a los datos
6. Totalmente inclusivo (sin posibilidad de prohibir a nadie o aplicar barreras de acceso a la red básica)
7. Red sin fronteras; no hay fronteras ni controles sobre su alcance

###¿Se limita la capacidad de almacenamiento?

Una vez la red alcance la masa crítica, la capacidad de almacenamiento de cada usuario sólo estará limitada por el número de safecoins que tienen, o que esté dispuestos a utilizar para almacenar sus datos.

Estos safecoins se pueden ganar al proporcionar recursos, o comprarlos. La Red SAFE utiliza un proceso de [deduplicación] (https://es.wikipedia.org/wiki/Deduplicaci%C3%B3n_de_datos) que permitir el uso más eficiente del espacio de almacenamiento en la red. A medida que la red se vaya constituyendo con los recursos de los usuarios, los costos de infraestructura serán una fracción de los actuales proveedores de almacenamiento en servidores.

###¿Quién paga por el almacenamiento?

Los usuarios (farmers) proporcionan sus recurso informáticos sobrantes a la red y es este recurso el que se usa para guardar los datos. Los usuarios que aporten espacio de disco para la Red SAFE son recompensados con safecoins por la red.

###¿Por cuánto tiempo se almacenan mis datos?

Todos los datos se almacenan en la Red SAFE para siempre a menos que el usuario decida eliminarlos. Los datos que han sido mantenidos durante mucho tiempo, pero sin acceso a ellos, se moverán a un archivo.

###¿Cómo se asegura que trozos de datos no se pierdan cuando un usuario se desconecta?

La Red SAFE mantiene automáticamente un mínimo de cuatro copias accesibles de cualquier trozo de datos en cualquier momento. Cuando un usuario apaga su ordenador, sus Vault managers (el grupo responsable de cuidar de los nodos de red) lo notifica a la red y todos los fragmentos de datos en poder de ese Data manager se copian en otros lugares. Este proceso ocurre muy rápidamente, en unos 20 milisegundos.

###¿Puedo descentralizar sitios como Youtube?

Sí, se puede descentralizar cualquier aplicación o servicio web que existe actualmente en Internet.


###¿Cómo se defiende la Red SAFE contra ataques Sybil?

La Red SAFE requiere que todas las solicitudes sean procesadas por al menos dos grupos de Vaults.
El cliente MaidSafe aprueba una solicitud a sus cuatro Data managers, que verifican la solicitud basándose en la firma del cliente. La solicitud se pasa entonces a un grupo seleccionado, de forma determinista, de otros cuatro Vault que también verifican la solicitud basándose en su firma.

Al seleccionar el segundo grupo de Data managers de manera determinista, este ataque ya no es válido para la red SAFE, ya que no le es posible, a un atacante, obtener el control de un Vault simplemente rodeándolo.

Para evitar esto, el atacante requeriría la capacidad de rodear Vaults específicos en la Red SAFE. Esto no se puede lograr, ya que requeriría ser capaz de generar de forma efectiva diferentes valores que, cuando se calculara su hash con SHA-512, diera valores cercanos a un punto particular.

###¿De qué manera trata la red SAFE la redundancia de datos para asegurar que todos los datos que se comparte se encuentren accesibles?

Cada archivo se encripta y divide en trozos durante nuestro proceso de autoencriptación (Self Encryption). La red guarda y conserva cuatro copias de cada trozo encriptado y copia estos fragmentos por la red cuando los nodos dejan de estar disponibles, ya sea a través de un error o un apagado. Con el fin de hacer frente a esta rotación, la red es capaz de reconfigurase a nivel mundial de forma extremadamente rápida (20 milisegundos). Los trozos se distribuyen a nivel mundial para una mayor robustez.

La Red SAFE es completamente autónoma y todas estas operaciones son gestionadas y completadas por el sistema sin nuestro conocimiento.


###¿Cómo se almacenan y recuperan los datos?

Los datos se almacenan y se recuperan mediante el proceso de autoencriptación. La autoencriptación se utiliza para mezclar y codificar los datos antes de que se envíen a la Red SAFE. Este proceso es automático y ocurre instantáneamente. En cuanto los datos se guardan en disco duro virtual de un usuario, se dividen en al menos tres trozos. Se crea un mapa de datos y para cada trozo se crea un hash, (una huella dactilar digital) y se escribe en el mapa de datos. Para mayor seguridad el mapa de datos también pasa por este proceso de autoencriptación.

Cada fragmento se cifra para crear datos aleatorios, no repetidos. Por último, y en combinación con los hashes originales, los trozos son vueltos a encriptar. La salida de cada trozo se añade entonces al mapa de datos.

El mapa de datos, con hashes antes y después del cifrado, se utiliza para recuperar y decodificar los datos del usuario, ya que proceso de cifrado no es reversible. Los datos se descifra y se reconstituye con los datos de usuario PIN, palabras clave y contraseña.

###Cuando los archivos se dividen en trozos, la red recibe un hash (una huella digital) de cada uno. ¿Puede el hash original, llevar hasta un usuario?

En resumen, no. Los trozos no están vinculados a un usuario específico en la red y el anonimato (y la seguridad que podría decirse que son la misma cosa) está en el corazón de la red. En la red SAFE existen una serie de características, de muy alto nivel, que proporcionan total anonimato.

* RUDP (Reliable UDP) encripta cada salto del mensaje a medida que atraviesan la red
* La capa de enrutamiento borra las direcciones IP tras el primer salto
* El almacenamiento y recuperación de datos se lleva a cabo usando un identificador conocido sólo a la red que no está vinculado a una persona o nombre público
* No hay conexión al servidor y por lo tanto no hay un punto central de reconocimiento o ataque
* Las contraseñas no se almacenan o transmiten por la red
* Todos los mensajes se encriptan y el identificador del emisor/receptor no es el de inicio de sesión del usuario, el identificador se almacena dentro de un paquete encriptado


