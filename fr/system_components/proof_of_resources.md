# Preuve de ressources (proof of resource)

Le mécanisme de preuve de ressources permet au Réseau de valider ses actions ou services à l'aide d'une méthode vérifiable mathématiquement.

Le processus mesure la capacité d'une Vault à stocker et retrouver des fragments de données. Il dépend des critères suivants, qui concernent la machine de l'utilisateur:

* Vitesse du processeur,
* Bande passante disponible,
* Espace disque,
* Temps passé connecté au Réseau.

Ceci donne à la preuve un caractère utile, mesurable et immédiatement vérifiable. Elle est en outre un mécanisme efficace qui a un cout transactionnel nul.

La preuve de ressources sur le Réseau SAFE utilise un mécanisme similaire à celui d'une [preuve à divulgation nulle de connaissance](http://fr.wikipedia.org/wiki/Preuve_%C3%A0_divulgation_nulle_de_connaissance), ou zero knowledge proof. Le mécanisme vérifie qu'une donnée est bien disponible, mais n'a pas besoin de connaitre le contenu de la donnée: seulement de savoir qu'elle est bien stockée et qu'elle l'est de manière précise. 

La preuve de ressources suit les étapes suivantes:

1. Un groupe de Vaults de contrôle crée une chaine de caractères aléatoire,
2. Cette chaine est cryptée et envoyée à tous les détenteurs de la donnée,
3. Ces derniers prennent la chaine, l'ajoutent a la donnée original, et construise un hash du tout,
4. L'ensemble de ces hash est récupéré et décrypté par le groupe de contrôle et les compare,
5. Si l'une des Vaults renvoie un résultat différent des autres, alors elle est considérée comme compromise, et son rang est abaissé.

Le mécanisme est déclenché par les requêtes GET, et lors des transferts de compte. Les transferts de compte ont lieu lorsque des informations concernant un fragment de donnée ou concernant le nœud stockant un fragment sont passées à de nouveaux membres d'un [groupe mitoyen](http://systemdocs.maidsafe.net/content/fr/how_it_works/vault.html). La preuve de donnée est non déterministe, et rendue aléatoire à chaque utilisation et pour chaque utilisateur. Elle est considérée comme sure, et utilise la divulgation nulle de connaissance, non pas pour masquer le contenu de la donnée (puisque le Réseau peut demander n'importe quelle donnée), mais pour s'assurer que toute donnée qui doit être transférée n'est pas contaminée.

Voici un aperçu du processus de preuve de ressources.

![Proof of resource figure](./img/por-diagram.png)
