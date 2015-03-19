#Common Überblick

Die SAFE-Common Bibliothek stellt viele Komponenten zur Verfügung die von anderen SAFE Bibliotheken genutzt werden. Es wird empfohlen diese Komponenten zu benutzen da sie gut getestet sind und im Lauf der Zeit weiter entwickelt werden um Effizienz und Effektivität zu erhöhen.

Grundsätzlich können die verwendeten Komponenten in die folgenden Gruppen eingeteilt werden:


_**Gleichzeitigkeitshelfer**_
* [Active-Object](https://github.com/maidsafe/MaidSafe-Common/wiki/Active-Object) - Eine Implementierung des  implementation of [Active Object Design Pattern](http://en.wikipedia.org/wiki/Active_object).  Nützlich um Operationen in einem sicheren Thread laufen zu lassen.
* [Asio-Service](https://github.com/maidsafe/MaidSafe-Common/wiki/Asio-Service) - Asynchroner Helfer für asynchrone Operationen und Timer, basierend auf  [Boost ASIO](http://www.boost.org/doc/libs/release/doc/html/boost_asio.html).
* [Safe-Queue](https://github.com/maidsafe/MaidSafe-Common/wiki/Safe-Queue) -  Eine intern synchronisierte (nicht sperrfreie) Queue, basierend auf  [deque](http://en.cppreference.com/w/cpp/container/deque). Bitte stattdessen [boost lock free](http://www.boost.org/doc/libs/release/doc/html/lockfree.html) nutzen. Es ist wahrscheinlich das dieser Container abgelöst wird.

_**Typ Sicherheit**_
* [Bounded-String](https://github.com/maidsafe/MaidSafe-Common/wiki/Bounded-String) - Erstellt Stringtypen with ober und unter "Grenzen"
* [Tagged-Value](https://github.com/maidsafe/MaidSafe-Common/wiki/Tagged-Value) - Eine Implementierung von  [the whole value idiom](http://martin-moene.blogspot.co.uk/2012/07/light-on-whole-value.html)

_**Erwartungs Sicherheit**_
* [Errors-Exceptions](https://github.com/maidsafe/MaidSafe-Common/wiki/Errors-Exceptions) - Fehlerhandhabungssystem das  ```std::error_code``` und ```std::exception``` erweitert.
* [On-Scope-Exit](https://github.com/maidsafe/MaidSafe-Common/wiki/On-Scope-Exit) - Ein sehr nützliches Utility um Werte wieder herzustellen oder Funktionen auszuführen wenn es zu einem Fehler kommt.

_**Daten Speicher**_
Die Daten Speicher Sektion implementiert Klassen welche die Anforderungen der RESTful Schnittstelle erfüllen, hauptsächlich PUT, GET und DELETE. Das sind im Grund <Schlüssel, Wert> Speicher, keine Datenbanken.

Diese Schnittstelle wird von den folgendenen Dateien bereitgestellt:

* [permanent_store.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/permanent_store.h) - Eine persistente Speicher Klassen die das Filesystem als Speichermedium benutzt.
* [data_buffer.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_buffer.h) - Ein FIFO Speicher welcher eine Kombination aus Festlatten und Arbeitsspeicher benutzt. Es wurde entworfen um schnelle Lese/Schreibzugriffe auf den Arbeitsspeicher zu erlauben während ein Hintergrundprozess die Daten auf die Festplatte schreibt.

* [data_store.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_store.h) - Eine Richtlinien basierte Speicher Klasse. Diese wird momentan zusammen mit  [DataBuffer](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/data_buffer.h) als Richtlinie verwendet.
* [memory_buffer.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_stores/memory_buffer.h) - Ein Arbeitsspeicher Klasse welche intern [`boost::circular_buffer`][boost_circular_buffer] verwendet um einen schnellen und kleinen FIFO Speicher für nicht persistente Daten bereitzustellen.

_**Datentypen**_

Die Datentypen Sektion wird verwendet um die verschiedenen Typen von Daten die das Netzwerk handhaben kann, zusammenzuführen. Manche Typen sind hier definiert, andere werden einfach aufgezählt. Aufzählungen der Typen sind leider notwendig wenn Daten der aufgereiht werden um sie an die "Peer Nodes" im Netzwerk zu senden.

Die Schnittstelle wird mit den folgenden Dateien geliefert:


* [data_type_values.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_type_values.h) - Eine Aufzählung aller Datentypen die vom SAFE Netzwerk verarbeitet werden können.
* [immutable_data.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/immutable_data.h) - Ein Datentyp dessen Inhalt nicht verändert werden kann, der Name ergibt sich aus dem SHA512 Hash Wert. Dies ist der primaere Datentyp der sich aus der Selbstverschlüsselung einer Datei ergibt.
* [structured_data_versions.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/structured_data_versions.h) - Ein Datentyp der nicht direkt in das Netzwerk geladen wird, sondern viel mehr ein Mittel um variable Daten ins Netzwerk zu laden. Diese Version stellen Verweise auf nicht veränderbare Chunks dar.
* [mutable_data.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/mutable_data.h) - Datentyp der serialisierte Versionen darstellt. Diese Versionen werden gebraucht um Updates zu Ordnern zu speichern, jedes Update wird eine neue Version. Wenn sich der Ordner eines Benutzer verändert, wird es serialisiert, verschlüsselt wenn es notwendig ist und der Inhalt als unveränderbare Daten ins Netzwerk gestellt. Der Name dieser unveränderbaren Daten ist der Wert einer Version (zusammen mit einer Referenz zur vorherigen Version). Diese Version wird dann zu einem veränderbaren Chunk serialisiert und gespeichert. Der Name des Ordners ist ein zufälliger String und bleibt solange bestehen wie der Ordner existiert.
* [data_name_variant.h](https://github.com/maidsafe/MaidSafe-Common/blob/master/include/maidsafe/common/data_types/data_name_variant.h) - Eine [`boost::variant`][boost_variante] der Namen der verschiedenen Datentypen die das Netzwerk handhabt. Das wird primär in den Datenspeichern genutzt in denen es eine Anforderung gibt verschiedene Typen von ähnliche strukturierten Daten zu speichern die aber unterschiedliche Namenstypen haben.

_**Generelle Hilfsmittel**_
* [Utils](https://github.com/maidsafe/MaidSafe-Common/wiki/Utils) - Eine vielfältige Mischung aus Hilfsmitteln, wie ein Hex Konverter, Zufallszahlengenerator, etc.
* [Key Value Buffer](https://github.com/maidsafe/MaidSafe-Common/wiki/Key-Value-Buffer) - Eine gepuffertes, schreibendes Festplattensystem mit einer Caching Fähigkeit.
* [Node Id](https://github.com/maidsafe/MaidSafe-Common/wiki/Node-Id) - Hilfsmittel um 512 bit Adressen zu nutzen wie sie in den SAFE Bibiliotheken vorkommen.

_**Krpytographische Helfer**_
* [Krypto Hilfsmittel](https://github.com/maidsafe/MaidSafe-Common/wiki/Crypto-Utils) - Symetrische Verschlüsselung, Hash wrapper, sicherer Passwort Wrapper, N+P Schlüsselteilung (basierend auf Shamir's Algorithmus)
* [Asymmetrische Verschlüsselung](https://github.com/maidsafe/MaidSafe-Common/wiki/Asymmetric-Encryption) - Wrapper für asymetrische Verschlüsselungsmethoden (inklusive SAFE Verschlüsselung), momentan unterstützt durch RSA.
*
_**Debug Helfer**_
* [Test Runner](https://github.com/maidsafe/MaidSafe/wiki/Running-Tests)
* [Protokollierung](https://github.com/maidsafe/MaidSafe/wiki/Logging-Options)

## NetWork_Viewer

[NetWork Viewer](http://visualiser.maidsafe.net:8080/auth) ist ein UI Tool von Maidsafe um das Maidsafe-Routing Netzwerk zu visualisieren. Es kann dabei helfen zu verstehen wie das Netzwerk funktioniert und Routing Algorithmen zu debuggen.


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
