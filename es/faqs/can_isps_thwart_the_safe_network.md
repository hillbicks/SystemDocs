### ¿Pueden los ISP frustar la red SAFE?

La red utiliza cargás útiles de [UDP](http://en.wikipedia.org/wiki/User_Datagram_Protocol) encriptado para mover los datos a través de la red. Así que un contenido UDP no legible es difícil de distinguir de un tráfico regular de Internet. Esto hará que sea extremadamente difícil, para los ISP, detectar el tráfico de red SAFE y por lo tanto detenerlo. Además, todos los nodos se conectan a puertos diferentes al azar, por lo que el filtrado de puertos (una forma de control de acceso) no funcionará.

###¿Hay alguna manera de comprometer o interrumpir MaidSafe, como la creación de un gran número de ordenadores con velocidades de carga muy bajas pero enormes discos duros?

El número de ordenadores tendría que superar masivamente el de los nodos de la red. Cuando un nodo se une, su ID se ve alterada por los nodos existentes y se coloca en una ubicación aleatoria. Se tendría que añadir un importante número de nodos, y ser extremadamente afortunado, para conseguir que 4 estén cerca, el uno del otro, en el espacio de direcciones. Incluso entonces, el daño estará limitado a un único trozo de dicho grupo.

###¿Cómo trata la red, con la redundancia de datos, para asegurar que todos los datos que se comparte permanecen accesibles?

Cada archivo está cifrado y dividido en trozos durante nuestro proceso de autoencriptación (Self Encryption). La red controla y mantiene 4 copias de cada trozo encriptado y mueve estos fragmentos por la red cuando los nodos dejan de estar disponibles, ya sea a través de un fallo o de un apagado. Con el fin de hacer frente a esta rotación, la red es capaz de reconfigurarse globalmente de forma extremadamente rápida (tan rápido como 20 milisegundos). Estos pasos aseguran que los datos están siempre disponible mientras, el usuario que desee recuperar los datos, tenga una conexión a Internet y las credenciales correctas.

###¿Cómo se gestiona con la latencia inherente a una red distribuida como SAFE?

Varias cosas facilitan aquí la solución. En primer lugar, no solo una persona almacena un fragmento de datos ya que, en cada momento, se mantienen múltiples copias del trozo con un mínimo de 4. Además, la red también clasifica los nodos, por ejemplo, si un nodo Vault falla a la hora de recuperar un trozo de datos, a continuación, un nuevo nodo lo reemplaza como titular y el nodo original baja de rango. Si el problema persiste, este nodo será clasificado a un punto en el que no se le pedirá almacenar más datos. Así la red determina qué nodo es eficaz para el almacenamiento de datos. Por último, si un trozo de información que se solicita es muy popular y se recupera a menudo, se almacena en caché por los nodos, por lo que los tiempos de recuperación disminuyen. Todo esto asegurará que los datos populares se recuperarán más rápidamente.

###¿Por qué una red distribuida sería más seguro que un servidor tradicional utilizando el mismo cifrado que se usa dentro de SAFE?

Aquí es donde la seguridad física juega un papel importante. En primer lugar, usar un servidor tradicional proporciona un control completo, de un archivo, a una entidad que es capaz de descifrar y leer su contenido o borrar sus datos. Por el contrario, la descentralización difunde los datos en múltiples fuentes para cada archivo. Además, las acciones de la red se realizan basadose en el consenso del grupo, por lo que un nodo sólo, aunque quiera, no puede completar ninguna tarea. Sólo por acuerdo mayoritario del grupo se puede realizar las tareas. Esto hace que SAFE sea extremadamente robusta ya que, la posibilidad de conseguir dominar un grupo cercano de nodos en esta red, es extremadamente improbable.









