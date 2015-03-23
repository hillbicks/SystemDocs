# Schädliche Daten speichern

##Angriffsbeschreibung

Wenn ein Angreifer die Selbstverschlüsselung untergräbt, um zu versuchen illegale oder unmoralische Daten unter 1MB Größe zu erstellen und diese nach dem Hash benennt, dann wäre der Angreifer potentiell in der Lage dazu die Daten im SAFE Netzwerk zu speichern.

Da selbstverschlüsselte Daten stark verschleiert sind und nahezu unmöglich zu entziffern, würde das normale Speichern von Daten nicht zu diesem Angriff
führen. Es gibt keinen Mechanismus um das Speichern von Daten auf einem bestimmten Vault zu forcieren, die Daten sind lediglich im SAFE Netzwerk.

##Zweck des Agriffs

Der Sinn dieses Angriff wäre einzig die Diskreditierung des Netzwerks. Es ist unwahrscheinlich das irgendein Angreifer weiß wo die Daten gespeichert sind und das Netzwerk legt diese Informationen in keinster Weise offen. In jedem Fall könnte ein Angreifer behaupten, dass es ihm gelungen ist um die Benutzer zu alarmieren.

##Abwendung des Angriffs

Da nicht bekannt ist wo die schädlichen Daten im SAFE Netzwerk gespeichert sind, ist dieser Angriff bereits vereitelt da der Angreifer die Daten nicht abrufen könnte. Das ansich ist bereits eine Absicherung, das SAFE Netzwerk geht jedoch noch einen Schritt weiter.

Wenn Daten im SAFE Netzwerk gespeichert werden, wird der Schlüssel zum speichern der Daten auch zum Verschlüsseln der Daten genutzt. Dieser Schlüssel wird selbst noch gehashed und der Inhalt zu diesem Schlüssel umbenannt. Der Original Schlüssel und der Inhalt werden anschliessend gelöscht. Auf diese Weise ist der Vault der die Daten speichert nicht in der Lage die gespeicherten Datenteile zu entschlüsseln.

Das SAFE Netzwerk kann ein Datenteil anfragen woraufhin der Vault diese Anfragen hashen wird und in seinem Speicher nach diesem Hash sucht. Wenn dieser gefunden wird, benutzt der Vault den angefragten Schlüssel um den Inhalt im Speicher zu dekodieren und die Daten, benannt mit dem Schlüssel, zu senden. Dieses wird dann zu einem unveränderbaren Datenteil zusammengesetzt, allerdings nur für den anfragenden Vault.

Daher ist es ähnlich zu einem Angreifer, der die Daten selbst komplett verschlüsselt und sie auf irgendeinem Computer oder Netzwerk speichert.
