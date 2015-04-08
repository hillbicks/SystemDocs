# Эффективность - самоидентификация

SAFE Network поддерживает самоидентификацию. Это механизм, позволяющий пользователям создавать аккаунты в сети и заходить в них с любого компьютера или устройства без постороннего вмешательства.

Для этого право на идентификацию (маркер доступа) должно храниться в SAFE Network как часть процесса самоидентификации.

To achieve this the authentication privileges (login token) have to be stored on the SAFE Network as part of the self authentication process.

Пользователь создает свой ключ (К) и пароль (W), из которых создается [соль](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D0%BB%D1%8C_(%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F)).

Далее, для генерации уникального идентификатора, из соединенных вместе ключевого слова и соли создается хеш H(K + S).

[Password Based Key Derivation File](https://ru.wikipedia.org/wiki/PBKDF2) используется для усиления пароля. Это необходимо, потому что чаще всего выбранные пользователями пароли слабы.

В конце концов разрешение зашифрованного доступа хранится в SAFE Network с помощью следующей структуры:

Finally the encrypted access permission are stored on the SAFE Network using the following structure:

**Store On Network [H(K+S)] Symmetric Encrypt [ PBKDF2[P][S] ] (Account)**

Самоидентификация полагается на систему, в которой хранилище может создавать уникальный ключ для хранения значения в SAFE Network. Значение, хранящееся с этим ключом, должно содержать зашифрованный паспорт данных.

Self authentication relies on a system where a Vault can create a unique key, to store a value in the SAFE Network. The value stored with this key should contain an encrypted passport to data.

Этот паспорт содержит криптографически защищенные ключи и/или список других ключей, чтобы использовать хранимую или расшаренную информацию.

This passport contains cryptographically secure keys and/or a list of other keys to make use of the information to be stored or shared.

Местонаходжение ключа скрыто или по крайней мере неочевидно в SAFE Network. Этот подход является основой самоидентификации и в рамках SAFE Network позволяет данным храниться открыто без необходимости таких механизмов, как межсетевые экраны или контроли доступа.

The location of this initial key is masked or at least not obvious in the SAFE Network. This approach is the basis for self authentication and is extended into the SAFE Network to allow access to data to be stored publicly and with no additional requirement such as firewalls or access controls.

##Попытки угадать информацию доступа

Злоумышленник в сети проверяет наличие пользователя и пытается заполучить его маркер доступа, что напоминает долгие попытки угадать пароль на централизованном сервере. Такие серверы ослабевают эти атаки, запрещая вход на некоторое время после неудачных попыток входа.

An intruder or hostile person on the network can test for the presence of a user and download the login token. This is similar to continual testing of a password on a centralised server. In centralised servers this is alleviated by backing off log in attempts.

Когда пользователь запрашивает маркер доступа, SAFE Network предоставляет ему один. При каждой попытке сеть предоставляет неправильные маркеры. Это означает, что взломщику придется дешифровать много миллионов сильно зашифрованных маркеров. Этот процесс сделает такие атаки вычислительно невыполнимыми.

In the SAFE Network as a user requests a login token, they are provided with one. Invalid tokens are provided on every attempt. This means that an attacker will have to decrypt many millions of strongly encrypted log in tokens. This process should render such attacks computationally infeasible.

Маркер доступа нужен для идентификации после ее подтверждения в отличии от пароля и логина, которые никогда не отправляются в SAFE Network.

The login token is only useful for authentication after it is validated against the password and user details, which are never sent out onto the SAFE Network.
