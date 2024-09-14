Continue
============

Пользователь Ansible создаёт определённые «плейбуки» (playbook) в формате YAML с описанием требуемых состояний управляемой системы. «Плейбук» — это описание состояния ресурсов системы, в котором она должна находиться в конкретный момент времени, включая установленные пакеты, запущенные службы, созданные файлы и многое другое. Ansible проверяет, что каждый из ресурсов системы находится в ожидаемом состоянии и пытается исправить состояние ресурса, если оно не соответствует ожидаемому.

Для выполнения задач используется система модулей. Каждая задача представляет собой имя задачи, используемый модуль и список параметров, характеризующих задачу. Система Ansible поддерживает переменные, фильтры обработки переменных (поддержка осуществляется библиотекой Jinja2), условное выполнение задач, параллелизацию, шаблоны файлов. Адреса и настройки целевых систем содержатся в файлах «инвентаря» (inventory). Поддерживается группирование. Для реализации набора сходных задач существует система ролей. 



Подключение к удаленным узлам

Ansible взаимодействует с удаленными машинами по протоколу SSH. По умолчанию Ansible использует собственный OpenSSH и подключается к удаленным машинам, используя ваше текущее имя пользователя, так же, как и ОНА.
Действие: проверьте свои SSH-соединения

Подтвердите, что вы можете подключиться по SSH ко всем узлам в вашем инвентаре, используя одно и то же имя пользователя. При необходимости добавьте свой открытый SSH-ключ в файл authorized_keys в этих системах.

QuickStart
https://docs.ansible.com/ansible-core/devel/user_guide/intro_getting_started.html


Installing Ansible on RHEL, CentOS, or Fedora

On Fedora:

$ sudo dnf install ansible

On RHEL:

$ sudo yum install ansible

On CentOS:

$ sudo yum install epel-release



Installing Ansible on Ubuntu

Ubuntu builds are available in a PPA here.

To configure the PPA on your machine and install Ansible run these commands:

$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible

Add the following line to /etc/apt/sources.list or /etc/apt/sources.list.d/ansible.list:

deb http://ppa.launchpad.net/ansible/ansible/ubuntu MATCHING_UBUNTU_CODENAME_HERE main

Example for Debian 11 (Bullseye)

deb http://ppa.launchpad.net/ansible/ansible/ubuntu focal main

Then run these commands:

$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
$ sudo apt update
$ sudo apt install ansible


