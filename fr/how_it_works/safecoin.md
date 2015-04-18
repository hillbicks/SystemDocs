#Les Safecoins

Les safecoins peuvent être gagnés, échangés ou achetés. La valeur des safecoins sera déterminée par le marché, selon l'offre et la demande.

## Le cours du marché
Le nombre de safecoins en circulation augmentera en même temps que l'usage du Réseau. Pratiquement tous les détenteurs de safecoins seront au départ des fermiers (farmers), et cette offre de ressources assurera à la fois la liquidité et la distribution des ressources. Il est prévu que la quasi-totalité des utilisateurs auront en leur possession au moins quelques safecoins dans leur portefeuille.

Les utilisateurs peuvent échanger leurs safecoins contre des services, pour de l'argent liquide, ou pour une autre monnaie virtuelle, à l'aide d'un marché de changes. Le ratio des safecoins économisés (restant dans les nouveaux portefeuilles) par rapport au ratio des safecoins émis par les fermiers établira un prix, qui constituera la valeur marchande d'un safecoin.

## Ressources et monnaie

Les safecoins sont utilisés pour accéder aux services du Réseau SAFE. Ceci encourage la réutilisation de la monnaie, qui par la même accroit la demande de cette ressource finie. Par conséquent, la valeur des safecoins augmente avec le temps. Alors que la valeur elle-même des safecoins augmente, la quantité de services qu'ils permettent d'acheter sur le Réseau augmente aussi, comme le décrit la Figure 2.

![](safecoin resources.png)

## Taux de farming

Le Farming est le processus par lequel l'utilisateur fournit ses ressources (espace de stockage, CPU et bande passante) au Réseau.

Comme montre par la figure 1, l'algorithme de rendement des safecoins est base sur fonction sigmoïde: chaque Vault gagne peu au début, mais le taux augmente à mesure qu'elle supporte le même stockage que la moyenne du Réseau. Ce rendement prend aussi en compte le rang de la Vault, un mécanisme par lequel le Réseau évalue l'utilité de chaque Vault entre 0 (le moins bon) et 1 (le meilleur). Le taux est au final le résultat du rendement du réseau, du solde de la demande et de l'offre, multiplie par le rang de la Vault. Le rendement du réseau commencera à s'aplanir à partir de 20% au-dessus de la moyenne, décourageant ainsi les très grandes Vaults, qui pourraient sans cela recréer une certaine centralisation dans le processus de farming. Des safecoins sont alloués par le réseau et sont payés a la Vault qui rapatrie avec succès une donnée (GETS), mais pas lorsqu'une donnée y est stockée (PUTS).

![](safecoin farming speed.png)

Le réseau accroit automatiquement les récompenses de farming lorsque plus d'espace est nécessaire, et les réduit lorsque l'espace est disponible en trop grange quantité. Les données étant distribuées uniformément sur le réseau, les fermiers désireux de maximiser leurs gains auront d'avantage intérêt à mettre en place plusieurs nœuds aux performances moyennes qu'un seul nœud avec de meilleures spécifications.

## Mécanisme de transfert des safecoins

Contrairement à bitcoin, le Réseau SAFE n'utilise pas de blockchain pour contrôler la possession des safecoins. Les Transaction Manager du Réseau SAFE sont non-chaines, c'est à dire que seuls le propriétaire précèdent et actuel d'un safecoin sont connus. Il est pertinent de considérer au regard de cet aspect les safecoins comme de l'argent liquide numérique.

Un des défis majeurs que doivent surmonter toutes les monnaies virtuelles est d'éviter le phénomène de double spending. A l'intérieur du Réseau SAFE le transfert des données (qui incluent donc les safecoins) est atomique, et utilise une signature cryptographique pour transférer la propriété.

Les safecoins, la monnaie du Réseau SAFE, sont générés en réponse à l'utilisation du réseau. Lorsqu'une donnée est stockée, ou lorsqu’une application est créée puis utilisée, le réseau génère des safecoins, chacun avec un identifiant unique. Ces safecoins sont eux-mêmes divisibles, et à chaque nouvelle dénomination est associé une nouvel identifiant unique.

A mesure que les safecoins sont alloues a des utilisateurs par le Réseau, seul leur propriétaire peuvent transférer l'un de ses safecoins grâce à une signature cryptographique. Pour illustrer ceci, quand Alice paie un safecoin a Bob via le client, elle soumet une demande de paiement. Le Transaction Manager vérifie qu'Alice est la détentrice actuelle du safecoin en récupérant sa clé publique et en confirmant qu'il est signe correctement avec la clé privée correspondante. Les Transaction Managers n'accepteront qu'un message signe par son propriétaire. Cela lève tout doute sur le fait qu'Alice est bien la propriétaire de ce safecoin, à la suite de quoi celui-ci est transfère à Bob qui maintenant est le seul à pouvoir le transmettre à un autre utilisateur. Le processus est résumé en figure 3.

![](safecoin transfer mech.png)



