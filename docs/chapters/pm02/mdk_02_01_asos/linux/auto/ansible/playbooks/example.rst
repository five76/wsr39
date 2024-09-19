Примеры настройки playbooks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Пример 1. Создание локальных учетных записей
"""""""""""""""""""""""""""""""""""""""""""""

Создайте пользователя sshuser на хостах в сети br_net

- Пароль пользователя sshuser с паролем P@ssw0rd
- Идентификатор пользователя 1010
- Пользователь sshuser должен иметь возможность запускать sudo 
без дополнительной аутентификации.

::

- name: Make sure we have a 'wheel' group
  group:
    name: wheel
    state: present


- name: Add the user 'sshuser with a bash shell, appending the group 'admins' and 'developers' to the user's groups
  ansible.builtin.user:
    name: sshuser
    password: P@ssw0rd
    uid: 1010
    shell: /bin/bash
    groups: wheels
    append: yes
    createhome=yes


- name: Allow 'wheel' group to have passwordless sudo
  lineinfile:
    dest: /etc/sudoers
    state: present
    regexp: '^%wheel'
    line: '%wheel ALL=(ALL) NOPASSWD: ALL'
    validate: 'visudo -cf %s'

  - name: Set up authorized keys for the deployer user
    authorized_key: user=deployer key="{{item}}"
    with_file:
        - /home/railsdev/.ssh/id_rsa.pub


