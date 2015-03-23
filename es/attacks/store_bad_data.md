# Guardado de datos ilegales

##Descripción del ataque

Si un atacante quiere subvertir la autoencriptación para tratar de crear datos ilegales o inmorales de tamaño inferior a 1Mb, y nombrarlo con el hash de su contenido, a continuación, potencialmente, el atacante sería capaz de almacenar estos datos inmorales en la Red SAFE.

Como los datos de la autoencriptación están muy ofuscados y son casi imposible de descifrar, el almacenamiento de estos datos no producirían el ataque deseado. Un vault no tiene ninguna manera de impedir que dichos datos se almacenen pero solo serían otros datos encriptados y ofuscados dentro de la red SAFE.

##Propósito del ataque

El propósito de este ataque es solamente desacreditar la red SAFE. Es poco probable que un atacante pueda saber dónde se almacenan los datos y la red no da a conocer esta información. En cualquier caso, un atacante podría afirmar que se logró hacerlo simplemente para crear alarma.

##Evitar el ataque

Debido a no saber donde se almacenan los datos inmorales en la Red SAFE, este ataque queda frustrado ya que el atacante no podría recuperarlos. En sí misma, esta es una forma de protección, sin embargo, la Red SAFE también va un paso más allá.

Cuando un dato se guarda en la red SAFE, la clave usada para el guardado también se usa para encriptar su contenido. Se realiza posteriormente un Hash de dicha clave y el contenido se renombra con este resultado. La clave original y el contenido se borran. De esta manera el Vault que guarda el dato es incapaz de desencriptar los chunk que posee.

La red SAFE puede pedir un chunk, entonces el Vault realiza un Hash de esta solicitud y busca si este Hash coincide con alguno de los que guarda. Si lo encuentra, el vault usará la clave pedida para decodificar el contenido en memoria y enviar los datos solicitados junto la clave. Esto permitirá recomponer el dato como chunk inmutable pero solo para el Vault solicitante.

Esto es por tanto similar a un atacante que encripta los datos totalmente y los almacena en cualquier ordenador o red.
