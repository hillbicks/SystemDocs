## Vault Überblick

Das SAFE Netzwerk besteht aus Softwareprozessen (Nodes), auch Vaults genannt. Diese Vaults verrichten viele Funktionen im Netzwerk und diese funktionalen Komponenten werden auch Personas genannt. Das unterliegende Netzwerk, wenn es mit [MaidSafe-Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) verlinkt wird, ist ein XOR Netzwerk und als solches können Nodes Nähe oder Verantwortung zu jeder anderen Node oder Element im Netzwerk ausdrücken, sofern die Node sich in relativer Nähe zum Ziel befindet. In dieser Zusammenfassung wird der Begriff **NAE** (Network Addressable Element/Netzwerk adressierbares Element) genutzt das sich auf alles mit einer Netzwerkadresse bezieht, inklusive Daten.

Die Vaults sind auf [MaidSafe-Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) angewiesen um Verantwortlichkeiten für NAE mittels der relevanten [API calls](https://github.com/maidsafe/MaidSafe-Routing/blob/master/include/maidsafe/routing/routing_api.h) zu berechnen, wie zum Beispiel

```C++
GroupRangeStatus IsNodeIdInGroupRange(const NodeId& group_id, const NodeId& node_id) const;
GroupRangeStatus IsNodeIdInGroupRange(const NodeId& group_id) const;
bool EstimateInGroup(const NodeId& sender_id, const NodeId& info_id) const;
```
Diese Aufrufe erlauben es uns das Netzwerk von der Perspektive jedes NAE für das wir verantwortlich sein könnten, zu berechnen. Es kann nicht oft genug erwähnt werden das der EINZIGE Weg um Verantwortlichkeit für einen NAE zu bestimmen der ist, das Netzwerk aus der Perspektive des NAE zu sehen. Wenn wir die Vektoren der Nodes sortieren von denen wir wissen und deren nächsten Nodes (als Gruppenmatrix bezeichnet) und wir nicht in den ersten K (Replikationszählung) Nodes sind, dann sind wir nicht für das NAE verantwortlich. Das ist ein fundamentaler Sachverhalt und die Wichtigkeit hiervon kann nicht oft genug betont werden.
Da das Netzwerk in Bezug auf Abwanderung und Vault Fähigkeiten sehr wechselhaft ist, muss das Vault Netzwerk individuelle Vaults messen und reporten und am wichtigsten, sicherstellen das alle Personas der Vaults ihre Aufgaben für das NAE für das sie verantwortlich sind auch ausführen. Um das zu ermöglichen wird das [Matrix change (Churn Event)](https://github.com/maidsafe/MaidSafe-Routing/wiki/Documentation#matrix-change-churn-event) Feature von Routing verwendet. Im Falle einer Abwanderung in einem gegebenen Netzwerksegment wird ein Matrix Change Event von Routing erstellt und dem Vault übergeben. Diese Obejkt enthält die Liste der alten und neuen Nodes in der Gruppenmatrix. Basierend auf dieser Information stellt es Helferfunktionen bereit, um bestimmte Informationen die sich auf jedes gegebene NAE beziehen, abzuleiten.
Für den Fall das die Node die das Abwanderungsevent erhält zu den ersten K Nodes gehört die am nächsten zur mitgelieferten NAE ist: Wenn ja, welche neuen Node(s) brauchen Informationen die in Verbindung zur mitgelieferten NAE stehen. Wenn nein, lösche alle gespeicherten Informationen die in Verbindung zur gegebenen NAE stehen.

Abwanderung, Duplizierung von Daten und Sicherstellung das alle Mitglieder einer Gruppe zustimmen, wird von einer Kombination aus Synchronisation, dem Akkumulator und Gruppennachricht gehandhabt. Das ist ein komplexe Reihe von Regeln die eine signifikante Aufmerksamkeit von Randfällen benötigen.


#Begrifflichkeiten die genutzt werden
**_Bitte beachte das diese Begrifflichkeiten ausgiebig genutzt werden und dieses Dokument unlesbar machen, wenn diese nicht komplett verstanden sind._**

* Sy - [Sync](#sync), diese Funktion synchronisiert Daten zwischen Nodes in der gleichen Gruppe (eng verbunden).
* Sr - [Sync](#sync), diese Funktion synchronisiert Resultate von Nachrichten zwischen Nodes in der gleichen Gruppe (eng verbunden).
* So - Send on/Senden bei, diese Funktion sendet die Nachricht weiter zur nächsten Persona.
* Ac - Accumulate/Akkumulieren, diese Funktion akkumuliert Nachrichten von vorherigen Personas (die Personas die die Nachricht zur aktuellen Persona senden).
* Fw - Firewall, diese Funktion stellt sicher das doppelte Nachrichten nicht verarbeitet werden und beantwortet diese Nachrichten mit einer kalkulierten Antwort. (wie z.B. erfolgreich:fehler:synchronisieren etc. und kann eine Nachricht inkludieren die Daten enthält)
* Uf - Update Firewall, diese Funktion wird die Firewall mit dem neu berechneten Rückgabewert und der optionalen Nachrichten auf den neuesten Stand bringen.
* **Fett** repräsentiert  Indirect Network Addressable Entities (Indirekte Netzwerk Adressierbare Einheiten) INAE
* _Kursiv_ repräsentiert Direct Network Addressable Entities (Direkt Netzwerk Adressierbare Einheiten ) DNAE
* ->>>> repräsentiert eine Gruppe Nachricht (kein Rückruf)
* -> repräsentiert eine direkte Nachricht (kein Rückruf)
* =>>>> repräsentiert eine Gruppennachricht(mit Rückruf)
* => repräsentiert eine direkte Nachricht (mit Rückruf)

### SAFE Netzwerk Identitäten
Das SAFE Netzwerk besteht aus vielen Datentypen sowie aus vielen Identitätstypen. Diese Typen werden in [MaidSafe-Passport](https://github.com/maidsafe/MaidSafe-Passport/wiki) beschrieben. Die Personas sind besonders fokussiert auf 4 dieser Identitäten und stellen sicher das angebrachte Einheiten die erzwungen Anforderungen dieser Identitäten erfüllen.

* SAFE (SAFE Netzwerk Anonyme ID) - Die Client Identität um unstrukturierte Daten zu manipulieren. Ein Client kann eine solche haben.
* PMID (Proxy Maidsafe ID) - Die Client Identität um unstrukturierte Daten sicher zu speichern. Ein Client kann mehrere davon haben.
* MPID (Maidsafe Public ID) - Die Client Identität um öffentlichen IDs erlauben (öffentliche Netzwerknamen, wie z.b. der Name einer Person oder Nickname) sichere Kommunikation zu ermöglichen. Ein Client kann mehrere davon haben.
* MSID (Maidsafe Share ID) - Die Client Identität um Gruppen von MPID's zu verwalten um Daten vertraulich zu teilen (strukturiert und unstrukturiert). Ein Client kann mehrere davon haben. Dieser Identitätentype hat aus Sicherheitsgründen keine NAE Halter.



### Vault Personas
Die Personas die von SAFE Vaults beutzt werden, fallen in zwei eindeutige Kategorien, nämlich Datenmanagement und Nodemanagement. Diese Kategorien definieren die Aktionen und Gruppierung der Nodes und deren Wissen der Umgebu und Nachrichten.
* Datamanagement Nodes sind verantwortlich für NAE spezifische Daten, normalerweise Pointer für Daten.
* Nodemanagement personas sind verantwortlich für eine Einheiten Persona (Node) and verwaltet die Aktionen und Anfragen von dieser Einheiten Persona.

Eingehende Nachrichten werden [demultiplext](https://github.com/maidsafe/MaidSafe-Vault/blob/master/src/maidsafe/vault/demultiplexer.h) um die Persona und den Datentyp für den sie gedacht sind zu identifizieren. Die Nachricht wird dann zu dieser Persona geleitet.

# Generelles Datenmanagement
___
###Nicht versionierte Daten

Datenteile im Netzwerk zu speichern erfordert Koordination und Zusammenarbeit von mehreren Personas. Ein Datenteil von den Daten eines Benutzers wird am SAFENode erstellt und wird von den DatenManagern verwaltet. DatenManager stellen sicher das verschiedene Kopien der Daten gespeichert werden und auf entfernten PmidNodes jederzeit verfügbar sind. Das folgende Diagramm zeigt einen Basisablauf des Datenspeicherprozess im Netzwerk.

![UnversionedData](./img/UnversionedData.png)


| Data Management| Node Management   | Nodes     |
| ---------------|:-----------------:| ---------:|
| [**DataManager**](#datamanager)    | [**MaidManager**](#MaidManager)| [_MaidNode_](#maidnode)  |
| [**VersionManager**](#versionmanager) | [**PmidManager**](#PmidManager)| [_PmidNode_](#pmidnode)  |

####`Put<Data>`
|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ |

####`Put<Unique>` _(a specialisation)_
|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]=>>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ |

####`Delete<Data>`
|  _MaidNode_ [So]=>>>> |[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>> * 4| [Ac, Fw] **PmidManager** [So, Sy] =>|[Ac, Fw]_PmidNode_ |

####`Get<Data>`
|  _MaidNode_ [So]=>>>> | [Ac, Fw] **DataManager** [So]=> * (# of live PmidNodes)|[Ac, Fw] _PmidNode_ |


##Anatomie einer Manager Persona mit nicht versionierten Daten.

Alle Personas folgen einem ähnlichen Muster, sie akkumulieren, firewall, handhaben und senden bei Nachrichten und synchronisieren ihre Ergebnisse woraufhin nach Zustimmung die Ergebnisse einer Operation in einer Datenbank aufgezeichnet werden. Die Resultate oder Werte die pro Operation gespeichert werden weichen bei anderen Personas ab, sonst aber nur sehr wenig.

In der Praxis wird von vornherein davon ausgegangen das Nodes im Netzwerk nicht vertrauenswuerdig sind, jedoch sind kooperierende/zusammenarbeitende Nodes essentiell für die kontinuirliche Stabilität und Gesundheit des Netzwerk. Um sicherzustellen das beide Bedinungen erfüllt sind wenn sie etwas von einer INAE empfangen, wird die Gruppe von Nodes, unter dem Deckmantel einer spezifischen Persona, die das Zielelement umgibt, für die Validierung der Anfrage verantwortlich. Synchronisation zwischen den Gruppen erlaubt jeder das Resultat des anderen aufgrund einer vorherbestimmten Nummer zu akkumulieren um die Anfrage zu erfüllen. Der Prozess erkennt von sich aus Nachrichtenverfälschung oder Möchtegern-Hacker und mit einem Ranking Mechanismus werden Serientäter in eine Netzwerkisolation gezwungen.

Wenn von einer DNAE empfangen wird, akkumuliert jede Persona nur auf einer einzelnen Nachricht. Das wird ermöglicht da eine Verbindung zwischen den Nodes existiert und das sowohl Routing als auch rUDP die Identität der anderen Node bestätigt auf eine kryptographische sichere Art bestätigt haben.

Die Vielfalt von Nicht-Daten Nachrichten wird eindeutig von jeder Persona gehandhabt.

___

###Versionierte Daten

Speichern eines versionierten Datenteils wie z.b. ein Verzeichnisversion wird von unterschiedlichen Personas gehandelt, basierend auf den Privatssphäreneinstellungen des Verzeichnis das der Nutzer vorgenommen hat. Privatssphäreneinstellungen umfassen private, öffentliche und geteilte versionierte Daten. Verschiedene Identitäten werden tätig, basierend auf den Privatssphäreneinstellungen die mit den Verzeichnissen verbunden sind. Auch wenn unterschiedlich versionierte Daten erstellt und von unterschiedlichen Personas weitergeleitet werden, so wird alles durch den VersionManager gespeichert und verwaltet.
Das folgende Diagramm zeigt einen grundlegenden Ablauf des versionierten Datenspeicherprozesses im Netzwerk, basierend auf den Privatssphäreneinstellungen.

![VersionedData](./img/VersionedData.png)


####`PutVersion<Data>`
`Put <Data>`|  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So,Sy]->>>> * 4|[Ac, Fw] **PmidManager** [So, Sy]=>| [Ac, Fw]_PmidNode_ | +
* |  _MaidNode_ [So]=>>>>|[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw, Sy, Uf] **VersionManager** |


####`GetVersion<Data>`
* |  _MaidNode_ [So]=>>>> |  [Ac, Fw] **VersionManager** | +
* `Get<Data>` |  _MaidNode_ [So]=>>>> | [Ac, Fw] **DataManager** [So]=> * (# of live PmidNodes)|[Ac, Fw] _PmidNode_ |


#####`DeleteVersionTillFork<Data>`
* `Delete<Data>` |  _MaidNode_ [So]=>>>> |[Ac, Fw] **MaidManager** [So, Sy]->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>> * 4| [Ac, Fw] **PmidManager** [So, Sy] =>|[Ac, Fw]_PmidNode_ | +
* |  _MaidNode_ [So]=>>>> | [Ac, Fw, Sy, Uf] **VersionManager** |

##Anatomie einer Manager Persona mit versionierten Daten

Diese VM Personas müssen alle der Anfrage zustimmen wenn sie empfangen wird. Um das zu erreichen akkumulieren die VM Personas die empfangenen Nachrichten von vorherigen Personas. Sobald das erwartete Limit erreicht ist, wird die Anfrage mit anderen VM Personas in der Gruppe synchronisiert. Jede VM Persona die die erwartete Anzahl an Syncnachrichten erhält  versucht die Anfrage auszuführen. Die Anfrage auszuführen ist gefolgt von der Rückführung des Resultats der Aktion zum Akkumulator und dem Aufruf von Antwort Funktoren die Anfragen von vorherigen Personas zugeordnet sind. Wieder versucht `Get` lediglich die selbst errechnete Antwort zurückzuliefern um schnell zu sein, aber alle mutierten Aufrufe verlangen Zustimmung von allen VersionManager Personas die im Aufruf involviert sind. Das ist ein entscheidender Unterschied zu den nicht versionierten DataManagern. Der Unterschied kommt weil diese Personas die endgültige Antwort auf die Anfrage sind und Synchronisation für Zustimmung genutzt wird, anstatt die Antwort selbst zu errechnen und es dem Anfrager zu überlassen dem zuzustimmen.


## Generelles Kommunikations Management
| Node Management      | Node      |
| ----------------------|----------:|
| [**MpidManager**](#mpidmanager)    | [_MpidNode_](#mpidnode)  |



---
##Daten Management Personas
Daten Management Personas sind dafür verantwortlich Verbindungen zwischen NAE Schlüsseln und Adressen NAE zu verwalten die den Inhalt des fraglichen Schlüssel beinhalten. Diese Vault Personas können als Manager von Pointern zu Daten gesehen werden sowie Datenlebenszeit und Replikations Management, mit der Fähigkeit Netzwerkabwanderung zu widerstehen.

###Daten Manager<a id="datamanager"></a>
Die **Datenmanager** Persona verwaltet Pointer zu nicht versionierten Daten, wie Dateiinhalt, Netzwerkschlüssel wie definiert in [passport](https://github.com/maidsafe/MaidSafe-Passport/wiki) und alle anderen 'statischen' Datenelements.

### Container Struktur
Uses a unique ManagerDb
Verwendet eine eindeutige ManagerDb

* Schlüssel : Datenschlüssel + Typ
* Wert: DataManagerWert Objekt (in serieller Reihenfolge erstellt)

### Nachrichten Eingang
* `Put<Data>` Die Nachricht ist von einer SAFEManager Gruppe (von K) empfangen worden und wird akkumuliert. Die Datenbank wird auf die Existenz des Schlüssels überprüft. Wenn er existiert wird der Teilnehmer Zähler erhöht.
Wenn kein Eintrag existiert, dann wird jeder DataManager dieser Gruppe der am nächsten zum NAE ist der gespeichert wird, eine verbunden Node als PmidNode zum Speichern auswählen (wenn die Nachricht einen Datenhalter Hinweis hat, dann wird der nächste der DatenManager von diesen NAE versuchen hier zu speichern). Um einen Datenteil zu speichern wird eine PUT<Data>`(Schlüssel, Inhalt) Nachricht zum PmidManager gesendet der für die selektierte PmidNode verantwortlich ist.
* * | **MaidManager** ->>>> | [Ac, Fw]**DataManager**[So, Sy]->>>>|

* `Delete<Data>` Die Nachricht ist von einer SAFEManager Gruppe (von K) und wird akkumuliert. Die Datenbank wird auf die Existenz des Schlüssels überprüft. Wenn er existiert wird der Teilnehmer Zähler verringert. Wenn der Zähler Null erreicht (falls das jemals passiert) dann wird ein Delete<data>(key) zu allen PmidManagern gesendet um alle Daten zu löschen.
* * | **MaidManager** ->>>> | [Ac, Fw] **DataManager** [So, Sy]->>>>|

* `Get<Data>` Diese Nachricht wird nicht akkumuliert, nur "fire-walled". Diese Nachricht kann von jeder Persona kommen, sollte diese Persona jedoch aufgrund von caching nicht allzu häufig erreichen. Wenn die Nachricht hier ankommt, dann sendet der DataManager ein GET<Data> direkt zur PmidNode und erhält die Daten und es mittels reply_functor zurücksendet übergeben von [routing](https://github.com/maidsafe/MaidSafe-Routing/wiki).

* * |  _MaidNode_ ->>>> | [Ac, Fw] **DataManager** | (firewall)
* `Node Status Veränderung`
| **PmidManager** ->|[Ac, Fw] **DataManager** [So, Sy]->>>> |


### Nachrichten Ausgang
* `Put<Data>` Wenn ein Datenelement zum ersten gespeichert wird oder ein DataManager verlangt das ein anderen PmidNode die Daten speichert (weil PmidNodes ausfallen, ausgeschaltet werden oder anders Daten verlieren) wird dieser Nfs Aufruf gemacht um Daten zu speichern.
* * | **DataManager** [So, Sy, Uf]->>>> | **PmidManager** |

* `Delete<Data>` Wenn der Teilnehmerzähler sich Null nähert oder die PmidNode als nicht länger verantwortlich für ein Datenelement angesehen wird, wird dieser Nfs Aufruf getätigt.
* * | **DataManager** [So, Sy, Uf]->>>> | **PmidManager** ->|

* `Get<Data>` Als Antwort zum Empfang eines Get<data> wird diese Node versuchen die Daten von irgendeiner Live PmidNode abzuholen und wird Daten mit Hilfe der Routing reply_functor zurück zum Anforderer schicken die mit der Anfrage geliefert wurde.
* * | **DataManager** => * live PmidNodes| _PmidNode_ |

### Datenintegritätschecks
Als Antwort auf ein Abwanderungsereignis (erkannt durch einen Nodestatus Veränderung, siehe unten), wird der DatenManger ein zufälliges Datenstück erstellen und dieses an jede PmidNode senden mit der Aufforderung diese Daten anzufügen und die Daten noch einmal zu hashen und mit dem neuen Wert zu antworten. Das erlaubt dem DataManager zu evaluieren das alle PmidNodes die selben Daten vorhalten (nicht korrumpiert oder verloren)
* * | **DataManager** [So, Sr]=>>>> * 4| **PmidManager** ->| _PmidNode_ |


### Version Manager<a id="versionmanager"></a>
Der **VersionManager** verwaltet versionierte Daten. Das beinhaltet momentan Daten die in [StructuredDataVersions](https://github.com/maidsafe/MaidSafe-Private/blob/master/include/maidsafe/data_types/structured_data_versions.h) definiert werden können. Private Datenverzeichnisse, privat geteilte Verzeichnisse und öffentliche Verzeichnisse werden momentan in dieser Struktur verwaltet. Diese Persona ist nicht limitiert Versionierung zu verlangen und kann daher mit Leichtigkeit für strukturierte Daten von vielen verschienden Typen verwendet werden.

### Container Struktur
Verwendet eine eindeutige ManagerDb
* Schlüssel: Datenschlüssel + Typ + EinheitenID
* Wert : StrukturiertesDatenVersions Objekt  (in serieller Reihenfolge erstellt)

### Nachrichten Eingang
* `PutVersion<Data>`
* * | **MaidManager** =>>>> | [Ac, Fw] **VersionManager** [Sy, Uf] |
* `DeleteBranchTillFork<Data>`
* * | **MaidManager** =>>>> | [Ac, Fw] **VersionManager** [Sy, Uf] |
* `Get<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **VersionManager** |
* `GetBranch<Data>`
* * |  _MaidNode_ =>>>> |[Ac, Fw] **VersionManager** |

### Nachrichten Ausgang
 [Keine] Alle Eingänge sind Rückrufe

### Node Management Personas
Node management personas, verwalten Einheiten personas (nodes).

### Maid Manager<a id="MaidManager"></a>
Die **MaidManager** Funktion ist es die MaidNode zu verwalten. Das bedeutet das sichergestellt sein muss der der Client einen Account hat und mindestens eine registrierte PmidNode.

### Container Struktur
Benutzt die AccountDb

* Schlüssel: Hash vom Datenschlüssel (hashed um die Daten des Client zu schützen)
* Wert : Anzahl der Kopien (int32) : absoluter Aufwand (int32)


### Header Struktur

* Absolute Datengröße gespeichert
* Verfügbarer Platz

### Nachrichten Eingang
* `PUT<Data>` Ein Client kann anonyme SAFE Pakete im Netzwerk speichern was den Account erstellt. Daraufhin muss die SAFENode eine PmidNode registrieren was mit ein Vault Registrierungspaket geschieht, welches von SAFE und PMID privaten Schüsseln  (auf die der Client Zugriff haben muss) signiert werden. Der individuelle SAFEManager wird die PmidManager Gruppe beim Start, Abwanderungsereignis und wenn der Client nur noch wenig Speicherplatz zur Verfügung hat anfragen, um sicherzustellen das die PmidNode genügend Speicherplatz hat. Bei Erfolg kann die SAFENode die Daten zum relevanten DataManager senden und die Kosten für die Bezahlung abrufen. Bei Erhalt der Kosten wird die Intention die Einträge in der Datenbank zu speichern mit den anderen SAFEManagern synchronisiert. Die MergePolicy wird in diesem Fall den Schlüssel zur Datenbank hinzufügen und/oder die Anzahl der Kopien und totale Kostenfeld zu inkrementieren.

* * |  _MaidNode_ ->>>> | [Ac, Fw]**MaidManager** [So]|

* `Delete<Data>` Der MaidManager wird den Accounteintrag des SAFE Client Account nach einem passenden PUT des Datenschlüssel durchsuchen. Bei Erfolg wird der SAFEManager die Anzahl der PUT Teile für diesen Schlüssel reduzieren. Wenn mehrere Teile in diesem Schlüssel gespeichert sind, werden die Kosten reduziert indem ein Durchschnitt der totalen Kosten für diesen Schlüssel genommen wird. Löschen liefert immer einen Erfolg zurück da der Client nur seine eigene Zeit in einer Attacke dieser Art verschwendet.
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `PutVersion<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `DeleteBranchTillFork<Data>`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |
* `Register/Unregister vault`
* * |  _MaidNode_ =>>>> | [Ac, Fw] **MaidManager** [So] |

### Nachrichten Ausgang
* `PUT<Data>`
* * | **MaidManager** [So] ->>>> | **DataManager** |
* `Delete<Data>`
* * | **MaidManager** [So] ->>>> | **DataManager** |
* `PutVersion<Data>`
* * | **MaidManager** [So] =>>>> | **VersionManager** |
* `DeleteBranchTillFork<Data>`
* * | **MaidManager** [So] =>>>> | **VersionManager** |
* `GetPMIDHealth`
* * | **MaidManager** [So] =>>>> | **PmidNode** |

### Pmid Manager<a id="PmidManager"></a>
Die PAH (Pmid Account Halter) Funktion dient der Verwaltung des Pmid Client. Das bedeutet das sichergestellt wird das der Client einen Account hat und einen akuraten Eintrag über Datenelemente die vorhanden und verloren sind (frühre Version von Rang)

### Container Struktur
Nutzu die AccountDb

* Schlüssel : Datenschlüssel
* Wert : Größe (int32)

### Header Struktur

* Totale Datengröße gespeichert
* Totale Datengröße verloren
* Verfügbarer Platz

### Nachrichten Eingang

* `Put<Data>` Speichert Daten auf der PmidNode und aktualisiert die gespeichterten Zählerstand für diese PmidNode
* * | **DataManager** =>>>> | [Ac, Fw] **PmidManager** |
* `Delete<Data>` Sendet Löschungen zur PmidNode und entfernt vom gespeicherten Zählerstand für diese PmidNode
* * | **DataManager** ->>>> | [Ac, Fw] **PmidManager** |
* `Get<Data>` Rückgabe von Daten oder Fehlern
* * | **DataManager** =>>>> | [Ac(1), Fw] **PmidManager** |
* `Lose<Data>` Sendet Löschungen zur PmidNode und fügt zum Zählerstand für Verloren hinzu.
* `GetPMIDHealth`
* * | **MaidManager** =>>>> | [ **PmidManager** | (keine Fw oder Ac, beantwortet jeden Aufruf )
### Nachrichten Ausgang
* `SendAccount` Das wird zu einer _PmidNode_ gesendet es der Gruppe erneut beitritt. Das wird vom **PmidManager** bei Empfang eines Abwanderungsereignis erkannt. Der Account ist in diesem Fall alle Daten die _PmidNode_ hätte speichern sollen und wird keine gelöschten oder verlorenen Daten einschliessen die aufgetreten sind während die _PmidNode_ offline war.
* *   | **PmidManager** [So]-> | **PmidNode**


### Mpid Manager<a id="mpidmanager"></a>
Die MPAH (Mpid Account Halter) Funktion dient zur Verwaltung des Mpid client. Das bedeutet das sichergestellt wird das der Client einen Account hat und eine Blackliste/Whiteliste wenn sie vom Client benötigt wird. Diese Persona wird ausserdem Nachrichten vorhalten die für einen Mpid Client bestimmt sind der grade offline ist.

### Container Struktur
Benutzt die AccountDb

* Schlüssel : MpidName  + SenderMpidName + NachrichtenID
* Wert : Nachrichteninhalt

Diese Datenbank sollte nur Einträge enthalten während  _MpidNode_ offline ist.

### Header Struktur

* whitelist
* blacklist


### Nachrichten Eingang

* `Message`
* * | _MpidNode_ =>>>> | this**MpidManager** [So]|
* * | **MpidManager** =>>>> | this**MpidManager** [So]|
* `AddToWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `WhiteList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(antworte mit whitelist)
* `AddToBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `BlackList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(antworte mit blacklist)

### Nachrichten Ausgang
* `Message` Bei Erhalt einer Nachricht werden die Listen überprüft und die Nachricht wird weiter gesendet. Wenn die Node offline ist dann wird die die Nachricht lokal gespeichert, bei Empfang eines Abwanderungsereignis wird die offline List auf Nachrichten überprüft und diese werden an den Client gesendet.
* * | this**MpidManager** [So] -> | _MpidNode_ | (kann von der Datenbank kommen)
* * | this**MpidManager** [So] =>>>> | **MpidManager** [Sy]| (live Nachricht  von entfernter _MpidNode_)

## Einheiten personas
### Pmid Node<a id="pmidnode"></a>

### Nachrichten Eingang

* `Put<Data>`
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**
* `Delete<Data>`
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**
* `Get<Data>`
* *   | **DataManager** => |[Ac, Fw] **PmidNode**
* `SendAccount` Das wird zu einer  _PmidNode_ wenn es der Gruppe wieder beitritt. Das wird vom **PmidManager** bei Empfang eines Abwanderungsereignis erkannt. Der Account ist in diesem Fall alle Daten die der _PmidNode_ hätte speichern sollen und wird keine gelöschten oder verlorenen Daten einschliessen die aufgetreten sind während _PmidNode_ offline war.
* *   | **PmidManager** -> |[Ac, Fw] **PmidNode**


### Nachrichten Ausgang

[Keine]

### Maid Node <a id="maidnode"></a>

### Nachrichten Eingang

[Keine]

### Nachrichten Ausgang

* `Put<Data>` Liefert Fehlercode und PmidHealth zurück
* * |  _MaidNode_ [So]=>>>>| **MaidManager** |
* `Delete<Data>` Liefert Fehlercode zurück
* * |  _MaidNode_ [So]=>>>>| **MaidManager** |
* `Get<Data>` Liefert Fehlercode zurück und Daten (wenn erfolgreich)
* * |  _MaidNode_ [So]=>>>>| **DataManager** |


### Mpid Node<a id="mpidnode"></a>

### Nachrichten Eingang

* `Message`
* * | _MpidNode_ =>>>> | this**MpidManager** [So]|
* * | **MpidManager** =>>>> | this**MpidManager** [So]|
* `AddToWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromWhiteList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `WhiteList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(antworte mit whitelist)
* `AddToBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `RemoveFromBlackList`
* * | _MpidNode_ ->>>> | this**MpidManager** [Sy]|
* `BlackList`
* * | _MpidNode_ =>>>> | this**MpidManager** [Sy]|(antworte mit blacklist)

### Nachrichten Ausgang
[Keine]

# <a id="sync"></a>Synchronisation

Das Netzwerk befindet sich in einem konstanten Zustand des Wandels, in dem Nodes kontinuirlich erscheinen und wieder verschwinden. Das erzwingt einen Statusveränderungsmechanismus der in der Lage ist in Echtzeit  zu erkennen welche Nodes für welche Daten verantwortlich sind und Abwanderungsereignisse in der Nähe einer Node zu erkennen. Mit Hilfe von [SAFE-Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) kann das Netzwerk Veränderungen der Nodes sehr schnell erkennen. Routing stellt ebenfalls einen Mechanismus zur Verfügung der mit großer Präzision garantieren kann welche Nodes für welche NAE verantwortlich sind (inklusive der Node die das Netzwerk abfragt)

Um ein Datenelement aufzulösen, sei es ein `Account transfer` oder eine `ungelöste Aktion`, dann verlangt die Node eine Mehrheit von Nodes in der Nähe die bezogen auf das Element zustimmen. Da es mehrere Abwanderungsereignisse während eines Transfers solcher Daten geben kann, erwartet die Node solch mehrfache Abwanderungsereignisse selbst zu handhaben. Um das mit jedem Abwanderungsereigniss zu machen müssen die alte und die neue Node gehandhabt werden indem überprüft wird ob die alte Node ein Datenelement gegeben hat und ersetzen diese node_id mit der neuen node_id. Wenn das Datenelement die neue Node nicht enthalten hat, es aber sollte (z.B. die neue Node wäre für ungelöste Elemente verantwortlich gewesen) dann wird die neue node_id als Datensender hinzugefügt. Um das ein wenig einfacher zu machen, alle synchronisierten Daten haben die aktuelle Node als ein 'gesehen' Element, das hilft dabei Größe der Mehrheit die nötig ist. Wenn eine Node sich selbst zu ungelösten Daten in dieser Art hinzufügt, dann fügt es sich selbst am Ende des Containers hinzu. Das hindert die Node daran dieses Element in einer synchronisierten Nachricht zu senden.

Wenn eine Node eine synchronisierte Nachricht sendet, dann sendet sie einen eindeutigen Identifikator und seine eigene ID mit den ungelösten Daten. Jede empfangene Node erstellt einen Container der node_id und entry_id zusammen mit den ungelösten Daten. Wenn die Größe des Containers 0.5 * die Größe der nächsten Node +1 ist, dann sind die Daten gelöst und werden in den Datenspeicher geschrieben.

Es gibt zwei Typen von Daten welche kumulative Daten (von einer Gruppe) erforden um gelöst zu werden:

* `synchronisieren` Das sind Daten die bisher noch nicht synchronisiert wurden (ungelöst), aber diese Node hat sich selbst `gesehen`. Diese Daten enthalten die Aktionen die mit den Attributen notwendig sind um diese Aktion durchzuführen; dies kann als Funktion (Nachrichtentyp) gesehen werden und Parameter können angewandt werden.
* `account transfer`, dieser Datentyp ist gelöst und von der Node gespeichert. Es wird als Datenbankwert gesendet der gespeichert wird.


##Synchroniniseren um Aktionen zu validieren

Synchronisieren wird genutzt um sicherzustellen das alle Node in der selben Gruppe die fuer ein NAE verantwortlich sind sich über die Daten einig sind, in Relation zu der NAE die die Daten vorher gespeichert (oder verarbeitet) hat. Nodes erreichen das indem sie einen Container aus Daten erstellen der gelöst wird. Für solche Daten kann ein ungelöster Eintrag zum Container hinzugefügt werden, wenn :
* die Nachricht wird von der vorherigen Persona empfangen
* ein ungelöster Eintrag wird von einer ähnlichen Persona (in der Gruppe) für eine Ziel empfangen für das die Node verantwortlich ist.

Der ungelöste Eintrag der mit den Daten verbunden ist wird gelöst sobald die Anzahl an ungelösten Einträge die für diese Daten empfangen wurden (von den vorherigen und ähnlichen Personas) eine erwartete Anzahl erreicht. Eine Node die einen Eintrag gelöst hat kann am Senden zu anderen nodes, die als verantwortlich betrachtet werden, beteiligt werden, allerdings NUR wenn die Node die Daten von einer vorherigen Persona erhalten hat.
Durch die Nutzung von Routing wird Verantwortlichkeit bestätigt, eine **manager node** wird alle vorgehaltenen Daten überprüfen um sicherzustellen das es immer noch für die Daten verantwortlich ist (mit Hilfe der Routing API). Die Node überprüft anschliessend welchen Daten die neue Node haben sollte (wenn überhaupt Daten da sein sollten). Die Daten werden dann als `ungelöste Daten` zur neuen Node gesendet.

Diese Prozess geht weiter und jedes Mal wenn Daten gesendet werden, wird ein Zähler inkrementiert. Wenn dieser Zähler einen Parameter erreicht (inital 100) werden die Daten als unlösbar betrachtet und entfernt. Wenn die Daten gelöst werden, werden sie nicht von der Liste entfernt da es weitere Nodes geben könnte die zur Peerkomponente hinzugefügt werden (Wir lösen bei einer Mehrheit und  erwarten einen einstimmige Anzahl an Nodes die hinzugefügt werden und möchten keine neuen ungelösten Einträge erstellen). Wenn es ein komplettes Set von Peers gibt die die Daten gesendet haben (einstimmige Vereinbarung), dann wird der ungelöste Eintrag entfernt.

### Account Überführung

Manager Personas speichern die Informationen für Nodes/Daten liefern deren Funktionalität normalerweise basierend auf kumulativer Entscheidungsfindung, bei der eine Gruppe von nahen Nodes kooperieren um die gewünschte Funktionalität zu erreichen. Augenscheinlich sind richtige Entscheidungen zu einem großen Teil davon abhängig wie gut die Gruppenmitglieder synchronisiert sind. Die dynamische Natur des Netzwerk, in der eine Node innerhalb der Gruppe vielleicht ein oder austritt, macht einen Bedarf nach einem effizieten Mechanismus notwendig um sicherzustellen das jedes Mitglied einer Gruppe mit anderen Gruppenmitgliedern synchronisiert ist and die aktuellsten Daten vorhält für jede Node/Daten für die sie verantwortlich ist.

In dem Moment in dem eine Node einer Gruppe beitritt, werden ihr Account Übertragungsnachrichten gesendet, seine eigene Adresse wird zu den ungelösten Account Übertragungs Einträgen hinzugefügt (es ist die nächste gruppe -1 Node) um sicherzustellen das die Gruppengrößen Berechnung sich nicht verändern, obwohl die Gruppengröße nicht gleich groß sein kann da diese Node ein jetzt fehlende Node in dieser Gruppe ersetzt.

Der Account Übertragungs Eintrag ist ein Datenbankeintrag und verlangt Übereinstimmung und schreibt dann direkt in die Datenbank.

Zusätzlich werden ungelöste Daten dieser Node zur Verfügung gestellt und diese Node fügt abermals seine eigene ID den Daten hinzu. Die Node synchronisiert diese Daten jedoch nicht zurück zur nächsten Gruppe da alle Node das bereits erldigt haben als sie die neue Node beim Abwanderungsevent aufgenommen haben. Die ungelösten Daten sollten jetzt durch den oben genannten Prozess gelöst werden.



### Akkumulator

Wenn Nachrichten von einer Gruppe von Nodes empfangen werden, verlangt das Netzwerk Authorität um die Nachrichtenzu validieren. Dies wird erledigt imdem Nachrichten akkumuliert werden wenn sie ankommen, nachdem überprüft wurde das sie von einer Gruppe kommen die für das Senden der Daten verantwortlich ist (validiert durch die Routing API) und akkumulieren der Nachrichten bis wir mindestens eine Nahe Gruppen -1 Nachricht erhalten, wenn die Nachricht von einer **manager node** Gruppe kommt. Nachrichten die von einer DNAE Node kommen akkumulieren auf dieser einfachen Nachricht. Diese Node sind verifizierbar da sie verbunden sind und eine Aktion durchgeführt haben werden um zu validieren das sie berechtigt sind die Nachricht zu senden.

### Firewall

Wenn eine Nachricht akkumuliert ist, wird sie zur Firewall hinzugefügt. Diese Firewall wird jede eingehende Nachricht überprüfen und mit dem aktuellen Fehlercode antworten (und eine Nachricht senden, wenn das von diesen Nachrichtentyp verlangt wird). Der Fehlercode kann in Fällen `synchronisieren` in denen die akkumulierende Node genug Nachrichten akkumuliert hat um sicherzustellen das sie valide sind , aber noch keine Entscheidung darüber treffen kann wie die Nachricht zu beantworten ist (es kann notwendig sein zu synchronisieren oder sogar eine Nachricht zu einer Persona zu senden um weitere Informationen anzufordern)

### Caching

Caching ist ein sehr wichtiger Akspekt eines verteilten Netzwerk. Cachedaten werden wahrscheinlich im Arbeitsspeicher und nicht auf der Festplatte gespeichert. Momentan sind zwei caching Mechanismen definiert bei dem Vaults bleiben werden. Die folgenden Sektionen präsentieren eine generelle Diskussion, für eine detailierte Beschreibung der Implementierung siehe :
[Caching](https://github.com/maidsafe/MaidSafe-Vault/wiki/Caching) .

### Opportunistisches Caching

Opportunistisches caching ist immer ein first in first out (FIFO) Mechanismus. Als Antwort auf ein `GET` wird jede Node in der Kette jegliche cachebaren Daten (wie selbst Überprüfung oder unveränderbar) zu seiner FIFI hinzufügen. Das hat viele Vorteile:

* Mehrfache Anfragen für die selben Daten werden automatisch beschleunigt.
* Denial of service Attacken werden extrem schwierig da die Daten einfach den Anfrager der Daten umzingeln wenn er ununterbrochen danach fragt.
* Wichtige Daten sind schneller wenn sie wichtig sind und verlangsamt andernfalls, was eine gute Verwendung von Netzwerk Resourcen darstellt.
* Nahe Nodes können einfach temporäre Speicher von Daten werden was für SystemDesigner viele Vorteile darstellt.
* Wenn z.B. eine Webseite auf einem öffentliche Share platziert ist, dann wird diese Seite schneller laden wenn mehr Besucher auf sie zugreifen. Dieses Vorgehen ist logischer als das heutige Vorgehen bei dem Webseiten Gefahr laufen überlastet zu werden und zu crashen.

### Deterministisches Caching

Diese Art von caching ist noch nicht implementiert.
Deterministisches Caching erlaubt es dem Netzwerk die Anzahl an Nodes die Daten vorhalten und Pointer zu Daten effektiv zu erweitern wenn auf die Daten von einer großen Anzahl an Nodes oder Usern zugegriffen wird. Das erlaubt es dem Netzwerk mit extrem beliebten Inhalten umzugehen die z.b. eine microblogging Technologie repräsentiert die extrem populäre Accounts hat.

Zusätzlich zu diesem Cachingtyp erlaubt das Netzwerk Subskriptionslisten, in der Identitäten registriert werden können um über eine Veränderung alarmiert zu werden die die User verfolgen möchten, wie z.b. eine microblogging Seite oder eine populäre Newsseite, etc.
### NFS Policies

Die API zum Vault Netzwerk erfolgt mittels Nfs Policies and diese können hier gefunden werden [nfs library](https://github.com/maidsafe/MaidSafe-Network-Filesystem).

### Ranking

Nodes die sich unpassend benehmen müssen vom Netzwerk erkannt und entsprechend behandelt werden, genauso wie sehr hilfreiche und sich angepasste Nodes entsprechend mit größerem Respekt von anderen Nodes behandelt werden sollten. Um dies zu erreichen wird ein Ranking Mechanismus eingesetzt. Dieser Ranking Mechanismus im Vault Netzwerk handhabt momentan Nodes die Daten verlieren oder korrumpieren. Das kann aus vielen verschiedenen Gründen passieren, wie z.b:

* Schlechte Bandbreiten Leistungsfähigkeit
* Langsame CPU
* Unregelmäßige Verfügbarkeit
* Festplatten Korruption
* Festplatte an der sich jemand zu schaffen macht (Benutzer löschen Systemdaten)
* Und vieles mehr

Anstatt jedes Element einzeln zu messen, was sehr komplex und aufwendig wäre, misst das Netzwerk stattdessen die Anzahl an Daten die eine Node speichern kann und vergleich das mit den Daten die sie verliert. Diese Zahlen zusammen erlauben eine Berechnung über den verfügbaren Platz und seine Wertigkeit, mit einer hohen Rate an verlorenen Daten zu gespeicherten Daten die den Vault weniger wertvollen machen.
