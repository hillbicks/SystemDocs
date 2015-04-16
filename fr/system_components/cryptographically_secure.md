# Sécurité cryptographique
Au sein du Réseau SAFE, les Vaults créent tout d'abord une [paire de clés de chiffrement](http://en.wikipedia.org/wiki/Public-key_cryptography) pour rejoindre le Réseau.

Lorsque chaque Vault communique avec une autre, ces clés sont utilisées pour sécuriser la connexion.

A la différence des mécanismes de communication sécurisés traditionnels, qui nécessitent l'échange des clés (Ex: [diffie Hellman](http://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)) le Réseau SAFE fournit ces clés depuis la couche de [routage](https://github.com/maidsafe/MaidSafe-Routing).

Le bénéfice est que même si un ordinateur est compromis, il peut être traversé sans risques par le Réseau, et dissuade ainsi les éventuelles attaques du type [Man In The Middle (MiTM)](http://en.wikipedia.org/wiki/Man-in-the-middle_attack).

La sécurité du Réseau est améliorée, puisque chaque Vault crypte tous les messages en provenance et en direction des Vaults avec lesquelles elle communique.
