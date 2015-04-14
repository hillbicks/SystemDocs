#Les Vaults
Les Vaults sont créées sur les ordinateurs des utilisateurs au moment de l'installation du client SAFE et de l’inscription au Réseau SAFE.

L'utilisateur ne peut pas voir la Vault qui est installée sur sa machine. A la place, il voit un nouveau disque virtuel monté sur son système, qui lui donne accès à ses données sur le Réseau.

Lorsqu'un utilisateur crée ou modifie un fichier sur ce disque virtuel, ce fichier passe par plusieurs processus pour s'assurer que celui-ci est sécurisé et qu'il utilise au mieux les ressources du Réseau.

## Les personas des Vaults
Les Vaults peuvent avoir différentes personas, chacune d'elles ayant un rôle différent dans le fonctionnement du Réseau. Ces personas peuvent être groupées en 4 catégories:
* **Client Managers (gestionnaire de client)**<br/>
Ce groupe de personas est chargé du routage de nœuds particuliers sur le Réseau: ceux qui sont le plus proche du nœud du client. Ils savent qu'ils reçoivent une connexion d'un client lorsque celle-ci n'apparait pas dans leur table de routage. Les MaidManagers (le groupe qui s'occupe d'un compte Maid) et les MpidManagers (le groupe qui s'occupe des noms publics et des fichiers partages pour les clients publics) sont des exemples de personas de ce groupe.
* **NAE Manager **<br/>
Network Addressable Element manager, ou gestionnaire d'éléments adressables par le Réseau. Les NAE Managers sont mis en route lorsque l'adresse du nom d'un élément adressable est proche de leur propre adresse. Les DataManagers (chargés de surveiller les pointeurs de données) et les VersionManagers (responsables de l'annuaire et des autres 'mutating nodes' directement adressable) en sont des exemples.
* **Node Manager (gestionnaire de nœuds réseau)**<br/>
Pour chaque nœud du Réseau, c'est le groupe qui l'entoure directement. Un nœud sait pour quels autres nœuds il est NodeManager car ces derniers apparaissent dans sa table de routage. Les PmidsManagers (groupe chargé de contrôler les nœuds qui stockent des données) représentent un exemple de Node Managers.
* **Managed node (nœuds gérés)**<br/>
C'est un nœud de routage parmi un groupe de Node Managers. Ex: PmidNode (nœud qui détient un élément de donnée).

Il est impératif que ces groupes de personas obtiennent l'autorisation d'agir dans chacun de ces rôles, sans quoi un nœud pourrait déclarer appartenir à n'importe quel groupe et exécuter n'importe quelle fonction. Cette autorisation est obtenue après que chaque persona envoie son type de personas dans un message au groupe suivant. Celui-ci accumule alors les messages, vérifie et valide les signatures du groupe tout entier.

## Les donnees sur le Réseau SAFE

Le Réseau utilise 2 mécanismes pour autoriser un utilisateur à faire certaines actions à travers le Client SAFE. Une autorisation est obtenue par le consensus d'un groupe chaque fois que le Client met/stocke une nouvelle donnée sur le Réseau. Alternativement, des [signatures cyrptographiques](http://en.wikipedia.org/wiki/Digital_signature) sont utilisées si le client modifie des données déjà stockées, ou envoie des safecoins par exemple.

** Consensus de Groupe**<br/>
Lorsqu'un utilisateur poste une nouvelle donnée, le fichier est crypte et divise en plusieurs fragments lors du processus d'auto-cryptage (self-encryption). Puis il est passe un groupe fermé de Client Managers. Ce groupe fermé est compose des Vaults dont l'ID est le plus proche de la Vault de l'utilisateur au sens [XOR](http://en.wikipedia.org/wiki/Exclusive_or). Cette distance est une grandeur mathématique et non pas par une mesure géographique. Au moins 8 des 32 Client Mangers doivent atteindre un consensus pour que toute opération sur le réseau soit poursuivie.

Les Clients Managers passent alors les fragments à 32 Data Managers, choisis par le Réseau comme ayant l'ID le plus proche des IDs des fragments ; il apparait donc que l'ID d'un fragment détermine sa localisation sur le Réseau.

Le Réseau utilise une approche Scatter/Gather, basée sur le [Rabin’s Information Dispersal Algorithm](http://people.seas.harvard.edu/~salil/rabin2011-slides/rabin2011-mitzenmacher.pdf), qui permet de supporter une petite perte de donnée (jusqu'à 4 fragments) sans avoir besoin de relancer la transmission.

Lorsque le consensus est atteint, le Data Manager passe les fragments a 32 Data Holder Managers, qui a leur tour les transmettent a des Data Holders pour qu'ils soient stockes. Si un Data Holder Manager averti que l'un de ses Data Holders est hors-ligne, le Data Manager décide, en consultant le rang des Vaults, dans laquelle il placera le fragment de donnée. 

De cette manière les fragments du fichier original sont en permanence contrôlés pour s'assurer que son utilisateur puisse y avoir accès au fichier et le décrypter.

Un mouvement de fragments sur le Réseau ne peut être effectue que si il y a un consensus (28 sur 32) des Vaults l'entourant. Une Vault ne peut jamais agir isolement.

Toutes les communications sur le Réseau SAFE sont prises en charge par des groupes fermes de 32 nœuds. Ceci empêche un nœud rebelle d'agir avec malveillance. Un utilisateur ne peut pas choisir l'ID de son propre nœud, ni décider ou ses données sont stockées; tout cela est déterminé par le Réseau. Chaque fois qu'un nœud se déconnecte puis se reconnecte au Réseau, celui-ci lui assigne un tout nouvel ID aléatoirement.

[Cette vidéo résume le fonctionnement des Vaults](https://www.youtube.com/watch?v=txvKSeCaEP0)

** Signatures Cryptographiques**<br/>
Lorsqu'un utilisateur change une donnée déjà stockée sur le Réseau, par exemple en modifiant le contenu d'un fichier, ou en envoyant des safecoins à un autre utilisateur, le réseau n'utilise pas le mécanisme de consensus de groupe, car il n'est pas nécessaire dans ce cas, et s'en passer permet d'économiser des ressources Réseau.

Les signatures cryptographiques valident mathématiquement le propriétaire d'une donnée et peut le prouver avec certitude, à partir du moment bien sûr ou l'utilisateur garde sa clé privée en sécurité. Si l'utilisateur est le propriétaire d'une donnée et peut le démontrer en signant numériquement sa demande avec sa clé privée, alors le Réseau lui autorise l’accès a sa donnée pour modification.
