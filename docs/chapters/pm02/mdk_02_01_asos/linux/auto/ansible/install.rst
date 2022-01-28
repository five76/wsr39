Установка Ansible
======================

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

Требования к узлу управления
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для узла управления (машины, на которой работает Ansible) можно использовать любую машину с установленным Python 2 (версия 2.7) или Python 3 (версии 3.5 и выше). ansible-core 2.11 и Ansible 4.0.0 сделают Python 3.8 мягкой зависимостью для управляющего узла, но будут функционировать с учетом вышеупомянутых требований. Для работы на узле управления ansible-core 2.12 и Ansible 5.0.0 потребуется Python версии 3.8 или новее. Начиная с ansible-core 2.11, проект будет упакован только для Python 3.8 и новее. Это включает Red Hat, Debian, CentOS, macOS, любую из BSD и так далее. Windows не поддерживается для узла управления, подробнее об этом читайте в блоге Мэтта Дэвиса.

Требования к управляемым узлам
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для большинства управляемых узлов Ansible устанавливает соединение по SSH и передает модули с использованием SFTP. Если SSH работает, но SFTP недоступен на некоторых управляемых узлах, можно переключиться на SCP в ansible.cfg. Для любой машины или устройства, которые могут запускать Python, вам также понадобится Python 2 (версия 2.6 или более поздняя) или Python 3 (версия 3.5 или более поздняя).

Установка
~~~~~~~~~~

On Fedora
''''''''''

::

        $ sudo dnf install ansible

On RHEL:
''''''''''''''

::

        $ sudo yum install ansible


On CentOS:
''''''''''''

::
        
        $ sudo yum install epel-release
        $ sudo yum install ansible


Installing Ansible on Debian
''''''''''''''''''''''''''''''''''

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-debian




