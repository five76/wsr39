Настройка подключение по SSH
"""""""""""""""""""""""""""""

Ansible взаимодействует с удаленными машинами по протоколу SSH. По умолчанию Ansible использует собственный OpenSSH и подключается к удаленным машинам, используя ваше текущее имя пользователя.


Подтвердите, что вы можете подключиться по SSH ко всем узлам в вашем инвентаре, используя одно и то же имя пользователя. При необходимости добавьте свой открытый SSH-ключ в файл authorized_keys в этих системах.

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
