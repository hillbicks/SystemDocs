# Paradoxe des anniversaires/attaque Sybil

##Description de l'attaque
Un attaquant submerge le Réseau avec les Vaults qu'il contrôle, pour entourer une unique Vault avec 3 ou plus Vaults malveillantes, afin d'exercer un contrôle sur la Vault en question.

##Objectif de l'attaque

Un attaquant pourrait dans ce cas demander la suppression de fragments de données en prenant le rôle d'un Data Manager avec les Vaults contrôlées. Cela pourrait pousser les Data Holders à supprimer le fragment en réponse à une requête apparemment légitime, et ainsi empêcher son propriétaire d'accéder à cette donnée.

Même s'il n'est pas possible de délibérément positionner ses Vaults autours d'un point donné du Réseau SAFE, en ayant 0.8% des Vaults sous le contrôle (temporaire) d'un attaquant, il est probable que celui-ci parviennent à entourer une Vault au moins sur le Réseau, lui permettant d'exercer un contrôle sur cette Vault, et de créer un consensus sur ces mauvaises actions.

##Comment le Réseau empêche l'attaque

Le Réseau SAFE impose que toutes les requêtes soient traitées par au moins 2 groupes de Vaults.

Un client SAFE passe une requête à 4 de ses Data Managers, qui vérifient que la requête est bien basée sur la signature du client. La requête est ensuite passée à un groupe déterminé de 4 autres Vaults qui vérifient aussi la requête à partir de sa signature.

En sélectionnant de manière déterminée le second groupe de Data Managers, cette attaque ne peut plus être menée sur le Réseau SAFE, puisqu'il n'est pas possible au Réseau de gagner le contrôle d'une Vault simplement en l'entourant.

Pour contourner ce mécanisme, l'attaquant devrait avoir la capacité de cibler a priori les Vaults qu'il veut entourer. Ceci ne peut pas être mis en pratique, car il faudrait être capable pour cela de générer différentes valeurs, qui une fois hachées avec SHA-512, délivrent des hashs proches d'un même point.
