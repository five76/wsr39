WireGuard
##########

**Туннелирование** (от англ. tunnelling — «прокладка туннеля») — процесс, 
в ходе которого создаётся логическое соединение между двумя конечными точками посредством инкапсуляции различных протоколов. 
Туннелирование представляет собой метод построения сетей, при котором один сетевой протокол инкапсулируется в другой. 
От обычных многоуровневых сетевых моделей (таких как OSI или TCP/IP) туннелирование отличается тем, 
что в качестве туннеля используется протокол более высокого, чем инкапсулируемый, уровня, либо протокол того же уровня.


**WireGuard** (проволочное ограждение) — коммуникационный протокол и бесплатное программное обеспечение с открытым исходным кодом, 
который реализует зашифрованные виртуальные частные сети (VPN). 
Разработан для простого использования технологии VPN, высокой производительности и низкой поверхности атаки. 
WireGuard нацелен на лучшую производительность и большую мощность, чем IPsec и OpenVPN, два других распространенных протокола туннелирования. 
Протокол WireGuard передаёт трафик по протоколу UDP.

WireGuard использует:

* Curve25519 для обмена ключами
* ChaCha20 для симметричного шифрования
* Poly1305 для кодов аутентификации посланий
* SipHash для ключей хеш-таблицы
* BLAKE2s для криптографической хеш-функции
* UDP.


WireGuard полностью поддерживает IPv6 как внутри, так и снаружи туннеля. Он поддерживает только третий уровень как для IPv4, так и для IPv6,
и может инкапсулировать v4-in-v6 и наоборот.

WireGuard поддерживает несколько топологий:

* Точка-точка (сеть)
* Звезда (сервер/клиент)
* Конечную точку клиента не нужно определять до того, как клиент начнет отправлять данные
* Конечные точки клиента могут быть статически предопределены
* Mesh (сеть)

В основе WireGuard лежит концепция под названием «маршрутизация криптоключей», которая работает путем связывания открытых ключей со списком IP-адресов туннеля, 
которым разрешено находиться внутри туннеля. Каждый сетевой интерфейс имеет закрытый ключ и список пиров. 
У каждого узла есть открытый ключ. Открытые ключи - короткие и простые и используются узлами для аутентификации друг друга. 
Их можно передавать для использования в файлах конфигурации любым внешним методом, аналогично тому, как можно отправить открытый ключ SSH другу для доступа к серверу оболочки.

WireGuard отправляет и получает зашифрованные пакеты, используя сетевое пространство имен, в котором изначально был создан интерфейс WireGuard. 
Это означает, что возможно создать интерфейс WireGuard в своем основном сетевом пространстве имен, имеющем доступ к Интернету, 
а затем переместить его в сетевое пространство имен, принадлежащее контейнеру Docker, в качестве единственного интерфейса этого контейнера.

Сначала создается VPN-туннель, которому назначаются приватный и публичный ключи, а также IP-адрес. 
Данный туннель пропускает только те устройства, которые имеют публичные ключи, которые известны туннелю, 
в то же время инкапсулируя их в UDP (другие сетевые протоколы WireGuard не поддерживает). 
Однако, для использования туннеля, клиенты должны ввести публичный ключ сервера. 
Сервер получает IP-адреса клиентов вместе с пакетами, в то время как клиенты должны знать его для отправки пакетов.

Таким образом, ключи при использовании WireGuard статичны и заранее установлены. 
Несмотря на кажущуюся простоту данного алгоритма, он обеспечивает дополнительный уровень симметричного шифрования для предотвращения потенциальных уязвимостей. 
Это устраняет риск взлома трафика, так как даже квантовые компьютеры пока не способны взломать Curve22519 – алгоритм обмена ключами.

На первом этапе подключения каждая сторона с помощью зашифрованного приватным ключом сообщения аутентифицируется (проверяется публичным ключом).

Второй этап — создание с помощью ключей  симметричных ключей для шифрования самого трафика. 
Благодаря тому что расшифровать зашифрованное публичным ключом нельзя без приватного, можно создать ключ для симметричного шифрования и отправить его по защищенному каналу. 
Симметричное шифрование гораздо менее ресурсоемкая операция и в процессе передачи трафика необходимо использовать именно его. 

Для защиты процесса обмена ключами симметричного шифрования  используется асимметричная схема. 
В WireGuardG используется ECDH — вариация Диффи — Хеллмана на эллиптических кривых. Первые два этапа в терминах WG называются рукопожатием или хендшейком.
Затем симметричные ключи используются для шифрования трафика. Раз в две минуты происходит новое рукопожатие и сессионные симметричные ключи меняются.

Ещё одной важной частью работы WireGuard является концепт маршрутизации Cryptokey Routing. 
Он заключается в том, что при маршрутизации пакетов WireGuard руководствуется публичным ключом пира. 
После расшифрования пакета аутентификации, сервер проверяет IP источника пакета на существовании его в списке разрешенных IP. 
Если он там есть, то WireGuard его пропускает, подписывает своим ключом, шифрует ключом пира, подобранного на основании dst поле пакета, и отправляет соответствующему получателю.

Кроме того, на GitHub существует версия WireGuard с российскими криптографическими алгоритмами. 
Проект RuWireGuard использует ГОСТ Р 34.10-2012, ГОСТ Р 34.12-2015 и ГОСТ Р 34.11-2012 [8]. 
Таким образом, даже набор криптографических алгоритмов в WireGuard можно выбрать и настроить.
 
Сравнение скорости WireGuard и OpenVPN
*****************************************

В `статье <https://help.ishosting.com/ru/openvpn-vs-wireguard>`__ приведены результаты тестирования.
При работе с портом 1 Гбит/с, пропускная способность WireGuard практически в 4 раза превышает пропускную способность своего конкурента – более 1000 Мбит/с против 258 Мбит/с. 
При тестах скорости OpenVPN (UDP) и WireGuard при подключении к порту со скоростью 350 Мбит/с через сервер в Великобритании. 

Таким образом, Wireguard по итогам нескольких опытов оказался быстрее OpenVPN в среднем на 94%

Связано это с тем, что в кодовой базе WireGuard содержится практически в 100 раз меньше строчек, чем у других протоколов – 4 000 строк у WireGuard 
против 500 000 у IPsec и 600 000 строк у OpenVPN соответственно. 
Это подтверждает, что код WireGuard является более оптимизированным и ресурсноэффективным. 
Кроме того, код WireGuard выполняется напрямую в ядре наравне с другими файлами ОС, в отличие от других сетевых протоколов.

