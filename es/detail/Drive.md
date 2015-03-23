## Descripción general librería Drive

La librería MaidSafe-Drive presenta una interface en modo usuario del módulo kernel de Eldos' [Callback File System](https://www.eldos.com/cbfs/), [Fuse](http://fuse.sourceforge.net/) y [OSXFuse](http://osxfuse.github.io/) disponible para las respectivas plataformas windows, linux y mac. En cada caso se puede montar un disco virtual y presentarlo como un disco nativo del sistema. Las operaciones realizadas, por el sistema de ficheros en el disco, son sustituidas por las llamadas, que el código en modo usuario, debe realizar. Los ficheros/directorios son encriptados usando los recursos presentes en [Encrypt](https://github.com/maidsafe/MaidSafe-Encrypt/wiki), mientras son almacenados/recuperados directamente a/de la red via [Network-Filesystem](https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki), [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) y [RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki). El espacio disponible/usado en la unidad se deriva de los valores que el Cliente informa sobre los ofrecido/utilizado a la red por el Vault.

### Detalles

El API MaidSafe-Drive incluye los siguientes ficheros:

* [drive_api.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/drive_api.h) define la clase base con la interfaz compartida a las clases específicas derivadas de cada plataforma definidos en los archivos de cabecera que se describen a continuación.
* [win_drive.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/win_drive.h) incluye el código de modo usuario específico para windows y [Callback File System](https://www.eldos.com/cbfs/).
* [unix_drive.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/unix_drive.h) incluye el código de modo usuario específico para linux y [Fuse](http://fuse.sourceforge.net/).
* [config.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/config.h) Proporciona las convenientes definiciones de tipos y constantes.

Más detalles que describen las interfaces de clase, etc., se pueden encontrar dentro de los archivos y también en el material de apoyo disponible de la librería.




