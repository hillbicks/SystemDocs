# Un FAI simule un réseau

##Description de l'attaque

Un fournisseur d'accès Internet (FAI) crée ou simule un faux réseau. Sur ce réseau le FAI satisfait les requêtes de données demandées par l'utilisateur, mais ne stocke pas les données que l'utilisateur veut sauver sure le Réseau.

##Objectif de l'attaque

Le but et de forcer l'utilisateur à perdre des données qui peuvent lui être importante, ou simplement de discréditer le Réseau SAFE.

##Comment le Réseau empêche l'attaque

Dans ce cas le programme de d’amorçage (bootstrap) du client SAFE ou de la Vault ne sera pas capable de se connecter aux Vaults d'amorçages listées dans le cache du client. Ce fichier contient les adresses IP, ports et clés publiques de telles Vaults.

Puisque la session bootstrap est cryptée avec la clé publique de ces Vaults d'amorçages, l'essai de connexion échouera. Le client SAFE détecte donc que le réseau qu'on lui présenté n'est pas valide.

##Description d’une attaque avancée

Les machines du FAI prétendent être la machine de l'utilisateur, et demandent les informations de bootstrap de la part de l'utilisateur.

##Comment le Réseau empêche cette attaque avancée

Dans cette éventualité, la machine du FAI pourrait requêter une information et la renvoyer à l'utilisateur. Cette information est cryptée pour l'utilisateur seulement, le FAI ne peut donc pas réellement y avoir accès (à moins de casser l'algorithme de cryptage RSA 4096). L'utilisateur se connecte ensuite à ses plus proches Vaults. Si ces Vaults sont fausses, alors elles ne pourront pas traiter la requête de connexion, qui est cryptée des deux côtes. Une fois encore l'attaque échouera.

Ceci s'applique seulement dans le cas où le client SAFE n'a encore jamais été lancé sur la machine de l'utilisateur. En recevant le paquet de login, le client SAFE crypte la requête d'une de ses plus proches Vault vers la Vault d'amorçage. Une fois encore elle revient cryptée, ce qui la protège d'une attaque.
