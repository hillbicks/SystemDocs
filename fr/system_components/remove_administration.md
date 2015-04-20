# Suppression de l'administration

Le Réseau SAFE est autonome, et par conséquent il n'y a pas besoin d'administrateurs pour gérer et maintenir les données ou le Réseau lui-même. Chaque Vault s'adapte constamment à son environnement local, et des vérifications d'intégrité  détectent les éventuelles corruptions et mutations des fragments de donnée, en vérifiant le hash de chaque fragment.

Le Réseau maintient automatiquement à jour 4 copies de chaque fragment de données, mais aussi jusqu’à 16 copies mortes, dans la mesure où des machines hors-lignes ont des chances de rejoindre à nouveau le Réseau dans le future.

Les données sont en outre stockées au hasard sur le Réseau, parmi un espace de 2^512 adresses. Ceci permet de distribuer de manière homogène la charge sur tout le Réseau, tâche qui est  traditionnellement l’une des missions d'un administrateur réseau.
