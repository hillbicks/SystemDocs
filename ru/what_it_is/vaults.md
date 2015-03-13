# Хранилища (Vaults)
SAFE Network состоит из узлов (nodes), известных как хранилища (Vaults). Каждое хранилище располагается на жестком диске компьютера пользователя и выполняет множество функций, адаптируясь к нуждам сети. Одна из главных функций это хранение зашифрованных данных, которые попали в него из других хранилищ в сети.

Другая функция (persona) это умение общаться с другими хранилищами. Поскольку кусочки данных отправляются с узла на узел, определенная часть хранилища должна уметь определять целостность данных.
Another function, or persona, of a Vault is to manage other Vaults. As chunks of data are passed between Vaults the manager part of the Vault detects and monitors data integrity.

Еще у хранилищ может быть менеджер транзакций или Transaction validator persona. Этот менеджер занимается переводами safecoins.

Хранилища ничего не знают о данных, которые они запрашивают на хранение (информация может быть расшифрована только клиентом), и, так как эти данные на самом деле лишь кусочек чего-то целого, невозможно понять, частью чего они являются, например, документа или сообщения.

For every Vault that holds data there are another thirty one Vaults holding the same data. This means that if a Vault goes offline or a data chunk becomes corrupted, that data is not lost. Each Vault automatically finds another Vault to store the data.

Vaults are not visible to the users of the SAFE Network. Instead the user only sees a virtual mounted drive on their computer. With the Vaults constantly talking to, and monitoring each other, a user accesses data instantaneously from the mounted drive.
