# Infrastructure

Cette FAQ présente les questions relatives aux composants et processus qui assurent le fonctionnement du Réseau.


### Le Réseau SAFE requiert-il une connexion permanente à Internet de tous les participants ?

Non, le Réseau évalue chaque type de connexion et ajuste les ressources qu'il fournit, pour prendre en compte les temps d'indisponibilité (lorsqu'une machine défaille ou est éteinte).

Il n'est pas problématique d'être déconnecté pour de courtes périodes. Par contre l'être pendant longtemps affectera négativement les services que vous recevrez du Réseau.

### Que signifie 'décentralisé' pour le Réseau SAFE ?

Le Réseau SAFE, et en particulier l'approche de MaidSafe est que 'décentralisé' signifie 'totalement décentralisé'. Ce terme est utilisé de manière généralisée dans la communauté informatique. Pour le Réseau SAFE, cela signifie:

1. Aucun serveur
2. Aucune information héritée de l'infrastructure des serveurs (pas de DNS, de time servers, etc.)
3. Aucune connexion a des réseaux bases sur des serveurs
4. Aucune centralisation de données parmi les nœuds
5. Aucun contrôle par un ou des tiers
6. Aucun contrôle humain pour l'administration des données
7. Aucun moyen d'entraver l'accès au Réseau
8. Aucunes frontières: pas de contrôle ou de frontières sur la portée du Réseau.

###Est-ce que votre capacité de stockage est limitée?

Lorsque le Réseau aura atteint sa masse critique, la capacité de stockage de chaque utilisateur ne dépendra que du nombre de safecoin dont il dispose et qu'il veut utiliser pour stocker ses données.

Ces safecoins peuvent être perçus en fournissant des ressources au Réseau, ou achetées. Le Réseau SAFE utilise un processus de [déduplication](http://fr.wikipedia.org/wiki/D%C3%A9duplication) pour utiliser au mieux l'espace disponible. Puisque le Réseau est constitué par les ressources des utilisateurs, les coûts d'infrastructure ne représentent qu'une infime partie des couts générés par la maintenance de serveurs centraux.

###Qui paie pour le stockage?

Les utilisateurs (Fermiers) fournissent une partie inutilisée de leurs ressources au Réseau et c'est sur ces ressources que les données du Réseau sont stockées. Les utilisateurs qui contribuent au Réseau SAFE en rendant disponible une partie de leur espace disque reçoivent en récompense des safecoins.

###Pour combien de temps mes données sont-elles stockées?

Toutes les données stockées sur le Réseau SAFE le sont pour toujours, à moins que l'utilisateur à qui elles appartiennent décide de les effacer. Les données qui sont stockées depuis longtemps mais non consultées seront archivées.

###Comment vous assurez-vous qu'aucune donnée n'est perdue lorsqu'un utilisateur se déconnecte?

Le Réseau SAFE maintient pour toute donnée automatiquement un minimum de 4 copies disponibles. Lorsqu'un utilisateur éteint sa machine, leur Vault Managers (le groupe responsable de contrôler les adresses des nœuds du Réseau) informe le Réseau et tous les fragments de données stockés par la Vault et les Data Managers de la machine sont répliqués ailleurs. Ce processus est très rapide, autours de 20 millisecondes.

###Avez-vous la capacité de décentraliser des sites web comme YouTube?

Oui, il est possible de décentraliser n'importe qu'elle application ou service web qui existe actuellement sur Internet. 

###Comment le Réseau SAFE réagit-il face aux attaques Sybil?

Le Réseau SAFE exige que toutes les requêtes soient traitées par au moins 2 groupes de Vault. Le client SAFE passe une requête à ses 4 Data Managers, qui vérifient la validité de celle-ci avec la signature du client. Elle est ensuite transmise à un autre groupe de Vault prédéterminé qui vérifient également la requête à partir de sa signature.

En sélectionnant de manière déterministe le second groupe de Data Managers, une telle attaque ne peut plus être engagée contre le Réseau SAFE, car il est impossible de contrôler une Vault simplement en l'entourant.

Pour contourner cela l'attaquant devrait être capable d'entourer une Vault désignée dans le Réseau. Ceci est impossible, car il faudrait pour cela pouvoir générer plusieurs valeurs, qui, hachées avec SHA-512 donneraient un hash proche d'un point particulier.

###Comment le Réseau SAFE gère-t-il la redondance de donnée pour garantir que chaque donnée reste accessible?

Lors du processus d'auto-cryptage, chaque fichier est crypte puis divise en fragments. Le Réseau conserve et maintient 4 copies cryptées de chaque fragment, et déplacent ceux-ci à mesure que des nœuds du Réseau deviennent indisponibles (si les machines sont éteintes ou défaillent). Afin de faire face à ce brassage, le Réseau est capable de se reconfigurer intégralement de manière extrêmement rapide (environ 20 millisecondes). Les fragments sont distribués de manière globale pour accroitre la robustesse.


Le Réseau SAFE est totalement autonome, et toutes ces opérations sont prises en charge par le système à l'insu de tous ses utilisateurs.

###Comment les données sont-elles stockées et récupérées?

Elles sont stockées et retrouvées grâce au processus d'auto-cryptage (self encryption). Il est utilisé pour mélanger et crypter les données avant même qu'elles soient envoyées sur le Réseau. Ce processus est automatique et instantané. Lorsqu’une donnée est sauvée sur le disque virtuel de l'utilisateur elle est divisée en au moins 3 fragments. Une data map est créée pour chacun des fragments et un hash (une empreinte digitale unique) est générée et inscrite dans la data map. Pour augmenter encore la sécurité, la data map elle-même est passée par ce processus d'auto-cryptage.

Chaque fragment est ensuite crypté pour créer une donnée aléatoire et non-reproductible. Enfin, ils sont cryptés une nouvelle fois avec le premier hash. Le nouveau hash qui en résulte est aussi inscrit dans la data map. 

Il faut que la data map contienne ces hash avant et après cryptage pour récupérer et décoder la donne de l'utilisateur, puisque le processus de cryptage n'est pas réversible. La donnée est décryptée puis reconstituée grâce aux identifiants de l'utilisateur (PIN, mot-clé, mot de passe).

###Lorsque les fichiers sont divises en fragments, le Réseau crée un hash (empreinte digitale) pour chacun d'eux. Est-ce que l'analyse de ce hash peut permettre de remonter jusqu'à l'utilisateur?

Non. Les fragments ne sont pas liés a un utilisateur spécifique du Réseau, et l'anonymat (et donc la sécurité puisque l'un ne va pas sans l'autre) est au cœur du fonctionnement du Réseau. Il y a plusieurs fonctionnalités qui la garantissent, résumées ci-dessous:


* RUDP (Reliable UDP) crypte chaque message de point en point lorsqu'ils traversent le Réseau.
* La couche de routage efface l'adresse IP après le premier saut.
* Le stockage et la récupération des données utilise un identifiant connu seulement du Réseau, et qui n'est pas lie à une personne ou un nom public.
* Il n'y a pas de login à un serveur, et donc aucun point central de connaissance à attaquer.
* Les mots de passe ne sont ni stockes par ni transmis au Réseau.
* Tous les messages sont encryptes et l'identifiant de l'émetteur/receveur n'est pas celui avec lequel l'utilisateur se connecte; l'identifiant est stocké à l'intérieur d'un paquet crypte.
