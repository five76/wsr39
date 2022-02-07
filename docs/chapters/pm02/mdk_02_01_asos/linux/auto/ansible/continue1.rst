Общие сведения
""""""""""""""""

Группы серверов
~~~~~~~~~~~~~~~~~~~

Список групп серверов, которыми нужно управлять, Ansible может получать двумя основными способами:
* из специального текстового файла **hosts**;
* с помощью внешнего скрипта, возвращающего нужный нам список серверов, например из MongoDB. В официальном github-репозитории есть готовые скрипты для получения списка из Cobbler, Digital Ocean, EC2, Linode, OpenStack Nova, Openshift, Spacewalk, Vagrant, Zabbix.

Файл hosts
~~~~~~~~~~~~~~

Дефолтное расположение файла 

::

        —/etc/ansible/hosts
        
но оно может также быть задано параметром окружения $ANSIBLE_HOSTS или параметром -i при запуске ansible и ansible-playbook. Содержимое этого файла может выглядеть, например, так (в квадратных скобках указаны имена групп управляемых узлов, ниже перечисляются входящие в эти группы серверы):

::

        [centos]
        192.168.20.10 

        [dbservers]
        one.example.com 
        two.example.com
        three.example.com

        [dnsservers]
        rs1.example.com ansible_ssh_port=5555 ansible_ssh_host=192.168.1.50
        rs2.example.com
        ns[01:50].example.com


Помимо списка управляемых узлов, в файле hosts могут быть указаны и другие сведения, необходимые для работы: номера портов для подключения по SSH, способ подключения, пароль для подключения по SSH, имя пользователя, объединения групп и т.п. В некоторых случаях — в частности, при работе с большими и сложными конфигурациями, — различные параметры можно выносить в отдельные файлы и каталоги

**Информация об узлах (Facts)**

Перед внесением изменений Ansible подключается к управляемым узлам и собирает информацию о них: о сетевых интерфейсах и их состоянии, об установленной операционной системе и т.п. 

**Переменные**

Во время деплоя требуется не только установить какое-либо приложение, но и настроить его в соответствии с определенными параметрами на основании принадлежности к группе серверов или индивидуально (например, ip-адрес BGP-соседа и номер его AS или параметры для базы данных). Для обеспечения модульности разработчики:

* файлы с переменными групп хранятся в директории “group_vars/имя_группы”;
* файлы с переменными хостов в директории “hosts_vars/имя_хоста”;
* файлы с переменными роли в директории “имя_роли/vars/имя_задачи.yml”;


**Модули Ansible**

В состав Ansible входит огромное количество модулей для развёртывания, контроля и управления различными компонентами, которые можно условно разделить на следующие группы (в скобках приведены названия некоторых продуктов и сервисов):

* облачные ресурсы и виртуализация (Openstack, libvirt);
* базы данных (MySQL, Postgresql, Redis, Riak);
* файлы (шаблонизация, регулярные выражения, права доступа);
* мониторинг (Nagios, monit);
* оповещения о ходе выполнения сценария (Jabber, Irc, почта, MQTT, Hipchat);
* сеть и сетевая инфраструктура (Openstack, Arista);
* управление пакетами (apt, yum, rhn-channel, npm, pacman, pip, gem);
* система (LVM, Selinux, ZFS, cron, файловые системы, сервисы, модули ядра);
* работа с различными утилитами (git, hg).
* Помимо пользовательских переменных можно (и даже нужно) использовать факты, собранные ansible перед выполнением сценариев и отдельных задач.

Файлы конфигурации
~~~~~~~~~~~~~~~~~~~~~~~~
Основной файл конфигурации (по-усолчанию)

::

        /etc/ansible/ansible.cfg

Путь к файлу конфигурации
          
::

        ansible --version

Файлы конфигурации могт быть расположены в разных местах (применяется только один из них):

::

        /etc/ansible/ansible.cfg
        ~/.ansible.cfg





Continue
===========

Пользователь Ansible создаёт определённые «плейбуки» (playbook) в формате YAML с описанием требуемых состояний управляемой системы. «Плейбук» — это описание состояния ресурсов системы, в котором она должна находиться в конкретный момент времени, включая установленные пакеты, запущенные службы, созданные файлы и многое другое. Ansible проверяет, что каждый из ресурсов системы находится в ожидаемом состоянии и пытается исправить состояние ресурса, если оно не соответствует ожидаемому.

Для выполнения задач используется система модулей. Каждая задача представляет собой имя задачи, используемый модуль и список параметров, характеризующих задачу. Система Ansible поддерживает переменные, фильтры обработки переменных (поддержка осуществляется библиотекой Jinja2), условное выполнение задач, параллелизацию, шаблоны файлов. Адреса и настройки целевых систем содержатся в файлах «инвентаря» (inventory). Поддерживается группирование. Для реализации набора сходных задач существует система ролей. 




Подключение к удаленным узлам

Ansible взаимодействует с удаленными машинами по протоколу SSH. По умолчанию Ansible использует собственный OpenSSH и подключается к удаленным машинам, используя ваше текущее имя пользователя, так же, как и ОНА.
Действие: проверьте свои SSH-соединения

Подтвердите, что вы можете подключиться по SSH ко всем узлам в вашем инвентаре, используя одно и то же имя пользователя. При необходимости добавьте свой открытый SSH-ключ в файл authorized_keys в этих системах.


Пример

На компьютерах предприятии установлены ОС Centos и Debian.
Centos  - 192.168.0.10 - 192.168.0.20
Debian - 192.168.0.21 - 192.168.0.30
С помощью ansible, установленного на SRV (Centos8) создать сценарии автоматизации работы:

1) по установке программного обеспечения: lynx,vim,curl,tcpdump,wget

Решение:

1. На всех клиентах 

Обозначения:

SRV: Сервер ansible - компьютер с установленным ansible
Клиент - управляемое устройство

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

.. figure:: inv01.png
	:scale: 100%
	:align: center
	
.. figure:: inv02.png
	:scale: 100%
	:align: center



если необходимо разместить инвентраный файл в другом каталоге, то необходимо указать путь в /etc/ansible/ansible.cfg

Раздел [default]
* inventory=<Путь>
* Можно использовать $HOME

::
	[default]
	inventory = $HOME/hosts
	...
	

**Проверка связи:**

::
	
	ansible all -m ping

При установке связи может возникнуть ошибка при поиске интерпретатора команд на узлах.

Для устранения необходимо указать в инвентарном файле hosts:

Для Debian:

::

        ansible_interpreter=/usr/bin/python
        или
        ansible_interpreter=/usr/bin/python3

::

        [debian]
        5.5.5.2 ansible_interpreter =/usr/bin/python


Для Centos:

::

        interpreter_python=/usr/libexec/platform/python

Подробнее: 

https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html


**Модули Ansible: установка ПО**

::

	ansible.builtin.package:
		- name: <Имя пакета>
		- state: <Состояние>

	* Возможные состояния:

		present - установка
		absent - удаление

Пример:

::

	vim instsoft.yaml

::

	- name: Install software
	  hosts: centos
	  become:yes
	  tasks:
		- name: Install tcpdump
		ansible.builtin.package:
			name: tcpdump
			state: present
		- name: Remove firewalld
		  ansible.builtin.package
			name: firewalld
			state: absent

::

	ansible-playbook instsoft.yaml
	
	
.. note:: Возможне причины неправильной работы: запуск playbook без sudo, неправильное имя пакета, нет ключа 


::

	yum:
		name: <Имя пакета>
		state: <Состояние>
		enablerepo: <Репозиторий>
	Возможные состояния
		absent - удален
		present - установлен
		latest - обновлен
		
		
		
::

	- name: Install software
	  hosts: centos
	  become:yes
	  tasks:
		- name: Install Apache on C8
		  yum:
			name: httpd
			state: installed
		- name: Remove soft
		  yum:
			name: ﬁrewalld
			state: absent
			
**Ansible: управление службами	**	

::

        ansible.builtin.systemd:
		name: <Имя службы>
		state: <Состояние>
		enabled: < yes | no >
		daemon_reload: < yes | no >
	Cостояния
		reloaded / restarted /
		started / stopped
		Автозапуск: yes / no
		Обновление конфигурации			
		
Пример:

::
	
	- name: Install software
	  hosts: centos
	  become:yes
	  tasks:	
		- name: Start apache
		  ansible.builtin.systemd:
			name: httpd
			state: started
		- name: Ensure autostart BIND
		  ansible.builtin.systemd:
			name: named
			state: started
			enable: yes

**Проверка сценария**

--list-hosts

Перечисление хостов, на которых будет выполнен сценарий

--list-tasks

Перечисление задач к выполнению

--syntax-check

Проверка синтаксиса


**Применение переменных**

" {{ <Переменная> }} "

* Применяется в файлах сценария и шаблонах
* На момент вызова должна быть определена

Установка значения:

::

	ansible-playbook -e <Переменная>=<Значение>
	
Может быть указана в inventory

::

	[centos]
	ip1
	ip2
	ip3
	
	[centos:vars]
	target_package = httpd
	
	[debian]
	ip1
	ip2
	ip3
	
	[debian:vars]
	target_package = apache2
	
Использование:


::
	vim variablesoft.yaml
	
	- name: Install software
	  hosts: all
	  become:yes
	  tasks:
		- name: Install apache
		  ansible.builtin.package:
			name: "{{ target_package }}"
			state: present

::

	ansible-playbook variablesoft.yaml


