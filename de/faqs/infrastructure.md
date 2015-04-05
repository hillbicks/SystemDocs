
# Infrastruktur

Dieser Teil der FAQs beschäftigt sich mit den Komponenten und Prozessen, die dem Netzwerk unterliegen.

### Benötigt das SAFE Netzwerk eine kontinuierliche Internetverbindung von alles Beteiligten?

Nein, das SAFE Netzwerk bewertet jeden Verbindungtyp und stellt sich auf die Ressourcen ein, die diese zur Verfügung stellen, um dem Fall der Unverfügbarkeit (durch Abschalten oder technisches Versagen) vorzubeugen.

Es ist jedoch kein Problem für kürzere Zeiten vom Netzwerk getrennt zu sein. Wenn ein Node allerdings für längere Zeitabschnitte nicht verfügbar ist, dann wird das den Service, den man vom Netzwerk erhält herabstufen.


### Was bedeutet dezentralisiert in dem Kontext vom SAFE Netzwerk?

Das SAFE Netzwerk, und im engeren Sinne, der Ansatz MaidSafe bedeutet vollständig dezentralisiert. Dieser Term wird extensiv in der tech community verwendet. Für das SAFE Netzwerk bedeutet dies folgendes:

1. Überhaupt keine Server
2. Keinerlei Information über Server Infrastruktur (keine DNS, keine timer servers...usw...)
3. Keinerlei Verbindung zu Server-basierten Netzwerken
4. Keine zentralisierten Datenstrukturen zwischen den Nodes
5. Niemand kontrolliert das Netzwerk
6. Null menschliche Kontrolle zum administrieren des Datenzuganges
6. Vollständig inclusiv (keinerlei Vermögen Benutzer zu sperren, oder Netzwerkzugang zu
   beschränken).
7. Grenzenloses Netzwerk: keinerlei Grenzen oder Kontrollen bezüglich der Erstreckung des
   Netzwerkes.


###Ist der Speicherplatz des Netzwerkes begrenzt?

Nachdem das Netzwerk kritische Masse erreicht hat, wird der Speicherplatz der Benutzer nur durch die Anzahl an Safecoins begrenzt sein, die sie besitzen bzw aufbringen wollen um ihre
Daten zu speichern.

Diese Safecoins können verdient werden, indem man dem Netzwerk Ressourcen zur Verfügung stellt, oder gekauft werden. Das SAFE Netzwerk benutzt den Prozess der Deduplikation [http://en.wikipedia.org/wiki/D...](http://en.wikipedia.org/wiki/Data_deduplication)
um die effizienteste Nutzung des Speicherplatzes zu gewährleisten. Da das Netzwerk aus den Ressourcen aller Nutzer besteht, kostet die Infrastruktur einen Bruchteil von existierenden Server-basierten Speicherplatz Anbietern.

###Wer bezahlt für den Speicherplatz?
Benutzer (Farmer) stellen dem Netzwerk deren ungenutzte Computerressourcen zur Verfügung. Es sind diese Ressourcen, die zum Speichern von Daten verwendet werden. Benutzer werden für diesen Vorgang vom Netzwerk mit Safecoins entlohnt.


###Wie lange werden meine Daten gespeichert?

Alle Daten sind im SAFE Netzwerk auf ewig gespeichert, es sei denn der Benutzer löscht diese. Daten, die lange Zeit unbenutzt bleiben, werden in ein Archiv abgelegt.


###Wie wird sichergestellt, dass es zu keinem Datenverlust kommt nachdem ein Benutzer offline geht?

Das SAFE Netzwerk hält automatisch ein Minimum von vier live Kopien für jedes chunk instand. Wenn Benutzer ihre Computer ausschalten, benachrichtigen deren Vault Manager (die Gruppe, die dafür zuständig ist, sich um Network Nodes zu kümmern) das Netzwerk. Daraufhin werden alle Daten chunks, die in diesem Data Manager gehalten wurden, an einem anderen Ort im Netzwerk neu kreiert. Dieser Prozess wird sehr schnell ausgeführt, in ungefähr 20 Millisekunden.


###Können Webseiten wie YouTube dezentralisiert werden?

Ja, alle Applikationen oder Web Services, die heute im Internet existieren, können durch das SAFE Netzwerk dezentralisiert werden.





###Wie geht das SAFE Netzwerk mit Sybil Attacken um?

Im SAFE Netzwerk müssen alle Anfragen mindestens von zwei Gruppen von Vaults prozessiert werden.Der MaidSafe Client leitet eine Anfrage an seine vier Data Managers, die die Anfrage verifizieren, basierend auf der Signature von dem Client. Die Anfrage wird dann an eine deterministisch selektierte Gruppe von vier anderen Vaults geleitet, die auch die Anfrage basierend auf der Signatur verifizieren.

Durch die determinisch selektierte zweite Gruppe von Data Managers werden Sybil Attacken verhindert, da es dem Angreifer nicht möglich ist Kontrolle über ein Vault auszuüben, indem er es umstellt.

Um dies zu umgehen, müsste es dem Angreifer möglich sein, spezifische Vaults im SAFE Netzwerk zu umstellen. Dies ist aber unmöglich, da es die Fähigkeit voraussetzt, effektiv Werte zu generieren, die wenn mit SHA-512 gehash, in hashes nahe einem bestimmten Punkt resultieren.




###Wie geht das SAFE Netzwerk mit Datenredundanz um, um sicher zu stellen, dass auf gemeinsam genutzte Daten weiterhin zugegriffen werden kann?

Jede Datei wird während des Verschlüsselungsprozesses (Self Encryption) verschlüsselt und in chunks zerteilt. Das Netzwerk erhält mindestens vier Kopien von jedem verschlüsselten chunk aufrecht. Dieser werden im Netzwerk relokalisiert wenn Nodes ausfallen, entweder durch technisches Versagen oder Ausschalten. Um mit diesem Churn umzugehen, ist das Netzwerk in der Lage sich extrem schnell global zu rekonfigurieren (20 milliseconds). Die chunks sind global verteilt um die Rubustheit zu erhöhen.


###Wie werden Daten gespeichert und abgerufen?

Daten werden gespeichert und abgerufen unter Verwendung des self-encryption Prozesses. Self encryption wird verwended um Daten durcheinander zu bringen und zu verschlüsseln, bevor diese an das Netzwerk gesendet werden. Dieser Prozess ist automatisch und passiert sofort. Wenn Daten auf der virtuellen Festplatte der Benutzer gespeichert werden, so werden diese in mindesten drei chunks aufgespalten. Eine Data Map wird für angelegt und für jeden chunk wird ein hash (ein spezifischer digitaler Fingerabdruck) angelegt, der in die Data Map geschrieben wird. Um die Sicherheit weiter zu erhöher, wird die Data Map selbst auch durch den self encryption prozess geschleust.

Jeder chunk wird dann verschlüsselt um zufällige, nicht-repetitive daten zu kreieren. Schlussendlich werden die chunks zusammen mit den originalen Hashes nochmals verschlüsselt. Der Output von jedem chunk wird dann in die Data Map hinzugefügt.

Die Data Map, mit den Hashes vor und nach der Verschlüsselung, wird benutzt um die Daten der Benutzer abzurufen und zu entschlüsseln, da der Verschlüsselungsprozess nicht reversibel ist. Die Daten werden entschlüsselt und zusammengefügt unter Bereitstellung des Benutzers PIN, Keyword und Passwort.




###Wenn Dateien in Chunks aufgeteilt werden, dann nimmt das Netzwerk einen Hash (digitalen Fingerabdruck) von jedem Chunk. Kan des originale Hash zu den Benutzern zurückverfolgt werden?

Kurz gesagt, nein. Chunks sind nicht an spezifische Benutzer gebunden. Anonymität (und Sicherheit, die für uns dasselbe darstellen) ist ein Grundprinzip des SAFE Netzwerkes. Grob gesehen, gibt es folgende Eigenschaften, die Anonymität herstellen.


* RUDP (Reliable UDP) verschlüsselt jede Nachricht hop-zu-hop wenn diese das Netzwerk durchqueren
* Der Routing Layer entfernt IP adressen nach hop1
* Das Speichern und Abrufen von Daten wird über einen Identifier ausgeführt, der nur dem Netzwerk bekannt ist und nicht an eine Person oder öffentlichen Namen gebunden ist
* Es existiert kein Server login und deshalb kein zentraler Punkt, der Informationen besitzt oder angegriffen werden kann
* Passwörter werden nicht über das Netzwerk versendet oder in ihm gespeichert
* Alle Nachrichten sind verschlüsselt und der Identifier vom Sender/Empfänger ist nicht als was der Benutzer einloggt. Der Identifier ist in einem verschlüsselten Paket gespeichert
