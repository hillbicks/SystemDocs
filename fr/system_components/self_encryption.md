# Sécurité - auto-cryptage
La sécurité des données de chaque utilisateur est un aspect critique du Réseau SAFE et est en parti assuré par le processus d'auto-cryptage (self encryption). Le Réseau impose que toute donnée soit non reconnaissable et résistante au décryptage, même dans le cas où l'algorithme de cryptage lui-même serait compromis.

L'auto-cryptage est utilisé pour mélanger et crypter les données avant même qu'elles soient envoyées sur le Réseau SAFE. Ce processus est automatique et instantané.

Lorsque la donnée est sauvée sur le disque dur virtuel de l'utilisateur, celle-ci est divisée en un minimum de 3 fragments, qui sont [hachés](http://fr.wikipedia.org/wiki/Fonction_de_hachage) et cryptés. Pour brouiller encore la donnée, chaque fragment passe par une fonction [XOR](http://fr.wikipedia.org/wiki/Fonction_OU_exclusif) en employant  les hashs des autres fragments.
Ceux-ci sont alors coupés, et une association clé-éléments est créée pour chacun dans une table sur l'ordinateur de l'utilisateur, appelée data map. Cette data map contient la localisation de chaque fragment qui constitue le fichier initial. La data map est utilisée avec les hashs crées avant et après le cryptage pour retrouver et décoder la donnée de l'utilisateur. C'est la seule méthode pour retrouver la donnée puisque le cryptage est non réversible.

Tout ce processus à lieu sur le client de l'utilisateur, et de cette manière la donnée est toujours déjà cryptée lorsqu'elle est envoyée sur le Réseau. Seul l'utilisateur avec les autorisations appropriées peut décrypter le fichier. Ce système garantit que les mots de passes ne soient jamais être capturés sur le Réseau, puisque ceux-ci ne quittent jamais l'ordinateur de l'utilisateur. Et pour encore accroitre la protection, la data map subit également une phase d'auto-cryptage.

Le Réseau SAFE utilises un système de [déduplication](http://fr.wikipedia.org/wiki/D%C3%A9duplication) pour augmenter l'efficacité du stockage des données, qui fonctionne alors même que plusieurs copies de la même donnée sont cryptées chacune de manière unique : le Réseau est capable de distinguer les fragments de données identiques en comparant leurs hashs. Les Vaults elles aussi utilisent un hash pour s'identifier sur le Réseau, comme expliqué au [2.2.2](./guaranteed_vault_identification.html)


[Ici se trouve une vidéo qui explique le mécanisme d'auto-cryptage](https://www.youtube.com/watch?v=Jnvwv4z17b4)

Voici un résumé du processus d'auto-cryptage.

![Self encryption figure](./img/self-encryption.png)
