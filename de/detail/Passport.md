## Passport Überblick

Die Passport API bietet eine Selbstzertifizierende öffentliche Schlüssel Infrastruktur, PKI, unabhängig von den Anforderungen für ein zentrales Verzeichnis. Eine Variation an Schlüsseltypen sind verfügbar für eine verschiedene Aufgaben, während öffentliche Schlüssel, wie alle anderen Daten, an willkürlichen Orten im SAFE Netzwerk gespeichert werden. Authorisierte Funktionseinheiten können öffentliche Schlüssel vom Netzwerk abrufen um jegliche andere Funktionseinheiten zu validieren oder um zahlreiche andere Operationen anonym und sicher durchzuführen. Die API ist auf eine kryptographische Komponente angewiesen die Teil des [common](https://github.com/maidsafe/MaidSafe-Common/wiki) Projekt ist.

## Beschreibung

Die folgenden Dateien umfassen die öffentliche API von Passport.

* [passport.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/passport.h) beinhaltet die Passport Klassen Definitionen and diverse freie Funktionen die zusammen die Anforderungen für komplett sichere und anonyme Transaktionen/Kommunikation im Netzwerk erfüllen. Die Passport Klassen Mitglieder sind Identitäten die im Header definiert sind.
* [types.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/types.h).
* [types.h](https://github.com/maidsafe/MaidSafe-Passport/blob/master/include/maidsafe/passport/types.h) definiert Netzwerk Identitäten für Authentifizierung, speichern und abholen von Daten und sichere Kommunikation.


