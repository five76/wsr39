сфы
###########################################

Для работы будет использоваться утилита **mdadm**

RAID 5 используется при наличии минимум 3 дисков.

.. figure:: img/mdadm_01.png
       :scale: 75%
       :align: center
       :alt: asda

Установка mdadm
****************

apt-get install mdadm

Сборка RAID
*************

Подготовка носителей
=======================

1. Проверить список дисков в системе:

.. code::

	lsblk
	
2. Занулить суперблоки на дисках, которые будут использовать для построения RAID 
(если диски ранее использовались, их суперблоки могут содержать служебную информацию о других RAID):


.. code::

	mdadm --zero-superblock --force /dev/sd{b,c,d}

Ответ:

.. code::

	mdadm: Unrecognised md component device - /dev/sdb
	mdadm: Unrecognised md component device - /dev/sdc
	mdadm: Unrecognised md component device - /dev/sdd

Означает, что диски не использовались в RAID

3.  Удалить старые метаданные и подпись на дисках:

.. code::

	wipefs --all --force /dev/sd{b,c,d}
	
Создание рейда
=================

4. Cобрать избыточный массив следующей командой:

.. code::

	mdadm --create --verbose /dev/md0 -l 5 -n 3 /dev/sd{b,c,d}

где:

* **/dev/md0** — устройство RAID, которое появится после сборки; 
* **-l 5** — уровень RAID 5; 
* **-n 3** — количество дисков, из которых собирается массив; 
* **/dev/sd{b,c,d}** — сборка выполняется из дисков sdb и sdc.

Примерный вывод:

.. code::

	mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
	
	mdadm: size set to 1046528K

Cистема задаст контрольный вопрос, необходимо ли продолжить и создать RAID — нужно ответить y:

.. code::

	Continue creating array? y

.. code::

	mdadm: Defaulting to version 1.2 metadata
	mdadm: array /dev/md0 started.

5. Проверить список дисков:


.. code::

	lsblk
	
	


6. Найти информацию о том, что у дисков sdb, sdc, sdd появился раздел md0, например:

.. figure:: img/mdadm_02.png
       :scale: 75%
       :align: center
       :alt: asda

В примере виден собранный raid5 из дисков sdb,sdc и sdd

Создание файла mdadm.conf
=============================

В файле **mdadm.conf** находится информация о RAID-массивах и компонентах, которые в них входят. 

7. Для его создания выполнить следующие команды:

.. code::

	mkdir /etc/mdadm
	echo "DEVICE partitions" > /etc/mdadm/mdadm.conf
	mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf

Пример содержимого:

.. figure:: img/mdadm_02.png
       :scale: 75%
       :align: center
       :alt: asda

* в данном примере хранится информация о массиве /dev/md0 — его уровень 5 , он собирается из 3-х дисков.

Создание файловой системы и монтирование массива
==================================================

8. Создать файловую систему **ext4** для массива. Выполняется также, как для раздела:

.. code::

	mkfs.ext4 /dev/md0


9. Создать каталог для монтирования:

.. code::

	mkdir /etc/raid5
 
10. Примонтировать раздел командой:

.. code::

	mount /dev/md0 /raid5



11. Обеспечить монтирование раздела при загрузке системы. Добавить в **fstab**

11.1 Получить идентификатор раздела (**UUID**):

.. code::

	blkid

.. figure:: img/mdadm_03.png
   :scale: 75%
   :align: center
   :alt: asda


11.2 Открыть **fstab** и добавить строку, вписав **свой UUID**:


.. code::

	vim /etc/fstab

.. figure:: img/mdadm_04.png
   :scale: 75%
   :align: center
   :alt: asda
   
Для разделения параметров использовать **TAB**

12. Отмонтировать раздел:

.. code::

	umount /raid5

13. Выполнить автомонтирование:

.. code::

	mount -a

14. Проверить примонтированный раздел md0:

.. code::

	df -h

.. figure:: img/mdadm_05.png
   :scale: 75%
   :align: center
   :alt: asda

**!!! Перезагрузить систему и снова проверить монтирование раздела!!!**


Информация о RAID
===================


15. Посмотреть состояние всех RAID можно командой:

.. code::

	cat /proc/mdstat
	
.. figure:: img/mdadm_06.png
   :scale: 75%
   :align: center
   :alt: asda

**Пояснение:**

* **md0** — имя RAID устройства; 
* **raid1 sdd[2] sdc[1] sdb[0]** — уровень избыточности и из каких дисков собран; 
* 1046528 blocks — размер массива; 
* **[3/3] [UUU]** — количество юнитов, которые на данный момент используются.


16. Получить подробную информацию о конкретном массиве командой:

.. code::

	mdadm -D /dev/md0

.. figure:: img/mdadm_07.png
   :scale: 75%
   :align: center
   :alt: asda


* где:

* Version — версия метаданных.
* Creation Time — дата в время создания массива.
* Raid Level — уровень RAID.
* Array Size — объем дискового пространства для RAID.
* Used Dev Size — используемый объем для устройств. Для каждого уровня будет индивидуальный расчет: RAID1 — равен половине общего размера дисков, RAID5 — равен размеру, используемому для контроля четности.
* Raid Devices — количество используемых устройств для RAID.
* Total Devices — количество добавленных в RAID устройств.
* Update Time — дата и время последнего изменения массива.
* State — текущее состояние. clean — все в порядке.
* Active Devices — количество работающих в массиве устройств.
* Working Devices — количество добавленных в массив устройств в рабочем состоянии.
* Failed Devices — количество сбойных устройств.
* Spare Devices — количество запасных устройств.
* Consistency Policy — политика согласованности активного массива (при неожиданном сбое). По умолчанию используется resync — полная ресинхронизация после восстановления. Также могут быть bitmap, journal, ppl.
* Name — имя компьютера.
* UUID — идентификатор для массива.
* Events — количество событий обновления.
* Chunk Size (для RAID5) — размер блока в килобайтах, который пишется на разные диски.

Подробнее про каждый параметр можно прочитать в мануале для mdadm:

man mdadm

Также, информацию о разделах и дисковом пространстве массива можно посмотреть командой fdisk:

.. code::
	
	fdisk -l /dev/md0

Проверка целостности
======================


Для проверки целостности ввести:

.. code::
	
	echo 'check' > /sys/block/md0/md/sync_action

Результат проверки смотреть командой:

.. code::

	cat /sys/block/md0/md/mismatch_cnt

* если команда возвращает 0, то с массивом все в порядке.

Остановка проверки:

.. code::

	echo 'idle' > /sys/block/md0/md/sync_action
	
	
Восстановление, удаление массива можно посмотреть в `статье <https://www.dmosk.ru/miniinstruktions.php?mini=mdadm>`__ 