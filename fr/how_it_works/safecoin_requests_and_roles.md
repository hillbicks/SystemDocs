# Les requêtes Safecoin et leurs rôles

Il existe différents types de requêtes Safecoin:
* PUT - Utilisée pour mettre des safecoins (données) dans un lieu spécifié,
* GET - Utilisée pour obtenir des safecoins (données) à partir d'un lieu spécifié,
* EXCHANGE - Utilisée pour échanger des safecoins (données) entre deux lieux spécifiés.

Les safecoins sont un autre type de données, et ils disposent donc de requêtes PUT et GET qui leur sont définies sur le réseau. Par contre, à l'inverse des autres données, ils ne disposent pas de requête DELETE. 

La requête PUT pour les safecoins à la propriété "duplication interdite". Cela signifie que s'il existe déjà un safecoin avec le même nom (les 32 premiers bits), la requête PUT est rejetée. Cette vérification est effectuée par le Data Manager qui reçoit la requête.

La requête EXCHANGE permet de mettre jour les détails d'un safecoin, mais seulement sous les conditions suivantes.

* Le propriétaire est approuvé par la majorité des Vaults d'un groupe tiers (escrow)
* Le propriétaire peut (l'actuel et le précèdent propriétaire sont alors les approbateurs) mettre à jour tous les champs
* Les Vaults d'un groupe tiers (escrow) peuvent seulement mettre à jour leur champ correspondant, et une seule fois.
* Chaque fois que les champs de l'actuel et du précèdent propriétaire sont mis à jour, la version du safecoin doit être incrémentée de 1, et tous les champs escrow sont effaces.

Les règles décrites ci-dessus sont appliquées par le Pmid Manager qui détient les données du safecoin. Lorsque le champ du propriétaire, avec les champs des Vaults du groupe tiers (escrow) sont utilisés pour une 'transaction', le Pmid Manager devient dans les faits le Transaction Manager. Dans ce cas, les données du safecoin peuvent être considérées également comme un reçu.
