# Geburtstagsparadoxon/Sybil Attacke

##Angriffsbeschreibung

Bei diesem Angriff flutet ein Angreifer das Netzwerk mit Vaults die er kontrolliert, um einen einzelnen Vault mit 3 oder mehr bösartigen Vault zu umgeben, um so die Kontrolle über den Vault zu erhalten.


##Sinn des Agriffs

In diesem Fall könnte ein Angreifer das Löschen von Datenteilen anfordern, indem er sich als Data Manager der kontrollierten Vaults ausgibt. Das könnte dazu führen, dass die Halter der Daten die Datenteile als Antwort auf eine scheinbar legitime Anfrage löschen und Zugriff auf die Daten von legitimen Benutzern verhindern.

Obwohl es nicht möglich ist die bösartigen Vaults ganz bewusst um einen gewünschten Punkt im SAFE Netzwerk zu platzieren. Mit rund 0,8% der Netzwerk Vaults unter temporärer Kontrolle des Angreifers ist es aber wahrscheinlich, dass er mindestens einen Vault im SAFE Netzwerk umgeben hat und somit Kontrolle über diesen Vault ausübt und Quorum dieser falschen Aktionen erreicht.

##Abwendung des Angriffs

Das SAFE Netzwerk erfordert, dass alle Anfragen von mindestens zwei Gruppen von Vaults prozessiert werden.

Ein SAFE Client reicht eine Anfrage zu 4 seiner Data Manager weiter, die die Anfrage auf der Signatur des Client basierend verifizieren. Die Anfrage wird dann zu einer deterministisch ausgewählten Gruppen von 4 weiteren Vaults übergeben, die diese Anfrage ebenfalls basierend auf der Signatur verifzieren.

Durch deterministische Selektion der zweiten Gruppe der Data Manager ist dieser Angriff nicht länger gültig für das SAFE Netzwerk, da es für den Angreifer nicht möglich ist, Kontrolle über einen Vault zu erreeichen indem er diesen umgibt.

Um das zu umgehen müsste der Angreifer die Möglichkeit haben, spezifische Vaults im SAFE Netzwerk zu umgeben. Das kann nicht erreicht werden, da dies die Fähigkeit voraussetzt verschiedene Werte zu generieren die, wenn sie mit SHA-512 gehashed werden, in Hashes resultieren die dicht um einen bestimmten Punkt liegen.
