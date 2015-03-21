# Eliminación de la administración

La red SAFE es autónoma y por lo tanto no se necesitan administradores para gestionar o mantener los datos, red incluida. Cada Vault se adapta constantemente a su entorno local y las comprobaciones de integridad de los datos permite detectar corrupciones o mutaciones en los chunk de datos simplemente chequeando el hash de cada chunk.

La red mantiene automáticamente cuatro copias disponibles de cada trozo de dato pero también hasta diez y seis copias perdidas en tanto en cuanto máquinas apagadas podrian volver a unirse a la red en el futuro.

Los datos son guardados de forma aleatoria en un espacio de 2^512 direcciones de red. Esto distribuye automáticamente la carga uniformemente a través de la red, que es una tarea tradicionalmente realizada por los administradores de red.
