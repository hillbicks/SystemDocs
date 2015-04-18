# Stocker de mauvaises données

##Description de l'attaque

Imaginons qu'un attaquant parvenait à retourner le processus d'auto-cryptage pour essayer de créer une donnée illégale de moins de 1MB, et nommer avec le hash de son contenu. Alors il lui serait potentiellement possible de stocker cette donnée de manière illégale sur le Réseau.

Le processus de stockage de données ne cèderait normalement pas à ce type d'attaque, car l'auto-cryptage occulte et rend la donnée presque impossible à décoder. Il n'y a pas de mécanismes pour forcer une donnée à être stockée sur une Vault en particulier, mais la donnée serait simplement sur le Réseau SAFE. 

##Objectif de l'attaque

Ce serait une attaque pour discréditer de manière pure et simple le Réseau. Il est improbable qu'un attaquant sache ou la donnée serait stockée et le Réseau ne révèlerait cette information en aucun cas. Un attaquant pourrait néanmoins affirmer qu'il a réussi à le faire pour effrayer les utilisateurs.

##Comment le Réseau empêche l'attaque

Ne sachant pas où est stockée la donnée illégale, cette attaque est déjà contrecarrée, l'attaquant ne pouvant pas la récupérer. C'est déjà une forme de protection en soi, mais le Réseau SAFE va encore plus loin.

Lorsqu'une donnée est stockée sur le Réseau SAFE, la clé utilisée pour le stockage est aussi employée pour crypter le contenu. Cette clé est ensuite elle-même hachées et son contenu est renomme par cette clé. La clé originale et son contenu sont ensuite effacés. De cette manière, une Vault n'a aucun moyen de décrypter les fragments qu'elle a la charge de stocker.

Le Réseau SAFE peut demander un fragment, et la Vault lui hachera la requête et regardera si elle possède le hash du fragment demande. Si elle le trouve, elle utilisera la clé de la requête pour décoder le contenu dans sa mémoire et envoyer la donnée avec la clé demandée. Ceci permettra à la Vault qui a fait la requête, et à elle seule, de retrouver le contenu du fragment.

Ceci est par conséquent semblable a un attaquent qui crypterait complétement les données elles même et les stockerait sur n'importe quel ordinateur et réseau.
