# Infrastructure

Cette FAQ presente les questions relatives aux composants et processus qui assurent le fonctionnement du Reseau.


### Le Reseau SAFE requiert-il une connexion permanente a Internet de tous les participants ?

Non, le Reseau evaluera chaque type de connexion et s'ajustera les ressource qu'il fournit, pour prendre en compte les temps d'indisponibilite (lorsqu'une machine defaille ou est eteinte).

Il n'est pas problematique d'etre deconnecte pour de courtes periodes. Par contre l'etre pour de plus longues periodes affectera negativement les services que vous recevez du Reseau.

### Que signifie 'decentralise' pour le Reseau SAFE ?

Le Reseau SAFE, et en particulier l'approche de MaidSafe est que 'decentralise' signifie 'totalement decentralise'. Ce terme est utilise de maniere generalise dans la communaute informatique. Pour le Reseau SAFE, cela signifie:

1. Aucun serveur
2. Aucune information heritee de l'infrastructure des serveurs (pas de DNS, de time servers, etc.)
3. Aucune connexion a des reseaux bases sur des serveurs
4. Aucune centralisation de donnees parmi les noeuds
5. Aucun controle par un ou des tiers
6. Aucun controle humain pour l'administration des donnees
7. Aucun moyen d'entraver l'acces au Resau
8. Aucunes frontiere: pas de controle ou de frontieres sur la portee du Reseau.

###Est-ce que votre capacite de stockage est limitee?

Lorsque le Reseau aura atteint sa masse critique, la capacite de stockage de chaque utilisateur ne dependra que du nombre de safecoin dont il dispose et qu'il veut utiliser pour stocker ses donnees.

Ces safecoins peuvent etre percus en fournissant des ressources au Reseau, ou achetees. Le Reseau SAFE utilse un processus de [deduplication](http://fr.wikipedia.org/wiki/D%C3%A9duplication) pour utiliser au mieux l'espace disponible. Puisque le Reseau est constitue par les ressources des utilisateurs, les couts d'infrastructure ne represente qu'une infime partie des couts generees par la maintenance de serveurs centraux.

###Qui paie pour le stockage?

Les utilisateurs (Fermiers) fournissent une partie inutilisee de leurs ressources au Reseau et c'est sur ces ressources que les donnees du Reseau sont stockeed. Les utilisateurs qui contribuent au REseau SAFE en rendant disponible une partie de leur espace disque sont recoivent en recompenses des safecoins.

###Pour combien de temps mes donnees sont-elles stockees?

Toutes les donnees stockees sur le Reseau SAFE le sont pour toujours, a moins que l'utilisateur a qui elles appartiennent decident de les effacer. Les donnees qui sont stockees depuis longtemps mais non consultees seront archivees.

###Comment vous assurez vous qu'aucune donnee n'est perdue lorsqu'un utilisateur se deconnecte?

Le Reseau SAFE maintient pour toute donnee automatiquement un minimum de 4 copies disponibles. Lorsqu'un utilisateur eteint sa machin, leur Vault Managers (le groupe responsable de controler les adresses des noeuds du Reseau) informe le Reseau et tous les fragments de donnees stockes par la Vault et les Data Managers de la machine sont repliques ailleurs. Ce processus est tres rapide, autours de 20 millisecondes.

###Avez-vous la capacite de decentraliser des sites web comme Youtube?

Oui, il est possible de decentraliser n'importe qu'elle application ou service web qui existe actuellement sur Internet.

###How does the SAFE Network deal with Sybil attacks?

The SAFE Network requires all requests be processed by at least two groups of Vaults.
The MaidSafe client passes a request to its four Data managers, who verify the request based on the client’s signature. The request in then passed to a deterministically selected group of four other Vaults which also verify the request based on its signature.

By deterministically selecting the second group of Data managers, this attack no longer holds true for the SAFE Network, since it is not possible for the attacker to gain control over a Vault by simply surrounding it.

To circumvent this, the attacker would require the ability to surround specific Vaults in the SAFE Network. This cannot be achieved, as it would require being able to effectively generate different values which, when hashed with SHA-512, result in close hashes around one particular point.

###How does the SAFE Network deal with data redundancy to ensure whatever data is shared remain accessible?

Each file is encrypted and split into chunks during our encryption process (Self Encryption). The network keeps and maintains four copies of each encrypted chunk and moves these fragments around the network as nodes become unavailable, either through failure or power down. In order to cope with the churn, the network is able to reconfigure globally extremely quickly (20 milliseconds). The chunks are spread globally for increased robustness.

The SAFE Network is completely autonomous and all these operations are handled by the system and completely without our knowledge.


###How is data stored and retrieved?

Data is stored and retrieved using the self encryption process. Self encryption is used to mix up and encrypt data before it is sent out to the SAFE Network. This process is automatic and happens instantaneously. As data is saved to a user’s virtual hard-drive it is broken up into at least three chunks. A data map is created and for each chunk a hash, (a unique digital finger print) is created and written to the data map. For additional security the data map is also run through this self encryption process.

Each chunk is then encrypted to create random, non-repeating data. Finally, combined with the original hashes, the chunks are further encrypted. The output of each chunk is then added to the data map.

The data map, with hashes before and after encryption, is used when retrieving and decoding the user’s data, as the encryption process is non reversible. The data is decrypted and reconstituted with the users PIN, keyword and password.

###When files are split into chunks, the network takes a hash (a digital fingerprint) of each. Can the original hash be traced back to a user?

In short, no. Chunks are not linked to a specific user on the network and anonymity (and security as arguably they are the same thing) is at the very heart of the network. At a very high level, there are a number of features that provide anonymity.

* RUDP (Reliable UDP) encrypts every message hop to hop as they traverse the network
* The routing layer scrubs IP addresses after hop1
* The storage and retrieval of data is carried out using an identifier known only to the network that is not linked to a person or public name
* There is no server login and therefore no central point of knowledge or attack
* Passwords are not stored or transmitted on the network
* All messages are encrypted and the identifier of the sender/receiver is not what the user logs in as, the identifier is stored inside an encrypted packet


