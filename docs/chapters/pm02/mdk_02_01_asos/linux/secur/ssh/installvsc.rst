Установка Visual Studio Code  в Windows
==========================================

https://code.visualstudio.com/docs/cpp/config-wsl

1.	Установить Дополнительный компонент "Подсистема Windows для Linux" 

Выбрать **Панель управления** -> **Программы и компоненты** -> **Включение или отключение компонентов Windows** и установите флажок **Подсистема Windows для Linux**

.. figure:: instvsc/instvsc01.png
        :scale: 100%
        :align: center


.. figure:: instvsc/instvsc02.png
        :scale: 100%
        :align: center
        
        
.. figure:: instvsc/instvsc03.png
        :scale: 100%
        :align: center
        
2. Перезагрузить компьютер
3. Установиь **Visual Studio Code** (https://code.visualstudio.com/download)
4. Установить **Remote - WSL extension** (https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
5. Установить **Windows Subsystem for Linux**

* Запустить PowerShell от имени администратора

.. figure:: instvsc/instvsc04.png
        :scale: 100%
        :align: center

* Ввести команду 

::

        wsl --install –d Ubuntu-20.04
        
.. figure:: instvsc/instvsc05.png
        :scale: 100%
        :align: center

.. note:: Просмотр доступных ОС (**wsl --list --online** или **wsl -l -o**)

6. Открыть **Bash shell for WSL** (при установке запустится самостоятельно)

.. figure:: instvsc/instvsc06.png
        :scale: 100%
        :align: center

При запросе создать пользователя

* Login: student
* Password: <придумать>

7. Обновить систему

::

        sudo apt-get update
        
8. Установить отладчик gdb

::

        sudo apt-get install build-essential gdb
        
9. Настроить **git**

.. figure:: instvsc/gitname.png
        :scale: 100%
        :align: center

10. Сгенерировать ключ доступа по SSH для вашей ОС

.. figure:: instvsc/ssh-keygen.png
        :scale: 100%
        :align: center
        
При создании нажать несколько раз Enter (принять параметры по-умолчанию)

11. Добавить SSH-ключ на GitHub

Для добавления ключа надо его скопировать. Например, таким образом можно отобразить ключ для копирования:

::

        cat ~/.ssh/id_rsa.pub
        
После копирования надо перейти на GitHub. Находясь на любой странице GitHub, в правом верхнем углу нажмите на картинку вашего профиля и в выпадающем списке выберите **«Settings»**. В списке слева надо выбрать поле **«SSH and GPG keys»**. После этого надо нажать **«New SSH key»** и в поле **«Title»** написать название ключа (например «Home»), а в поле **«Key»** вставить содержимое, которое было скопировано из файла ~/.ssh/id_rsa.pub.

12. Создать каталог **oapisip**

::
        
        mkdir ~/oapisip
        cd ~/oapisip

13. Склонировать в данный каталог свой репозиторий

.. figure:: instvsc/gitclon.png
        :scale: 100%
        :align: center
        
14. Перейти в каталог  **exercises/03_linprogr/class/**

::

        cd exercises/03_linprogr/class/

или

::

        cd ~/oapisip/isip20_XX/exercises/03_linprogr/class/
        
15. Запустить **VS Code in WSL** из этого каталога

::
        
        code .

.. figure:: instvsc/instvsc07.png
        :scale: 100%
        :align: center

16. Install the C/C++ extension

.. figure:: instvsc/instvsc08.png
        :scale: 100%
        :align: center

17. Открыть в Code 

::

        cd ~/oapisip/isip20_XX/exercises/03_linprogr/home
        code .
        
18. Вписать в task_03_01.cpp

::

        #include<iostream>
        #include<cmath>

19. Сохранить
20. Отправить на github

::


        git add .
        git commit -m “Update task_03_01.cpp”
        git push origin main

.. note:: Для скачивания имеющихся обновлений с github в свой локальный репозиторий: **git pull**

     

        


