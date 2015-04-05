### Überblick

MaidSafe-Encrypt implementiert Funktionen, die sich auf die Selbstverschlüsselung von Dateien und Ordnern beziehen.

Das Interface der Library wird durch die folgenden Dateien zur Verfügung gestellt:

* [self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h) - dies ist die haupt API und wird weiter unten im Detail beschrieben
*
[data_map.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/data_map.h) - ein Konstrukt, dass die Verschlüsselungsdetails für eine einzelne Datei beinhaltet
*
[config.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/src/maidsafe/encrypt/config.h) - stellt const Library konfigurations-Variablen und return codes. In Zukunft werden return codes durch fehler-behandlungs Mechanismen ersetzt, die in der [MaidSafe Common library](https://github.com/maidsafe/MaidSafe-Common/wiki) bereitgestellt werden.



Die Library benutzt [OpenMP](http://en.wikipedia.org/wiki/OpenMP), wenn verfügbar für die Parallelisierung von Aufgaben. Sie ist auch auf die folgenden MaidSafe Libraries angewiesen

 [Common](https://github.com/maidsafe/MaidSafe-Common/wiki), [Private](https://github.com/maidsafe/MaidSafe-Vault-Manager/wiki), [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki), [RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki), [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) und [Network-Filesystem](https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki).


### Background

[Self-encrypting](http://maidsafe.net/images/opportunistic-caching.png)
eine Datei benötigt keinerlei anderen Input als die Datei selbst. Das Resultat sind mehrere verschlüsselte Daten Chunks, die in einem beständigen key-value store (z.B. ein MaidSafe Network Vault) abgelegt werden, und eine 'DataMap'.

Die DataMap besteht aus zwei Listen: Die erste Liste enthält die pre-encryption [SHA512 hashes](https://en.wikipedia.org/wiki/SHA-2) von den chunks der Dateien. Die zweite Liste
enthält die post-encryption hashes. Die pre-encryption hashes von zwei anderen chunks der Datei werden in der verschlüsselung/entschlüsselung eines jeden chunks verwendet. Die post-encryption hashes sind die Namen der verschlüsselten chunks, d.h. der Schlüssel unter dem sie im DHT gespeichert sind. Das Resultat ist, dass ein Benutzer der eine DataMap besitzt, die verschlüsselten chunks anfordern und durch Entschlüsselung die originale Datei herstellen kann.


Im Fall von Verzeichnissen, können die Inhalte (eine Kollektion von DataMap und Meta Informationen) zu einer einzelnen Datei serialisiert werden. Diese Datei, wenn self-encrypted, ergibt seine eigene DataMap, die auch im key-value store abgelegt werden muss. Die DataMap kann allerdings nicht unverschlüsselt gespeichert werden, wenn die Inhalte verborgen bleiben sollen. Diese Verzeichnis-DataMaps werden verschlüsselt/entschlüsselt indem zwei zusätzliche Informationsteile gebraucht werden. Im Fall des MaidSafe Netzwerkes, ist das erste Teil normalerweise eine zufällige ID, die dem Verzeichnis zugeschrieben wird. Das zweite Teil ist die ID des übergeordneten Verzeichnisses. Die IDs könne als Schlüssel in dem key-value store benutzt werden, d.h. sie sind stränge derselben Länge wie SHA512 hashes. Die ID für ein Verzeichnis wird also der Schlüssel, unter welchem seine verschlüsselte DataMap gespeichert ist.



### Details


[self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h) stellt freie Funktionen für die Verschlüsselung und Entschlüsselung von DataMaps zur Verfügung. Es definiert auch die `SelfEncryptor` Klasse, die beim Verschlüsseln/Entschlüsseln von Dateien verwendet wird.

Die `SelfEncryptor` Klasse ist designed um kompatibel mit den Typen von calls zu sein, die zu erwarten sind wenn Encrypt unter einem virtuellen Dateisystem (VFS) wie [FUSE](http://fuse.sourceforge.net/) benutzt wird. Es ist designed um eine throwaway Klasse zu sein; eine neue Instanz wird konstruiert wenn ein Datei handle im VFS geöffnet ist und wird zerstört wenn das handle abgelegt wird. Als solches ist die Klasse [conditionally thread safe](http://en.wikipedia.org/wiki/Thread_safety#Levels_of_thread_safety).

Der Konstruktor benötigt einen pointer zu einer `DataMap`, Zugang zu dem DHT über ein non-const ref zu einem `nfs::ClientMaidNfs` und Zugang zu einem Puffer für temporäre chunks über ein non-const ref zu einem `data_store::PermanentStore`. Desweiteren kann die Anzahl and threads die zu benutzen sind für die Verschlüsselung/Entschlüsselung bereitgestellt werden.

Weitere Details der individuellen Funktionen können in der [self_encryptor.h](https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h) Datei selbst gefunden werden.


[se]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/self_encryptor.h "#include "maidsafe/encrypt/self_encryptor.h""
[dm]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/include/maidsafe/encrypt/data_map.h "#include "maidsafe/encrypt/data_map.h""
[config]: https://github.com/maidsafe/MaidSafe-Encrypt/blob/master/src/maidsafe/encrypt/config.h "#include "maidsafe/encrypt/config.h""

[common]: https://github.com/maidsafe/MaidSafe-Common/wiki "MaidSafe-Common library"
[private]: https://github.com/maidsafe/MaidSafe-Private/wiki "MaidSafe-Private library"
[passport]: https://github.com/maidsafe/MaidSafe-Passport/wiki "MaidSafe-Passport library"
[rudp]: https://github.com/maidsafe/MaidSafe-RUDP/wiki "MaidSafe-RUDP library"
[routing]: https://github.com/maidsafe/MaidSafe-Routing/wiki "MaidSafe-Routing library"
[nfs]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki "MaidSafe-Network-Filesystem library"
[se_video]: http://maidsafe.net/self-encrypt-video

[omp]: http://en.wikipedia.org/wiki/OpenMP
[sha2]: https://en.wikipedia.org/wiki/SHA-2 "Secure Hashing Algorithm v2 - SHA512"
[fuse]: http://fuse.sourceforge.net/ "Filesystem in Userspace"
[thread_safety]: http://en.wikipedia.org/wiki/Thread_safety#Levels_of_thread_safety "Levels of thread safety"
