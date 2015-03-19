# Как она работает

Когда пользователь скачивает и устанавливает клиент SAFE, происходит настройка хранилища на его компьютере. 

После присоединения и регистрации в SAFE Network, пользователь увидит новый виртуальный диск на своем компьютере. На нем можно увидеть статические данные, зашифрованные и распределенные по другим хранилищам.

Хранилище к тому же может хранить и динамические данные, например, общение через приложение VoIP.

Прежде чем распределить данные по сети, они шифруются. Процесс самошифрования включает в себя разделение данных на маленькие кусочки и их последующее шифрование с помощью личной информации пользователя и самих данных. Это значит, что если кто-то захочет просмотреть данные, ему нужно будет знать личную информацию пользователя и специфику этих данных. Но эта информация не хранится в чужих системах или серверах.

Ообычно пользователь подключается к сети через роутер. Роутер использует протокол Reliable UDP (RUDP) (*рус. надежный UDP* ). RUDP более мощный, чем UDP, так как потерянные пакеты отправляются заново и они способны пересекать роутера NAT в отличии от TCP.

Typically the user connects to the network through a router. The router uses Reliable UDP (RUDP) protocol to connect to the network. RUDP is a more robust protocol than UDP as lost packets are retransmitted and are capable of traversing NAT routers, unlike TCP. The use of RUDP on the SAFE Network enables data to travel through a router without being corrupted or intercepted.

The user's Vault connects to other Vaults as part of the storage and management of data. The Vaults are constantly checked and ranked (by the Data holder managers personna) using the following criteria:

* **Availability** - how often the Vault is on or off
* **Storage** - how much storage space is in the Vault
* **CPU** - how much CPU resource of the Vault
* **Bandwidth** - how fast or slow the access is to the Vault

As demand and resources on the SAFE Network change, the Vaults adapt and continually balance the load of the network. This adjustment process is done automatically by the Vaults themselves. As the SAFE Network is completely autonomous, it can react quickly and without the need for any human intervention.

When a user provides more storage space than the amount they use on the SAFE Network, they are awarded safecoins at random by the network. Safecoins are required to access services on the network. The user can see how many safecoins they have by looking at their wallet. The wallet is automatically set up and configured as part of the MaidSafe client installation and sign up process.

App Builders earn safecoins when they create apps and programs for the SAFE Network. As the apps are used, the App Builder earns safecoins.

Core Developers can also be awarded safecoins by contributing to the SAFE Network codebase.
