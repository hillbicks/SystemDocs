# Como funciona

Cuando un usuario descarga e instala el cliente Maidsafe, un vault se configura en su ordenador..

Después de unirse y firmar en la red SAFE, el usuario verá un nuevo disco virtual montado en su sistema. Seleccionando este disco le mostrará los datos estáticos que han sido encriptados y distribuidos por otros vault.

Un vault de usuario también podrá manejar datos dinámicos como, por ejemplo, una comunicación a través a una aplicación VOIP.

Antes que los datos se guarden en la red SAFE son automáticamente encriptados. El proceso de autoencriptación implica dividir los datos en pequeños pedazos llamados chunks y encriptarlos usando tanto los datos del inicio de sesión (clave, pin, password) del usuario como los propios datos en sí mismo. Esto hace que para ver los datos se necesitan tanto los correspondientes al inicio de sesión como los de los datos y ninguna de esta información se guarda en en ningún servidor u otro tercer lugar.

Normalmente el usuario se conecta a la red a través de un router. En la red SAFE el router usa el protocolo RUDP (Reliable UDP) para dicha conexión. RUDP es un protocolo más robusto que UDP ya que los paquetes perdidos son retransmitidos y es capaz de atravesar el NAT del router al contrario que el protocolo TCP. El uso del RUDP para la red SAFE permite que los datos pasen a través de los router sin poder ser corrompidos o interceptados.

Los Vault de los usuarios se conectan a otros Vault como parte del guardado y gestión de los datos. Los Vaults son constantemente controlados y clasificados por unos gestores llamados "Data holder managers personna" usando los siguientes criterios:

* **Disponibilidad** - La frecuencia en que el Vault se apaga y enciende
* **Almacenamiento** - Cuanto espacio de almacenamiento existe en el Vault
* **CPU** - Cuantos recursos de CPU tiene el Vault
* **Ancho de banda** - Como de rápido o lento se acceden a los datos del Vault

En tanto en cuanto las peticiones y recursos de la red SAFE son cambiantes, los Vault se adaptan y equilibran continuamente la carga de la red. Este ajuste se realiza de forma automática por los propios Vault. Como la red SAFE es completamente autónoma, reacciona de forma rápida y sin intervención humana.

Cuando un usuario proporciona a la red SAFE mas almacenamiento del que usa es premiado, en un proceso aleatorio, con safecoin. Los safecoin son necesarios para acceder a los servicios de la red. El usuario puede ver cuantos safecoin posee a través de su wallet. Este wallet se crea y configura de forma automática como parte de la instalación del cliente Maidsafe.

Los desarrolladores de aplicaciones ganan safecoin cuando crean aplicaciones y programas para la red SAFE. En la medida que esas aplicaciones se usan, el desarrollador gana safecoin.

Los desarrolladores del software del núcleo también pueden ganar safecoin contribuyendo al desarrollo de su código base.
