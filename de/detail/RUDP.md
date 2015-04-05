## RUDP Überblick
RUDP (Reliable UDP) implementiert pseudo Verbindungen mittels UDP um viele der Vorteile eines Verbindungsbasierenden Protokolls wie TCP zu erreichen, dabei aber [NAT Traversal](https://de.wikipedia.org/wiki/Network_Address_Translation#NAT-Traversal) zu ermöglichen welches TCP nicht kann. Darüber hinaus werden alle Daten zwischen den beiden Endpunkten verschlüsselt indem ein sicherer und verifizierbarer RSA Public-Key Austausch Mechanismus genutzt wird. Das bildet einen Teil von [PKI](https://de.wikipedia.org/wiki/Public-Key-Infrastruktur) welches vom SAFE Netzwerk bereitgestellt wird.

Die Schnittstellen der Bibliothek wird mit den folgenden Dateien zur Verfügung gestellt:

* [managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h) - Das ist die Haupt API und wird weiter unten in mehr Details beschrieben
* [parameters.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/parameters.h) - stellt Variablen für die Bibliotheken Konfiguration
* [return_codes.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/return_codes.h) - Die Nutzung von return codes wird in Zukunft vom Fehlerbehandlungs Mechanismus in  [MaidSafe-Common](https://github.com/maidsafe/MaidSafe-Common/wiki) bereitgestellt.
* [nat_type.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/nat_type.h) - eine Aufzählung der relevanten  [NAT](https://de.wikipedia.org/wiki/Network_address_translation) Typen die die Bibliothek identifizieren muss.

Die Bibliothek macht starken Gebrauch von  [Boost.Asio](http://www.boost.org/doc/libs/1_55_0/doc/html/boost_asio.html), sowohl für Netzwerk bezogene Operationen als auch asynchrone Operationen. Es ist ausserdem abhängig von den Maidsafe Bibliotheken [Common](https://github.com/maidsafe/MaidSafe-Common/wiki), [Private](https://github.com/maidsafe/MaidSafe-Vault-Manager/wiki), and [Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki).


### Hintergrund

Zwei RUDP Nodes halten eine pseudo Verbindung oder "managed Verbindung" aufrecht, indem kontinuierlich kleine keepalive Kontrolnachrichten zwischen ihnen gesendet werden. Die Verbindung kann von jeder Seite geschlossen werden indem eine Abschaltungskontrollnachricht gesendet wird, daher bekommen Peers unter normalen Umständen sehr schnell mit wenn eine Verbindung geschlossen wurde. Sollte eine feste Anzahl von keepalives einer Verbindung  hintereinander nicht empfangen werden so sieht der Node diese Verbindung als geschlossen an. Das ist ein langsamerer Mechanismus als die Abschalt Nachricht, sie sollte jedoch viel seltener vorkommen da sie durch ein unerwartetes Abschalten des Peer Prozesses oder durch unerwarteten Verbindungsverlust der Netzwerkverbindung ausgelöst wird.

Für jede Verbindung hat eine Node einen `EndpointPair` der der Node und dem anderen Peer zugeordnet ist. Der `EndpointPair` enthält den "lokalen" Endpunkt (IP/Port wie er hinter dem Router aussieht) und dem "externen" Endpunkt (IP/Port wie er von ausserhalb des Routers aussieht). Externe Endpunkte werden bevorzugt, interne werden benutzt wenn die externen fehlschlagen und die beiden Peers beide hinter einem Router sind der kein hairpinning erlaubt.

Eine Verbindung zwischen zwei Peers die sich beide hinter Routern mit [Symmetrischem NAT](https://en.wikipedia.org/wiki/Network_address_translation#Methods_of_port_translation) befinden kann nicht hergestellt werden. Die [SAFE Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) Bibliothek handhabt dies indem ein proxy Node für solchen "versteckten" Nodes zur Verfgung gestellt wird, von daher versucht die RUDP Bibliothek gar nicht erst dieses Problem zu lösen.

### Details

[managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h) definiert drei Funktoren: `MessageReceivedFunctor`, `ConnectionLostFunctor`, und `MessageSentFunctor`. Die ersten beiden müssen in der `ManagedConnections::Bootstrap` Funktion bereitgestellt werden, können immer wieder von RUDP aufgerufen werden und sind selbsterklärend.

Der `MessageSentFunctor` wird genau einmal pro Aufruf von `ManagedConnections::Send` aufgerufen. Das zeigt an das die dazugehörige Nachricht vom Ziel Peer empfangen wurde (nicht nur zum Senden eingereiht, sondern tatsächlich empfangen) oder es fehlgeschlagen ist. Es wird davon ausgegangen (aber nicht angefordert) das eine separate Instanz von `MessageSentFunctor` jedem Aufruf zum Senden übergeben wird.

Intern hält eine `ManagedConnections` Klasse diverse (bis zu `Parameters::max_transports`) Transport Objektve aufrecht, jedes mit seinem eigenen Netzwerk Socket. Dieser Socket wird benutzt um diverse (momentan bis zu 50) (siehe [transport.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/src/maidsafe/rudp/transport.h)) `Transport::kMaxConnections`) pseudo Verbindungen aufrecht zu erhalten. Daher ist es wahrscheinlich das eine geringe Anzahl von `ManagedConnections` Objekten eine optimale Performance bieten werden.

Bootstrapping eines `ManagedConnections` Objektes kann ein langsamer Prozess sein, da versucht wird Verbindungen zu zahlreichen potentiellen Endpunkten aufzubauen bis eine erfolgreich ist.

`ManagedConnections::kResiliencePort()` bietet einen de gesamten Netzwerk bekannten Port der im Falle einer DisasterRecovery genutzt werden kann. Jede `ManagedConnections` Instanz versucht diesen Port lokal zu öffnen. Nach einer Netzwerk Segmentierung können die Nodes beispielsweise versuchen sich zu anderen lokalen Nodes zu verbinden, oder um verbundene Nodes mit Hilfe dieses Ports anzuweisen.

Weitere Details zu den einzelnen Funktionen sind in der [managed_connections.h](https://github.com/maidsafe/MaidSafe-RUDP/blob/master/include/maidsafe/rudp/managed_connections.h) Datei selbst zu finden.
