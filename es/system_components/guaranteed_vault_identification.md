# Garantía de identificación del Vault
La identificación de los Vaults es esencial si estos se van a encargar y responsabilizarse, en gran medida, de la gestión de los recursos.

Sin identificación, cualquier Vault podría suplantar a cualquier otro. Hay métodos conocidos de identificación digital, como los emitidos por una autoridad de certificación (como Verisign) o, en algunos casos, una web de confianza. En tanto en cuanto la red SAFE no tiene servidores, no hay intervención humana y es completamente no confiable, estas opciones no son adecuadas.

El proceso de identificación de un Vault involucra la creación de dos claves. Una de ellas es una clave de revocación y será usada solo para crear e invalidar la clave real.

Cuando la clave real se crea, su clave pública se firma con la clave privada de revocación y este paquete (clave pública mas firma) se guarda en la red como clave de identificación de un Vault. Su Hash se usa como identidad de dicho Vault.

La red SAFE puede recuperar este paquete de identificación a petición de cualquier otro Vault. La única manera de alterar este paquete es por un mensaje firmado por la misma identidad (ID) que el anterior paquete.

Este proceso permite a los Vaults identificarse a si mismo publicitando el Hash del paquete de identificación que él mismo ha creado y guardado.

Como solo el creador posee la clave privada emparejada con la clave pública contenida en el paquete de identificación, podemos asumir que se corresponde con el Vault correcto. Todos los mensajes están encriptados con su ID por lo que los Vault fraudulentos no podrán desencriptar esos mensajes, eso incluye las peticiones de conexión que se requieren para unirse a la red.
