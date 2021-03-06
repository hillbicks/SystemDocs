# Хранилища
Хранилище создается на компьютере пользователя, когда он устанавливает клиент SAFE и присоединяется к SAFE Network.

На своем компьютере вместо хранилища пользователь видит виртуальный монтированный диск, который предоставляет доступ к распределенным данным.

Когда пользователь создает или изменяет файлы на своем виртуальном диске, файл проходит через несколько процедур проверки, что он защищен и оптимально использует ресурсы SAFE Network.

## Менеджеры хранилища (personas)
В хранилище существует несколько менеджеров обработки данных. Каждый менеджер выполняет свою роль в SAFE Network.
* **Клиентский менеджер**<br/>
Клиентский менеджер принимает фрагменты самозашифрованных данных от хранилища пользователя.
* **Менеджер данных**<br/>
Он управляет фрагментами данных, принятых клиентским менеджером. Также он проверяет состояние SAFE Network.
* **Держатель данных**<br/>
Держатель данных используется для хранения фрагментов данных.
* **Менеджер держателя данных**<br/>
Этот менеджер следит за держателем данных. Он сообщает менеджеру данных о поломках или изменении данных, или если держатель данных выключается.
* **Менеджер хранилища**<br/>
Менеджер хранилища следит за тем, чтобы ПО было обновлено, чтобы хранилище работало, перезагружая его при ошибках.
* **Менеджер транзакций**<br/>
Менеджер транзакций управляет переводами safecoins.

## Данные в SAFE Network

Существует два механизма, используемых сетью, чтобы позволить конечному пользователю выполнять определенные действия через клиент SAFE. Разрешение выдается по решение группы консенсуса каждый раз, когда клиент хочет передать в сеть новые данные. В другом случае, если клиент изменяет уже хранимые данные или, например, передает safecoins, используются [цифровые подписи](https://ru.wikipedia.org/wiki/%D0%AD%D0%BB%D0%B5%D0%BA%D1%82%D1%80%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%B4%D0%BF%D0%B8%D1%81%D1%8C).

** Группа консенсуса**<br/>
Когда конечный пользователь хочет передать в сеть новые данные, файл шифруется и разбивается на фрагменты в процессе самошифрования, а затем отправляется близкой группе клиентских менеджеров. Эта близкая группа состоит из хранилищ, чьи ID ближе к ID пользовательского хранилища с точки зрения [XOR](https://ru.wikipedia.org/wiki/%D0%A1%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BF%D0%BE_%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D1%8E_2) расстояния. Оно высчитывается математически в отличии от географического расстояния. Как минимум двадцать восемь из тридцати двух хранилищ должны достичь консенсуса прежде чем любое действие в сети выполнится.

Клиентские менеджеры затем отправляют фрагменты тридцати двум менеджерам данных, выбираемых сетью на основании близости их ID к ID фрагментов данных, так что ID фрагмента тоже определяет свое местоположение в сети.

Сеть осуществляет разброс/сборку, основанную на [алгоритме разброса данных Рабина](http://people.seas.harvard.edu/~salil/rabin2011-slides/rabin2011-mitzenmacher.pdf), допуская небольшие потери данных (до четырех фрагментов) без необходимости передачи их заново.

Как только консенсус достигается, менеджер данных отправляет фрагменты тридцати двум менеджерам держателей данных, которые в свою очередь отправляют фрагменты на хранение держателям данных. Если менеджер держателей сообщает, что держатель выключился, менеджер данных решает, в какое хранилище отправить этот фрагмент, основываясь на рейтингах хранилищ.

Таким образом, фрагменты данных подлинного файла постоянно проверяются, гарантируя, что только владелец данных может получить и расшифровать свои подлинные данные.

Какая-либо передача фрагментов происходит только когда достигнут консенсус (28 из 32х) между окружающими хранилищами. Хранилище не может работать изолировано от всех.

Все действия в SAFE Network проходят проверку близкой группы из 32х узлов. Это предотвращает враждебные действия злоумышленников. Пользователь не может выбрать себе свой ID или решить, где будут храниться данные - все это решает сеть. Каждый раз, когда узел отключается от сети или переподключается, он получает новый произвольный ID.

[Просмотр видео, как работают хранилища](https://www.youtube.com/watch?v=txvKSeCaEP0)

** Цифровые подписи**<br/>
Когда конечные пользователи изменяют существующие данные, например, содержание файла, или отправляют другим пользователям safecoins, сеть не использует группу консенсуса, так как такой уровень сложности и увеличенная загруженность ей не нужны.

Цифровые подписи математически подтверждают владельца любых данных и могут доказать это вне всяких сомнений, при условии, что конечный пользователь сохранил в тайне свой частный ключ. Если пользователь является владельцем каких-либо данных и может доказать это, подписывая запрос своим частным ключом, сеть позволяет ему изменять данные.
