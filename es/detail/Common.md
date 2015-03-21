#Descripción general libreria Common

La librería Maidsafe-Common proporciona muchos componentes que son ampliamente utilizados por otras librerías MaidSafe. Se recomienda encarecidamente el uso de estos componentes ya que están bien probados y evolucionarán con el tiempo para aumentar su eficiencia y eficacia.

Generalmente, los componentes pueden dividirse en los siguientes grupos:

_**Ayudas a la concurrencia**_
* [Active-Object](https://github.com/maidsafe/MaidSafe-Common/wiki/Active-Object) - Una implementación del [Active Object Design Pattern](http://en.wikipedia.org/wiki/Active_object). Útil para correr operaciones en un hilo seguro.
* [Asio-Service](https://github.com/maidsafe/MaidSafe-Common/wiki/Asio-Service) - Ayudantes para operaciones asíncronas y temporizadores basado en [Boost ASIO](http://www.boost.org/doc/libs/release/doc/html/boost_asio.html).
* [Safe-Queue](https://github.com/maidsafe/MaidSafe-Common/wiki/Safe-Queue) - Cola interna sincronizada (con cerradura) basada en [deque](http://en.cppreference.com/w/cpp/container/deque). Por favor, considere mejor la estructura [boost lock free](http://www.boost.org/doc/libs/release/doc/html/lockfree.html). Este contenedor esta considerado obsoleto.

_**Tipos seguros**_
* [Bounded-String](https://github.com/maidsafe/MaidSafe-Common/wiki/Bounded-String) - Crear tipos de string con "límites" superior e inferior
* [Tagged-Value](https://github.com/maidsafe/MaidSafe-Common/wiki/Tagged-Value) - Una implementación de [the whole value idiom](http://martin-moene.blogspot.co.uk/2012/07/light-on-whole-value.html)

_**Seguros de excepciones**_
* [Errors-Exceptions](https://github.com/maidsafe/MaidSafe-Common/wiki/Errors-Exceptions) - Extensiones del sistema de manejo de errores ```std::error_code``` y ```std::exception```
* [On-Scope-Exit](https://github.com/maidsafe/MaidSafe-Common/wiki/On-Scope-Exit) - Una utilidad muy práctica para revertir valores o realizar funciones en caso de lanzar una excepción.

_**Almacenamiento de datos**_

La sección de almacenamiento de datos implementa clases que satisfacen algunos de los requerimientos de la interface de red RESTful; principalmente Put, Get and Delete. Son esencialmente almacenamiento <key,value>, no bases de datos.

Su interfaz está disponible en los siguientes archivos:

* [permanent_store.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/permanent_store.h) - Una clase de almacenamiento persistente usando el sistema de ficheros como medio de almacenaje.
* [data_buffer.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_buffer.h) - Un buffer FIFO que utiliza una combinación de almacenamiento basado en disco y de almacenamiento basado en memoria. Está diseñado para permitir lectura/escritura rápida en memoria mientras un subproceso de fondo copia los datos en la parte del disco.
* [data_store.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_store.h) - Una clase de almacenamiento basado en directivas. Se suele usar concurrentemente junto a [DataBuffer](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_buffer.h) como directiva.
* [memory_buffer.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/memory_buffer.h) - Una clase de almacenamiento de memoria interna usando [`boost::circular_buffer`][boost_circular_buffer] para proporcionar un pequeño y rápido almacenamiento FIFO para los datos no persistentes.

_**Tipos de datos**_

La sección de los tipos de datos se utiliza para reunir los distintos tipos de datos que puede manejar la red. Algunos tipos se definen aquí, otros simplemente se enumeran. Las enumeraciones de los tipos se requiere, desafortunadamente, cuando se serializa los datos a fin de enviarlos a nodos similares en la red.

Su interfaz está disponible en los siguientes archivos:

* [data_type_values.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_type_values.h) - Una enumeración de todos los tipos de datos que pueden ser manejadas por la Red MaidSafe.
* [immutable_data.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/immutable_data.h) - Un tipo de datos cuyo contenido no puede modificarse. Su nombre se define como el hash SHA512 de su valor. Este es el principal tipo de datos como resultado de la autoencriptación de un archivo.
* [structured_data_versions.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/structured_data_versions.h) - Un tipo de datos, no puesto por si mismo directamente a la red, sino más bien como medio para manejar versiones de los datos mutables de la red. Las versiones representan punteros a trozos ImmutableData.
* [mutable_data.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/mutable_data.h) - Los tipos de datos que representan versiones seriadas. Estos se utilizan en el contexto de almacenar cambios a directorios; cada actualización se convierte en una nueva versión. Los cambios de directorio de un usuario, son serializados, y encriptados en caso necesario, y los contenidos puestos en la red como ImmutableData. El nombre de ese trozo de ImmutableData es el valor de una versión (junto con una referencia a la versión anterior). Esta versión se serializa luego en un trozo MutableData y se almacena. El nombre del directorio es una cadena aleatoria y persiste durante la duración de la vida del directorio.
* [data_name_variant.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_name_variant.h) - Una [`boost::variant`][boost_variant] variante de los nombres de los diferentes tipos que maneja la red. Esto se utiliza principalmente en los almacenes de datos, donde hay el requisito de poseer varios tipos de datos de estructura similar, pero con diferentes nombres.

_**Utilidades generales**_
* [Utils](https://github.com/maidsafe/MaidSafe-Common/wiki/Utils) - Una mezcla variada de servicios públicos, tales como las conversiones hexadecimales, generadores de números aleatorios, etc.
* [Key Value Buffer](https://github.com/maidsafe/MaidSafe-Common/wiki/Key-Value-Buffer) - Un sistema de escritura en disco con buffer y capacidad de almacenamiento en caché.
* [Node Id](https://github.com/maidsafe/MaidSafe-Common/wiki/Node-Id) - Utilidades para manejar direcciones de 512 bit como se utiliza en las bibliotecas MaidSafe.

_**Ayudas criptográficas**_
* [Crypto Utils](https://github.com/maidsafe/MaidSafe-Common/wiki/Crypto-Utils) - Cifrado simétrico, empaquetado Hash, empaquetado de contraseña segura, clave compartida N+P (basado en el algoritmo de Shamir)
* [Asymmetric Encryption](https://github.com/maidsafe/MaidSafe-Common/wiki/Asymmetric-Encryption) - Empaquetado para métodos asimétricos de cifrado (incluyendo encripción safe), actualmente soporta RSA.

_**Ayudas a la depuración**_
* [Test Runner](https://github.com/maidsafe/MaidSafe/wiki/Running-Tests)
* [Logging](https://github.com/maidsafe/MaidSafe/wiki/Logging-Options)

## Visor de red

[NetWork Viewer](http://visualiser.maidsafe.net:8080/auth) es una herramienta de interfaz de usuario presentada por MaidSafe para visualizar la ruta de la red MaidSafe. Le puede ayudar a entender cómo funciona la red y depurar algoritmos de enrutamiento.



[boost_circular_buffer]: http://www.boost.org/doc/libs/release/doc/html/circular_buffer.html
[boost_variant]: http://www.boost.org/doc/libs/release/doc/html/variant.html

[permanent_store_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/permanent_store.h "#include "maidsafe/data_stores/permanent_store.h""
[data_buffer_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_buffer.h "#include "maidsafe/data_stores/data_buffer.h""
[data_store_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_store.h "#include "maidsafe/data_stores/data_store.h""
[memory_buffer_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/memory_buffer.h "#include "maidsafe/data_stores/memory_buffer.h""
[data_type_values_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_type_values.h "#include "maidsafe/data_types/data_type_values.h""
[immutable_data_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/immutable_data.h "#include "maidsafe/data_types/immutable_data.h""
[structured_data_versions_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/structured_data_versions.h "#include "maidsafe/data_types/structured_data_versions.h""
[mutable_data_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/mutable_data.h "#include "maidsafe/data_types/mutable_data.h""
[data_name_variant_h]: https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_name_variant.h "#include "maidsafe/data_types/data_name_variant.h""
