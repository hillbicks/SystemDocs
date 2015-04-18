# Les Vaults

Le Réseau SAFE est composé de programmes appelés Vaults (que l'on peut traduire par coffre-fort). Chaque Vault est localisée sur le disque dur de l'ordinateur d'un utilisateur et remplit plusieurs fonctions pour le Réseau, qui varient et s'adaptent en fonction des besoins de ce dernier. Une de ses principales fonctions est le stockage et le cryptage des données qui lui sont transmises par les autres Vaults du Réseau.

Une autre fonction, encore appelée "persona" d'une Vault, est la gestion et la supervision des autres Vaults (Data Manager). Lorsque des fragments de données sont transmis entre les Vaults, cette fonction de gestion contrôle et vérifie l'intégrité des données. 

Les Vaults peuvent aussi prendre la persona de gestionnaire de transaction (Transaction Manager), ou de validateur de transaction (Transaction Validator), qui sont, elles, utilisées pour gérer le transfert des safecoins.

Les Vaults n'ont pas connaissance des données qu'on leur demande de stocker (seul le client peut les décrypter), et, puisqu'elles n'en stockent qu'un fragment, et pas la donnée complète, il est impossible de déchiffrer quelle est la source originale de données, que ce soit un un document ou une communication. 

Pour chaque Vault qui stocke une donnée particulière, il y a également 31 autres Vaults chargées de stocker cette même donnée. Ceci implique que si une Vault se retrouve hors-ligne ou qu'un fragment de donnée devient corrompu, la donnée n'est pas perdue pour autant: chaque Vault cherche alors automatiquement un nouvel homologue pour y répliquer la donnée.

Les Vaults ne sont pas visibles pour les utilisateurs du Réseau SAFE. A la place, un utilisateur voit simplement un nouveau disque virtuel monté sur son ordinateur, qui contient ses données personnelles. Puisque les Vaults sont constamment en communication et se contrôlent les unes les autres, l'utilisateur peut avoir accès instantanément a ses données depuis ce disque virtuel.
