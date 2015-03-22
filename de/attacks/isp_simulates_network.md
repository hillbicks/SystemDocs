#Ein ISP simulitiert Netzwerk

##Angriffsbeschreibung

Bei diesem Angriff erstellt der ISP ein simuliertes or falsches Netzwerk. In diesem Netzwerk erhält der ISP die Daten die der Benutzer angefordert hat, speichert aber nicht die Daten die der Benutzer versucht hat zu speichern.

##Zweck des Angriffs

Der Angriff dient dazu den Benutzer zum Verlust seiner Daten zu zwingen, die unter Umständen wichtig für ihn sind, oder aber um das SAFE Netzwerk zu diskreditieren.

##Abwendung des Angriffs

In diesem Fall ist der "bootstrapping" SAFE Client oder Vault nicht in der Lage zu den bootstrap Vaults zu verbinden die in der Cache Datei aufgelistet sind. Diese Datei enthält IP, Port und den öffentlichen Schlüssel der bootstrap Vaults.

Da die bootstrap Session mit dem öffentlichen Schlüssel von irgendeinem bootstrap Vault verschlüsselt ist, wird der Verbindungsversuch scheitern. Der SAFE Client erkennt, dass es sich nicht um ein valides Netzwerk handelt.

##Verbesserte Angriffsbeschreibung

Eine Maschine eines ISP gibt vor, die Maschine eines Benutzers zu sein und fragt die bootstrap Informationen für den Benutzer an.

##Verbesserte Angriffsabwendung

In diesem Fall könnte die Maschine eines ISP die Information anfordern und sie dem Benutzer zurückliefern. Diese Information ist für den Benutzer verschlüsselt, so dass ein ISP nicht wirklich auf sie zugreifen kann (es sein er kann RSA 4096 knacken). Der Benutzer verbindet sich dann zum nächsten Vault. Wenn die nächsten Vaults Imitate sind, werden sie nicht dazu in der Lage sein die Verbindungsanfrage zu prozessieren, die Ende zu Ende verschüsselt ist. Daher schlägt auch dieser Angriff fehl.

Das ist nur zutreffend für SAFE Clients die sich noch nicht eingeloggt haben. Bei Erhalt des Anmelde Paketes verschlüsselt der SAFE Client die nächste Vault Anfrage a den bootstrap Vault. Auch diese Antwort wird verschlüsselt beantwortet und damit der Angriff verhindert.
