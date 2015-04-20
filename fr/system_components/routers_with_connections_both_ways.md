# Gestion des Routeurs
Le Réseau SAFE permet la communication entre Vaults.

Beaucoup de connexions Internet de particuliers passent à l'heure actuelle par un routeur, qui fournit des adresses privées qui ne doivent pas apparaître sur Internet. Cela est pris en charge par exemple par des serveurs [STUN](http://en.wikipedia.org/wiki/STUN).

Mais cette méthode ne peut pas être employée pour un réseau décentralisé. Dans le Réseau SAFE, RUDP est utilisé dans un processus qui émule un serveur STUN décentralisé, la table de hachage distribuée (DHT) fournissant les informations de connexion aux Vaults.

Le Réseau SAFE peut ainsi prendre en charge les connexions des utilisateurs sans que ceux-ci aient besoin de configurer spécifiquement leur routeur.
