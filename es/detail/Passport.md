## Descripción general librería Passport

El API Passport proporciona una infraestructura de auto-certificación de clave pública, PKI, que no requiere de ningún directorio central. Una variedad de tipos de clave están disponibles para la realización de diferentes tareas, mientras que las claves públicas, como cualquier otro dato, se almacenan en lugares al azar en la red SAFE. Entidades autorizadas pueden recuperar las claves públicas de la red con el fin de validar cualquier otra entidad o realizar otras operaciones con seguridad y de forma autónoma. El API se basa en un componente criptográfico que forma parte del proyecto [common](https://github.com/maidsafe/MaidSafe-Common/wiki).

## Descripción

Los siguientes archivos contienen la API pública de Passport.

* [passport.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/passport.h) incluye la definición de clase de passport y varias funciones libres que juntas satisfacen las exigencias para las operaciones de transacción/comunicación de la red que sean seguras y anónimas. Los miembros de clase de Passport son identidades definidas en la cabecera [types.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/types.h).
* [types.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/types.h) define identidades de red para la autenticación, almacenamiento/ recuperación de datos y comunicación segura.


