## Routing Überblick



Routing ist eine verteile Hash Table Bibliothek die auf einer [Kademlia](http://de.wikipedia.org/wiki/Kademlia) ähnlichen Routing Tabelle besteht. Routing spezifiziert die Netzwerk Struktur legt den Pfad zwischen Paaren von Nodes im Netzwerk fest, indem lokale Informationen jeder Zwischen Node genutzt werden.
Jede Routing Node speichert routing Informationen über die Nodes zu denen es direkt verbunden ist. Ausserdem hat jede Node partielles Wissen über die lokalen Informationen seiner nächsten Nodes (benachbarter ID Raum).
Die Informationen die in jedem Node gespeichert sind tragen zur Nachrichten Durchreichung Infrastruktur vom Routing bei. Der Austausch von Informationen im Routing beinhaltet typischerweise das Durchqueren von intermediären Nodes. Die Kommunikation zwischen jedem Nodepaar wird durch die Bereitstellung von [MaidSafe-RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki) erledigt. Ein Mechanismus zur Bestätigung/erneuten Übertragung um zuverlässige Nachrichtenzustellung zu garantieren wird ebenfalls bereitgestellt. Die Möglichkeit Die Fähigkeit eine effiziente Plattform für den Nachichtenaustausch zwischen Peers anzubieten, macht Routing eine ideale Overlay Netzwerk Komponente eines jeden Peer-2-Peer Netzwerks.

Routing bietet die folgenden Feature:

* Dem Netzwerk beitreten
* NAT traversal
* Betriebsmodi
* Zuverlässige direkte und Gruppen Nachrichten
* Evaluierung der Entfernung
* Node Identifikation mittels PKI bereitgestellt durch SAFE-Passport
* Caching Mechanismus
* Churn Handhabung
* Routing Tabelle und Gruppen Matrix


Die Schnittstellen der Bibliothek werden in den folgenden Dateien zur Verfügung gestellt:
* [routing_api.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/routing_api.h) - das ist die Haupt API und wird weiter unten mehr beschrieben.
* [api_config.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/api_config.h) - stellt die Erklärung der Typen bereit welche die Bibliothek benutzen.
* [node_info.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/node_info.h) - stellt Informationen über die Node bereit.
* [parameters.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/parameters.h) - stellt Konfigurationsvariablen der Bibliotheken bereit.
* [return_codes.h](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/return_codes.h)

### Übersicht

### Dem Netzwerk beitreten
Eine Node kann dem Netzwerk beitreten als:
1. Eine normale Node wenn das Netzwerk bereits existiert.
2. Als eine Null Zustand Node, wenn die Node eine von zwei der ersten Nodes ist die das Netzwerk erstellen.

####Als normale Node dem Netzwerk beitreten

Um einem existierenden Netzwerk beizutreten muss eine Node mit einer Liste von Endpunkten der Nodes versorgt werden, die bereits Teil des Netzwerks sind. Die Node versucht sich zu einem der Endpunkte aus der Liste zu verbinden. Sobald ein Verbindungsversuch erfolgreich war, ermittelt die verbindende Node die Node die ihr am nächsten ist und verbindet sich zu ihr [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc). Die Verbindung zur nächsten Node [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) ist entscheidend für einen verlässlichen Betrieb der Node. Der Rest der Rating Tabelle ist hauptsächlich mit zufälligen Nodes des Netzwerks gefüllt um einheitlichen Zugang zu verschiedenen Teilen des Netzwerk zu gewährleisten.


####Als Null Zustand Node beitreten

Der initiale Aufbau des Netzwerk, welcher Null Zustand genannt wird, beinhaltet zwei Nodes. Diese beiden Nodes werden mit Endpunkten voneinander versorgt und bilden durch die gegenseitige Verbindung das Netzwerk. Die Null Zustand sind identisch zu allen anderen Nodes im Netzwerk und können das Netzwerk zu jeder verlassen und wieder beitreten.


###NAT Traversal

Ein Ziel von Routing ist es die Kommunikation zwischen jedem Paar von Nodes im Netzwerk zu ermöglichen, unabhängig von deren Netzwerk Konfigurationen. Routing in Kombination mit [MaidSafe-RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki) vollzieht [hole punching](http://www.brynosaurus.com/pub/net/p2pnat/) um die direkte Verbindung zwischen jedem Nodepaar zu ermöglichen. Hole punching ist verfügbar solange beide Nodes sich nicht hinter symmetrischen Router befinden. Wenn sich beide Nodes hinter einem symmetrischen Router befinden ermöglicht Routing die Kommunikation zwischen den beiden Nodes indem eine dritte Node als Proxy zwischen den beiden Nodes gewählt wird.


###Betriebsmodi
####Non-Client

Eine non-client Node ist eine volle Routing Node das zum Betrieb und Aufrechterhalten des Netzwerk beträgt. Non-client Nodes sind Teil von DHT und aktiv bei den Routing Entscheidungen die getroffen werden.
Non-client Nodes können:
* Anfragen zu jedem non-client Node senden
* Anfragen zu nur verbundenen Client Nodes senden
* Eingehende Anfragen von jeder Node im Netzwerk empfangen.

####Client

Im Gegensatz zu Non-client Nodes, trägt eine Client Node nicht zur Routing Netzwerk Infrastruktur bei.
Client Nodes sind leichte Routing Nodes die minimale Netzwerk Ressourcen verbrauchen. Diese Nodes erhalten Zugang zum gesamten Netzwerk indem Verbindungen zur Nodes erstellt werden die der eigenen ID am nächsten sind [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc).

Eine Client Node kann:
* Anfragen an jede Non-client Node senden
* Anfragen an Clients mit der gleichen ID senden
* Eingehende Anfragen werden nur von verbundenen Non-client Nodes empfangen.


###Verlässliche direkte und Gruppen Nachrichten

Routing bietet zwei Arten der Kommunikation; direkt und gruppe.

####Direkte Nachrichten

Beim direkten Nachrichten senden ist das Ziel eine bekannte Node im Netzwerk und ist der finale Empfänger der Nachricht.


####Gruppen Nachricht

Eien Gruppe besteht aus einer Zahl von Nodes welche am nächsten an einer bestimmten ID sind. Die Anzahl der Gruppenmitglieder ist durch [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) definiert.

Bei der Gruppen Kommunikation ist das Ziel einer Nachricht eine Gruppe von Nodes welche dem Ziel am nächsten sind. Eine Node die als Ziel seine ID hat wird nicht Teil der Gruppen Kommunikation sein.


###Routing Tabelle und Gruppen Matrix
Die Routing Tabelle ist die Hauptkomponente der Routing Bibliothek. Sie speichert Informationen über eine Anzahl von Nodes im Netzwerk um Routing Entscheidungen für das Zustellen von Nachrichten zu treffen. Jeder Eintrag der Routing Tabelle entspricht einer direkten Verbindung von einer Routing Node zu dieser Node. Jede Node im Netzwerk hat eine ID aus 512 bits.Die Entfernung zwischen jedem Nodepaar wird mit Hilfe von XOR errechnet.
Ein zuverlässiger und effizienter Routing Betrieb im Netzwerk setzt voraus das jede Node a) von Nodes weiss die sich in unmittelbarer Nähe befinden und b) einen direkten Zugang zu verschiedenen Teilen des Netzwerk hat. Die Routing Tabelle speichert Informationen über:

* [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) nächsten Nodes
* Zufällig ausgewählte Nodes im Netzwerk
* Jede andere Node die diese Node als eine ihrer nächsten Nodes findet [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc)

Die oben aufgeführten Information die in der Routing Tabelle gespeichert werden sind genug für den Grossteil der Fälle um eine richtige Entscheidung treffen zu können. Es mag jedoch ungenuegend sein wenn zwei Nodes unterschiedliche Ansichten über die Nähe zueinander oder zu einer dritten Node haben. Um diese Situationen handhaben zu können ist die Routing Tabelle mit einer Gruppenmatrix ausgestattet.
Die Idee hinter der Gruppenmatrix ist es den Nodes mehr Wissen über den Bereich des Netzwerk bereitzustellen und wo sie sich darin befinden. Das wird erreicht indem jede Node partiell über die Nodes in seiner Nähe aus der Routing Tabelle unterrichtet wird. Eine Gruppenmatrix einer Node besteht aus:
* (i) Die nächsten Nodes  [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) der Node
* (ii) Andere Nodes die diese Node als eine ihrer nächsten betrachten. [Parameters::closest_nodes_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc)
* (iii) Nächste Node jedes Eintrages  in (i) & (ii)

Aktuelle Informationen in der Routing Tabelle und der Gruppenmatrix sind notwendig um korrekt Entscheidungen zu treffen. Daher bietet Routing einige Dienste an; um Updates in der Routing Tabelle einer Node schnell darstellen zu können und die Routing Tabelle und Gruppenmatrix jeglicher Node auf die Änderungen aufmerksam zu machen.


###Umgebungs Berechnung

Die meisten Vorgänge im Netzwerk werden in der Gruppen von Nodes ausgeführt die sich in unmittelbarer Nähe der gegeben ID befinden. Die Routing Tabelle versorgt die Nodes, zusammen mit der Gruppenmatrix, mit ausreichend Wissen über den Teil des Netzwerk (zu dem sie gehören) um die Peer Nodes die die gleiche Gruppen ID haben akurat zu identifizieren.

Die Node die Teil einer Gruppe ist, ist sich über andere Nodes in der Gruppe bewusst. Basierend auf diesem Wissen stellt die Routing API Methoden bereit um herauszufinden:

* Ob eine Node am nächsten zu einer gegebenen ID ist
* Ob eine Node Teil einer gegebenen Gruppe ist.
* Alte und neue Mitglieder einer Gruppe nach einem Churn Ereigniss
*

Basierend auf der durschnittlichen Netzwerkentfernung/Population stellt es ausserdem Methoden zur Verfügung um einzuschätzen ob eine Node nah genug zu einer bestimmten ID ist.

###Churn Handhabung

Beitreten und verlassen eines P2P Netzwerkes sind normale Ereignisse. Peer Fluktuation, oft auch churn rate (Abwanderungsquote) genannt, wird effizient von der Routing Bibliothek gehandhabt. Z.b. wird das Beitreten oder Verlassen einer Node des Netzwerk innerhalb weniger Sekunden in der Routing Tabelle der umliegenen Nodes dargestellt.

###Matrix Veränderung (Churn Ereigniss)

Im Routing Netzwerk werden Daten in der Regel an einer logischen Gruppen ID gespeichert. Das bedeutet das Daten auf den Nodes gespeichert werden die am nächsten  [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) zu einer gegebenen Gruppen ID sind. Jede Abwanderung im Netzwerk kann dazu führen das eine Node nah oder fern von der Gruppen ID im Netzwerk verschoben wird. Das bedeutet das viele der logischen Gruppen neu konfiguriert werden indem sie neue Mitglieder in der Gruppe haben oder alte Mitglieder verlieren.

Im Falle von:

1. Nodes verschwinden aus einer logischen  Gruppe: Neue Node(s) werden die alten basierend auf ihrer Entfernung zur Gruppen ID ersetzen und werden zur neuen [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) nahen Gruppe. Danach werden alle Nachrichten die dieser Gruppen ID gesendet werden auch von den neuen Nodes empfagen. Es ist die Verantwortung der anderen verbleibenden Nodes der Gruppe die Daten schnell auf die neuen Nodes zu replizieren.
2. Nodes erscheinen in der logischen Gruppe: Manche der Nodes die Mitglied der Gruppe sind werden sich weit von der Gruppen ID entfernen und werden nicht in der Nähe [Parameters::node_group_size](https://github.com/maidsafe/MaidSafe-Routing/blob/master/src/maidsafe/routing/parameters.cc) der Gruppen ID verbleiben. Ausserdem werden diese Nodes keinen Gruppennachrichten mehr erhalten die für die Gruppen ID vorgesehen sind. Diese Nodes sollten die Daten löschen für die sie nicht länger verantwortlich sind.


Um Daten in dieser dynamischen Gruppe verlässlich am Leben und verfügbar zu halten, sollten alle verantwortlichen Nodes Daten replizieren sobald sie vom Netzwerk Churn erfahren. Routing stellt die MatrixChange Klasse zur Verfügung um solche Abwanderungen schnell und zuverlässig erkennen zu können ob Datenreplikation für eine neue Node in der Gruppe notwendig ist. Nodes die sich nahe am Segment des Netzwerk befinden werden ein Abwanderungsevent durch Beobachtung der Gruppenmatrix mitbekommen. Wenn Gruppenmatrix Veränderungen bei jeder Abwanderung erfolgen erstellt ein MatrixChange Objekt die eine Liste der alten und neuen Nodes der Gruppenmatrix enthält. Diese Klasse bietet Helfer Funktionalitäten um jede Gruppen ID zu evaluieren um herauszufinden ob die Node die das Abwanderungsereigniss empfängt:

* Ist immer noch Teil der Gruppe und verantwortlich für die Daten die in der Gruppe gespeichert werden.
* Ist nicht mehr Teil der Gruppe und muss Daten die zur Gruppe gehören löschen.
* Muss Daten zu einer neuen Node replizieren.

```C++
  CheckHoldersResult CheckHolders(const NodeId& target) const;
```
CheckHoldersResult stellt eine Liste der alten und neuen Halter der Gruppe und proximity_status der Node im Hinblick auf die gegebene Gruppen ID zur Verfügung.


```C++
struct CheckHoldersResult {
  std::vector<NodeId> new_holders;  // New holders = All 4 New holders - All 4 Old holders
  std::vector<NodeId> old_holders;  // Old holders = All 4 Old holder ∩ All Lost nodes
  routing::GroupRangeStatus proximity_status;
};
```




###Caching Mechanismus

Um schnellen Zugang zu beliebten Inhalten zu ermöglichen, bietet Routing Möglichkeiten die es möglich machen Daten auf intermediären Nodes zwischenzuspeichern oder Daten von intermediären Nodes zu lesen wenn die Daten dort verfügbar sind. Die Funktionalität wird durch das Setzen eines Markers in der Routing Nachricht erreicht. Wenn der Nachrichtentyp eine Anfrage ist, wird der Cache in jeder intermediären Node auf dem Weg zun finalen Node nach den Daten gefragt. Wenn die Daten im Cache sind wird die intermediäre Node die Anfrage beantworten und die Antwort zum Sender schicken. Wenn der Nachrichtentyp eine Antwort ist, werden die Daten auf jeder intermediären Node vom Ziel zur Quelle gespeichert.

###Rückrufe

Eine effiziente Plattform zum Austausch von Nachrichten zwischen Peers anzubieten macht Routing zu einer idealen Kommunikations Komponente eines jeden P2P Systems. Um eine einfache und loose gekoppelte Nutzung der Routing Komponenten zu ermöglichen bietet Routing den Host Komponenten eine Anzahl von Rückruf Funktionalitäten. Manche dieser Rückrufe sind zwingend erforderlich und werden von der Host Applikation zur Verfügung gestellt. Andere Rückrufe sind zusätzliche Feature und optional für die Host Applikation.

Rückruf Funktionen von Routing:
* **MessageReceivedFunctor** : Wird aufgerufen wenn eine Nodelevel Anfragenachricht empfangen wird. Nodelevel Anfragenachrichten ist eine eingehende Nachricht die für den Applikationslayer bestimmt ist und den Routingnode nutzt.

* **NetworkStatusFunctor** : Wird aufgerufen wenn eine neue Verbindung aufgebaut wird oder ein Verbindungsabbruch auftritt. Es zeigt die prozentuale Gesundheit einer Nodeverbindung an in Bezug auf andere valide Peernodes im Netzwerk oder kann alternativ als Routing Tabelle Gesundheit bezeichnet werden.

* **MatrixChangedFunctor** : Jedes Abwanderungsereignis das in einer Veränderung der Gruppenmatrix der Node resultiert löst einen einen Aufruf dieses Funktor aus.

* **RequestPublicKeyFunctor** : Als Teil des Verbindungsprozess zu einem Nicht-Client Peer muss Routing der öffentliche Schlüssel des Peer zur Verfügung gestellt werden. Das wird durch die PKI Infrastruktur erreicht. Der Rückruf stellt einen anderen Rückruf zur Verfügung (GivePublicKeyFunctor) welcher mit dem validen öffentlichen Schlüssel des verbindenden Non-Client Peer aufgerufen werden sollte.


* **HaveCacheDataFunctor** & **StoreCacheDataFunctor** : Werden aufgerufen um nach zwischengespeicherten Daten auf einer Zwischennode zu gucken, oder um diese dort zu speichern.

* **NewBootstrapEndpointFunctor** : Wird jedes Mal aufgerufen wenn eine Routing Node sich zu einem Peer verbindet dessen Endpunkte in der Lage sind eine Node urzuladen (bootstrap). Da das Netzwerk sehr dynamisch ist, ist das eine sehr wichtige Information für das erneute Verbinden zum Routing Netzwerk. Eine Liste dieser Endpunkte muss Routing bereitgestellt werden um ein Wiederverbinden zum Netzwerk zu ermöglichen.
