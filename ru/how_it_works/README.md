# Как она работает

Когда пользователь скачивает и устанавливает клиент SAFE, происходит настройка хранилища на его компьютере.

После присоединения и регистрации в SAFE Network, пользователь увидит новый виртуальный диск на своем компьютере. На нем можно увидеть статические данные, зашифрованные и распределенные по другим хранилищам.

Хранилище к тому же может хранить и динамические данные, например, общение через приложение VoIP.

Прежде чем распределить данные по сети, они шифруются. Процесс самошифрования включает в себя разделение данных на маленькие фрагменты и их последующее шифрование с помощью личной информации пользователя и самих данных. Это значит, что если кто-то захочет просмотреть данные, ему нужно будет знать личную информацию пользователя и специфику этих данных. Но эта информация не хранится в чужих системах или серверах.

Ообычно пользователь подключается к сети через роутер. Он использует протокол Reliable UDP (RUDP) (*рус. надежный UDP* ). RUDP более мощный, нежели UDP, так как потерянные пакеты отправляются заново и могут проходить через роутеры NAT в отличии от TCP. Использование RUDP в SAFE Network позволяет данным передвигаться через роутеры без риска потери или перехвата.

Все хранилища соединены между собой, осуществляя хранение и управление данными. У каждого хранилища есть Менеджер хранения данных (Data holder managers persona), который постоянно проверяет хранилище и выставляет ему рейтинг на основании следующих критериев:

* **Доступность** - как часто хранилище доступно (онлайн)
* **Объем** - как много места в хранилище
* **Мощность ЦПУ** - какова мощность процессора
* **Пропускная способность** - скорость доступа к хранилищу

Так как спрос и ресурсы в SAFE Network постоянно изменяются, хранилища должны уметь адаптироваться и уравнивать загруженность сети. Этот процесс производится самими хранилищами автоматически. Поскольку SAFE Network полностью автономна, процесс проходит быстро без вмешательства человека.

Когда пользователь предоставляет больше памяти, чем он использует сам в SAFE Network, сеть награждает его safecoins, которые необходимы для получения услуг в сети. Пользователь видит, сколько safecoins у него есть в своем кошельке. Кошелек автоматически настраивается в процессе установки клиента SAFE и регистрации.

Разработчики приложений зарабатывают safecoins, когда создают свои программы для SAFE Network. Если приложение пользуется успехом, его разработчики получают safecoins.

Разработчики ядра сети тоже получают safecoins за участие в улучшении кода ядра.
