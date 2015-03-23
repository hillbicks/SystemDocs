# Seguridad - Autoencriptación (Self encryption)
La seguridad de los datos de usuarios es un aspecto crítico de la red SAFE y, en parte, está proporcionada por el sistema de autoencriptación (Self encryption). La red SAFE requiere que los datos guardados sean irreconocibles como dato y resistentes a la desencriptación, incluso en el caso que el algoritmo de encriptación esté comprometido.

La autoencriptación (Self encryption) se usa para mezclar y encriptar los datos antes de enviarlos a la red SAFE. Este proceso se realiza de forma automática e instantánea.

Cuando los datos se guardan en el disco virtual SAFE, son divididos en un mínimo de tres trozos (chunk), [Hasheados] (http://es.wikipedia.org/wiki/Funci%C3%B3n_hash) y encriptados. Adicionalmente, y para aumentar la ofuscación, cada trozo (chunk) es tratado con la función [XOR] (http://es.wikipedia.org/wiki/Cifrado_XOR) usando el hash de otros trozos.
Cada trozo se rompe de nuevo y un par de valores clave se añaden a una tabla llamada mapa de datos. Cada mapa de datos contiene la localización de cada trozo en que se ha dividido el fichero. Este mapa de datos, que contienen los Hash previo y posterior a la encriptación, se usa para recuperar y decodificar los datos ya que, sin ellos, el proceso de encriptación es no reversible.

Este proceso se realiza en el ordenador del cliente por lo que los datos siempre se encuentran encriptados dentro de la red SAFE y solo el usuario con las debidas credenciales puede desencriptar el fichero. Esto hace que el password no puede ser robado desde la red ya que nunca sale del ordenador del usuario.
Para aumentar la seguridad, el mapa de datos también pasa por su propio proceso de autoencriptación.

La red SAFE usa [deduplicación de datos] (http://en.wikipedia.org/wiki/Data_deduplication) para asegurar que dicho espacio se usa de manera eficiente evitando guardar múltiples copias de los mismos datos. La red es capaz de saber si dos piezas son idénticas gracias a poseer el mismo Hash. También los diferentes Vault usan un Hash para identificarse a si mismo.

[Aquí se puede ver un video explicativo de la autoencriptación] (http://www.youtube.com/watch?v=Jnvwv4z17b4)

Aquí debajo tenemos una visión de conjunto del proceso.

![Self encryption figure](./img/self-encryption.png)
