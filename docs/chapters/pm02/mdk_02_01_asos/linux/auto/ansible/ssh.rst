Настройка подключение по SSH
"""""""""""""""""""""""""""""

::

	vim /etc/ssh/sshd_config
	Port 22
	PermitRootLogin yes
	PubkeyAuthentication yes
	AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2

	systemctl restart sshd

если необходимо 

::

	setenforce 0
	
Создать на клиентах и сервере (на котором установлен  ansible) учетную запись, под которой будет выполняться управление работой (например **ansible**):

::
	useradd ansible
	passwd ansible
	
Дать право пользователю **ansible** на беспарольный запуск sudo команд

::

	visudo

	ansible ALL=(ALL) NOPASSWD:ALL
	
Сгенерировать ключ на сервере

::

	ssh-keygen-t rsa -b 4096
	
Скопировать ключ на клиентов

::

	ssh-copy-id ip_client
	
Заполнить инвентарный файл (используется при выполнении сценариев)

по умолчанию /etc/ansible/hosts

::

	[centos]
	ip1
	ip2
	ip3
	FQDN1
	FQDN2
	FQDN3

	[debian]
	ip1
	ip2
	ip3

	FQDN1
	FQDN2
	FQDN3


группы **all** и **ungrouped** - группы по умолчанию *Все и “все, кто не в группе”*
