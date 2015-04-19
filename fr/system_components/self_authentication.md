# Auto-authentification

Le Réseau SAFE supporte le mécanisme d'auto-authentification (Self authentication). Celui-ci permet aux utilisateurs de créer un compte et de se loguer sur le Réseau SAFE depuis n'importe quel ordinateur, sans avoir besoin d'un tiers et sans qu'un tiers en ait connaissance.

Pour ce faire les droits d'authentification (jeton de login) doivent être stockés sur le Réseau SAFE lors de ce processus.

Un utilisateur crée sa propre clé (K) et mot de passe (W). Un [sel][http://fr.wikipedia.org/wiki/Salage_(cryptographie)] (S) est dérivé de manière reproductible à partir de la clé et du mot de passe.

Pour générer un identifiant unique, un hash (ou emprunte) est créé à partir de la concaténation du mot clé et du sel, note ci-après H(K+S).

Une fonction de dérivation de clé (Password Based Key Derivation File, PBKDF2) est alors employée pour renforcer le mot de passe. Ceci pour parer à la relative faiblesse des mots de passe en général choisis par les utilisateurs.

Enfin, ces accès cryptés sont stockés sur le Réseau SAFE de la manière suivante:

**Stocker sur le Réseau [H(K+S)] Symmetric Encrypt [ PBKDF2[W][S] ] (Account)**

L'auto-authentification s'appuie sur un système ou chaque Vault est capable de créer une clé unique initiale pour y stocker une valeur particulière sur le Réseau. Cette valeur contient un passeport crypté vers les données de l'utilisateur.

Ce passeport contient divers types de clés qui permettent de conduire différentes tâches sur le Réseau.

La localisation de la clé initiale est masquée, ou tout du moins n'est pas évidente à déterminer sur le Réseau. Cette approche est à la base de l'auto-authentification, et est étendue sur le Réseau pour permettre que l'accès aux données soit stocké publiquement, tout en ne nécessitant pas d'autres systèmes de contrôles ou firewalls.

##Deviner les informations de login

Un intrus ou une personne malveillante sur le Réseau peut tester la présence d'un utilisateur et télécharger son jeton de login; de la même manière que les essais répétés de mot de passe sur un serveur central sont un moyen d'en récupérer un accès. Les serveurs centraux repoussent habituellement ce type d'attaque en empêchant les tentatives répétées de login. 

Chaque fois que sur le Réseau SAFE un utilisateur demande un jeton de login, le Réseau lui en fournit un. Lors d'une attaque de ce type, cela signifie que l'attaquant va devoir décrypter les millions de jetons qu'il va recevoir. Le temps de calcul nécessaire à un tel processus rend une telle attaque infaisable en pratique.

Le jeton de login n'est utilisé que pour l'authentification, lorsqu'il est validé par rapport aux mots de passe et autres informations de l'utilisateur, qui eux-mêmes ne sont jamais envoyés sur le Réseau SAFE.

[wikipedia-salt-crypto]: http://en.wikipedia.org/wiki/Salt_(cryptography)
