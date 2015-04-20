# Attaque par une Vault malhonnête

##Description de l'attaque

Dans cette attaque, une Vault déclare offrir une capacité de stockage supérieure à celle dont elle dispose en réalité.

##Objectif de l'attaque

Le but est de gagner la capacité de rejoindre des régions particulières du Réseau SAFE et d'établir de nouvelles règles en changeant son comportement habituel, de manière similaire à une "50% attack", que l'on retrouve fréquemment dans le domaine des monnaies cryptographiques.

Des attaques de ce type conduites à grande échelle pourraient causer des dommages considérables au Réseau SAFE, comme le vol, le double spending de safecoins, ou la suppression de données.

Le but pourrait être plus simplement de sapper la confiance des utilisateurs envers le Réseau.

##Comment le Réseau empêche l'attaque

Le Réseau impose un consensus de Vaults légitimes pour qu'une requête soit acceptée, ce qui limite le risque d'une telle attaque.

Un botnet ou un groupe de Vaults malveillantes devraient constituer autour de 75% de tout le Réseau SAFE pour arriver à un quorum et faire accepter une requête invalide, ce qui rend le succès d'une telle attaque improbable.
