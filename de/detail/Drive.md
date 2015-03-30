## Drive Überblick

Die MaidSafe-Drive library stellt ein user-mode Interface zu den kernel-mode Modulen Eldos'
[Callback File System](https://www.eldos.com/cbfs/), [Fuse](http://fuse.sourceforge.net/) und [OSXFuse](http://osxfuse.github.io/) dar, die für die Windows, Linux und Mac Plattformen bereitstehen. In jedem dieser Fälle kann ein virtuelles Laufwerk eingebunden werden, dass dann als natives Laufwerk auf dem System präsentiert wird. Dateisystem Operationen auf diesem Laufwerk werden zu relevanten callback Funktionen gefiltert, die vom user-mode code behandelt werden. Verzeichnisse bzw. Dateien sind verschlüsselt durch die Funktionen in [Encrypt](https://github.com/maidsafe/MaidSafe-Encrypt/wiki), während sie direkt im Netzwerk gespeichert/abgerufen werden durch das [Network-Filesystem](https://github.com/maidsafe/MaidSafe-Network-Filesystem/wiki), [Routing](https://github.com/maidsafe/MaidSafe-Routing/wiki) und [RUDP](https://github.com/maidsafe/MaidSafe-RUDP/wiki). Der Platz auf dem Laufwerk, der vorhanden/genutzt ist, wird abgeleitet von Werten, die ein Client berichtet sein Vault angeboten/benutzt zu haben im Netzwerk.





### Details

Das MaidSafe-Drive API beinhaltet folgende Dateien:

* [drive_api.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/drive_api.h) definiert die Grundklasse mit gemeinsamen Interface zu den plattformspezifischen, abgeleiteten Klassen, die in den header Dateien nachfolgend beschrieben sind.
*
 [win_drive.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/win_drive.h) beinhaltet user-mode Code spezifisch für Windows und [Callback File System](https://www.eldos.com/cbfs/).
* [unix_drive.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/unix_drive.h) beinhaltet user-mode Code spezifisch für Linux und [Fuse](http://fuse.sourceforge.net/).
*
[config.h](https://github.com/maidsafe/MaidSafe-Drive/blob/master/include/maidsafe/drive/config.h) stellt praktische typedefs und Konstanten zur Verfügung.

Weitere Details, die die Klassen Interfaces usw. beschreiben, können in den Dateien und auch in dem weiterführenden Material zu der Library gefunden werden.
