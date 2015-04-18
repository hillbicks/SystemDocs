# Comment le Réseau fonctionne

Lorsqu'un utilisateur télécharge et installe le client SAFE, une Vault est créée et configurée sur son ordinateur.

Après s'être connecte et logué au Réseau SAFE, un nouveau disque virtuel est monte sur le système de l'utilisateur. S'il le sélectionne, il aura accès a ses données statiques, qui ont été encryptées et disséminées sur le Réseau dans les autres Vaults.

La Vault d'un utilisateur peut aussi prendre en charge des données dynamiques, telles que le les communications par VoIP par exemple.

Avant qu'une donnée ne soit stockée sur le Réseau SAFE, elle est automatiquement encryptée. Ce processus d'auto-cryptage (appelé self-encryption) consiste à diviser la donnée en plusieurs fragments, puis à chiffrer chacun d'eux a partir des informations de l'utilisateur, et de la donnée elle-même. Cela signifie que si un tiers voulait voir la donnée, il lui faudrait les détails de connexion sécurises de l'utilisateur, et avoir connaissance des fragments. Or, aucune de ces informations n'est en possession ni d'un serveur, ni d'un système tiers.

La plupart du temps un utilisateur se connecte au Réseau au travers d'un routeur. Le routeur utilise le protocole Reliable UDP (RUDP), un protocole plus robuste que UDP, car les paquets perdus sont retransmis a nouveaux, et car il est capable de traverser les routeurs NAT, contrairement au protocole TCP. L'utilisation de RUDP sur le Réseau SAFE permet aux données de traverser les routeurs sans risques de corruptions ou d'interceptions.

Les Vaults se connectent à leurs pairs pour le stockage et la gestion des données. Elles sont constamment évaluées et classées par rang (par la persona Data Holder Manager qui les supervise) à l'aide des critères suivants:

* **Disponibilité** - Combien de temps la Vault est en ligne/hors ligne, et a quelle fréquence elle se connecte/déconnecte
* **Stockage** - Combien d'espace de stockage est alloue a la Vault
* **CPU** - De combien de ressources CPU dispose la Vault
* **Bande passante** - A quelle vitesse le Réseau peut accéder a la Vault

A mesure que les besoins et ressources sur le Réseau SAFE varient, les Vaults adaptent et équilibrent en permanence la charge du Réseau. Ce mécanisme d'ajustement est réalisé automatiquement par les Vaults. Le Réseau SAFE est complètement autonome, et peut donc réagir rapidement et sans intervention humaine.

Lorsqu'un utilisateur fournit plus d'espace de stockage que qu'il n'utilise lui-même sur le Réseau SAFE, il reçoit en récompense des safecoins, distribués de manière aléatoire par le Réseau. Il peut savoir combien de safecoins il a détient en consultant son portefeuille. Ce portefeuille est configuré automatiquement lors du processus d'installation du client SAFE et à la création d'un compte.

Les développeurs d'applications gagnent des safecoins lorsqu'ils créent des applications et des programmes pour le Réseau SAFE. Lorsqu'une application est utilisée, le développeur reçoit des safecoins.

Les Core Developers peuvent aussi obtenir des safecoins en contribuant au code source du Réseau SAFE.
