Просмотр результата
~~~~~~~~~~~~~~~~~~~~

Рассмотрены способы, которые позволяют посмотреть на вывод, полученный с устройств.

Примеры используют модуль raw, но аналогичные принципы работают и с другими модулями.

register
""""""""

Параметр **register** сохраняет результат выполнения модуля в переменную. Затем эта переменная может использоваться в шаблонах, в принятии решений о ходе сценария или для отображения вывода.

::

	- hosts: all
	tasks:
		- name:
			command: df -h
			register: storage
		
		
Если запустить этот playbook, вывод не будет отличаться, так как вывод только записан в переменную, но с переменной не выполняется никаких действий. 

Следующий шаг - отобразить результат выполнения команды с помощью модуля debug.

debug
"""""

Модуль **debug** позволяет отображать информацию на стандартный поток вывода. Это может быть произвольная строка, переменная, факты об устройстве.

::

	- hosts: all
	tasks:
		- name:
			command: df -h
			register: storage
		- debug: var=storage.stdout_lines
		
Обратите внимание, что выводится не всё содержимое переменной **storage**, а только содержимое **stdout_lines**


Задание:
""""""""

В данном задании необходимо будет выполнить примеры из раздела "Ansible Playbook для простых и повседневных задач".

- Ansible установлен на BR-SRV - 192.168.0.2

- Управляемые хосты: BR-RTR - 192.168.0.1, BR-CLI - 192.168.0.10  

Выполнить `Ansible Playbook для простых и повседневных задач <https://vaiti.io/kak-ispolzovat-ansible-dlya-prostyh-i-slozhnyh-zadach>`__.

Обнаружение переменных: факты и магические переменные
"""""""""""""""""""""""""""""""""""""""""""""""""""""

С помощью Ansible можно извлекать или обнаруживать определенные переменные, содержащие информацию об удаленных системах или о самом Ansible.
Переменные, относящиеся к удаленным системам, называются **фактами**. С помощью **facts** можно использовать поведение или состояние одной системы в качестве конфигурации для других систем. 
Например, вы можете использовать IP-адрес одной системы в качестве значения конфигурации в другой системе. Переменные, связанные с Ansible, называются **магическими переменными**.

Сбор фактор по-умолчанию разрешен. Если необходимо отключить данное поведение, то необходимо указать **gather_facts : false**

::

	- name: "Name play"
	hosts: localhost
	gather_facts: false
	....

