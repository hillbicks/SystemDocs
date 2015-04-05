## Netzwerk Dateisystem Überblick

Das SAFE Netzwerk Dateisystem, NFS, ist eine policy basierende Bibliothek die eine RESTful API anwendet. Genau genommen akzeptiert das Haupt-NFS-Klassen-Template Templateparameter, die selbst Klassentypen sind, egal ob ein Template oder nicht, die die Netzwerk Operationen put, get, delete und post definieren. Das Design ist über verschiedene Parameter skaliert  und vereinfacht die Logik die notwendig ist um Nachrichten für die Verteilung zu paketieren. Das geschieht via [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki), im Auftrag eines Client oder einer [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki) persona.

### Details

Die API deckt eine Anzahl an Dateien ab die das Haupt NetzwerkDateiSystem Klasse Template definieren, die Policy Klassen und zahlreiche Support/Hilfs Strukturen und Funktionen. Die komplette Liste an Dateien, zusammen mit einer kurzen Beschreibung der einzelnen ist unten aufgelistet. Dabei ist zu beachten das Dateien die dem Schema *inl.h folgen nicht spezialisierte Mitglieder Funktionen, Mitglieder Templates und Funktionstemplates implementieren, die in den korrespondieren .h Dateien deklariert und nicht in der Liste aufgeführt sind.

* [maid_node_nfs](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/maid_node_nfs.h) definiert das Netzwerk Dateisystem Klassen Template dessen Template Argumente put, get, delete und post policies festlegen das ein instantiiertes Objekt für die Netzwerk Operationen benutzen wird.
* data_policies.h beinhaltet ein DataPolicy Klassentemplate das von Clients und den diversen Persona benutzt wird die in [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki) definiert sind.
* client_post_policies.h wird vom Client für die Netzwerk Kommunikation benutzt.
* client_utils.h enthält einige Hilfsmittel Funktion und Klassen die die für den Client verfügbar sind.
* structured_data.h stellt die Klassenschnittstelle zur Verfügung um strukturierte Datenversionen vorzuhalten.
* [message.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/messages.h)definiert die generischen Nachrichten Klasse dessen packetierter Inhalt zur empfangenden Node im Netzwerk gesendet wird, die direkt oder indirekt für den Inhalt verantwortlich ist. Beachte das es möglicherweise eine Anzahl an Hops erfordert damit eine Nachricht ihr Ziel erreicht. Die Details dazu finden sich in der DHT Implementierung die von [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) bereitgestellt wird.
* reply.h definiert die Antworten Klassen die initialisiert wird um Informationen über den Ausgang der Netzwerk Operationen zur Verfügung stellt.
* person_id.h definiert eine Struktur die die Identität der Empfängernode im Netzwerk enthält, sowie die Zielpersona einer Nachricht.
* [data_getter.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/data_getter.h) definiert eine Klasse die genutzt wird um die öffentlichen Schlüssel verschiedener Typen abzufragen die im Netzwerk gespeichert sind. Siehe [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki) für die verfügbaren Schlüsseltypen die verfügbar sind.
* [pmid_registration.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/pmid_registration.h) enthält eine Klasse dessen Mitglieder die authentifizierte Registrierung eines [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki) erlauben.
* [types.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/types.h) enthält geeignete Typen Definitionen und Aufzählungstypen die zum Datentransfer genutzt werden.
* [utils.h](https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/utils.h) enthlt Hilfsmittel Funktionen und Klassen die von der Bibliothek benutzt werden.

### Applikation

LifeStuff Client Transaktionen/Kommunikationen sind mit Hilfe des ClientMaidNfs Objektes packetiert, wie unten beschrieben.

```C++
typedef PutDataPolicy<Maid, Persona::kClientMaid> Put;
typedef GetDataPolicy<Maid, Persona::kClientMaid> Get;
typedef DeleteDataPolicy<Maid, Persona::kClientMaid> Delete;
typedef ClientPostPolicy<Maid, Persona::kClientMaid> Post;

typedef NetworkFileSystem<Put, Get, Delete, Post> ClientMaidNfs;
```
Hier ist der ClientMaidNfs als eine Instanz des Netzwerk Dateisystem definiert, mit den Policies PutDataPolicy, GetDataPolicy, DeleteDataPolicy und ClientPostPolicy, alle Klassentemplates, von denen jeder Template Argumente annimmt um eine Identität zu signieren und den Persona Typ des Objektes, in diesem Fall Maid und Persona::kClientMaid.

Das NetzwerkDateisystem Klassentemplate und definierte Policy Klassen die von dieser Bibliothek offengelegt sind, werden auch genutzt aus denen die verschiedenen Vault Persona Nfs Objekte zusammengestellt werden die von  [Vault](https://github.com/maidsafe/MaidSafe-Vault/wiki) gebraucht werden.


[0]: https://github.com/maidsafe/MaidSafe-Routing/wiki
[1]: https://github.com/maidsafe/MaidSafe-Vault/wiki
[2]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/maid_node_nfs.h
[3]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/messages.h
[4]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/client/data_getter.h
[5]: https://github.com/maidsafe/MaidSafe-Passport/wiki
[6]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/vault/pmid_registration.h
[7]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/types.h
[8]: https://github.com/maidsafe/MaidSafe-Network-Filesystem/blob/master/include/maidsafe/nfs/utils.h
