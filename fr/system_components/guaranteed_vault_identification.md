# Garantie d'identification des Vaults
L'identification des Vaults est essentielle dès lors que les Vaults sont responsables dans une large mesure de la gestion des ressources du Réseau.

Sans méthode d'indentification une Vault pourrait se faire passer pour n'importe quelle autre. Il y a plusieurs méthodes connues pour valider des identités numériques, par exemple en faisant appel à des autorités de certification (comme Verisign), ou alors dans certains cas par des réseaux de confiance. Comme le Réseau SAFE n'a pas de serveurs, ne requiert aucune intervention humaine et est complètement trustless, ces options ne sont pas adaptées.

Le processus d'identification des Vaults implique la création de 2 paires de clés cryptographiques. Une de ces paires est une paire dite de révocation, et n'est utilisée que pour créer ou invalider la paire de clés réelle.

La paire de clés réelle est créée et la clé publique est signée par la clé de révocation privée. Ce paquet (clé public + signature) est stocké sur le Réseau avec le type Clé d'Identification de Vault. Ce hash est ensuite utilisé comme identifiant de la Vault.

Le Réseau SAFE peut retrouver le paquet d'identification de n'importe quelle Vault. La seule manière d'altérer ce paquet est par un message signé par le même identifiant que celui qui a signé le paquet.

Ce processus permet donc aux Vaults de s'identifier elles-mêmes en publiant le hash du paquet d'identification qu'elles ont créé et qu'elles stockent.

Puisque seul le créateur possède la clé privée et la clé publique contenus dans le paquet d'identification, alors on peut considérer que c'est bien la bonne Vault. Tous les messages sont cryptés avec cet ID, par conséquent des Vaults malveillantes ne pourront pas décrypter ces messages, et donc ne pourront pas non plus avoir accès aux requêtes de connexion au Réseau.
