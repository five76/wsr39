Общие сведения
""""""""""""""""

Группы серверов
~~~~~~~~~~~~~~~~~~~

Список групп серверов, которыми нужно управлять, Ansible может получать двумя основными способами:
- из специального текстового файла **hosts**;
- с помощью внешнего скрипта, возвращающего нужный нам список серверов, например из MongoDB. В официальном github-репозитории есть готовые скрипты для получения списка из Cobbler, Digital Ocean, EC2, Linode, OpenStack Nova, Openshift, Spacewalk, Vagrant, Zabbix.

Инвентарный файл (hosts)
~~~~~~~~~~~~~~

Инвентарный файл - это файл, в котором описываются устройства, к которым Ansible будет подключаться.

Дефолтное расположение файла 

::

        —/etc/ansible/hosts
        
но оно может также быть задано параметром окружения $ANSIBLE_HOSTS или параметром -i при запуске ansible и ansible-playbook. 

Содержимое этого файла может выглядеть, например, так (в квадратных скобках указаны имена групп управляемых узлов, ниже перечисляются входящие в эти группы серверы):

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


Помимо списка управляемых узлов, в файле hosts могут быть указаны и другие сведения, необходимые для работы: номера портов для подключения по SSH, способ подключения, пароль для подключения по SSH, имя пользователя, объединения групп и т.п. В некоторых случаях — в частности, при работе с большими и сложными конфигурациями, — различные параметры можно выносить в отдельные файлы и каталоги.

Один и тот же адрес или имя хоста, можно помещать в разные группы. 

При этом обычно лучше создавать свой инвентарный файл и использовать его. Для этого нужно, либо указать его при запуске ansible, используя опцию -i <путь>, либо указать файл в конфигурационном файле Ansible.

Если в группу надо добавить несколько устройств с однотипными именами, можно использовать такой вариант записи:

::

        [web-servers]
        192.168.20.[1:5] 


Такая запись означает, что в группу попадут устройства с адресами 192.168.20.1-192.168.20.5. 

Этот формат записи поддеживается и для имен хостов:

::

        [dbservers]
        one.example[A:D].com 
