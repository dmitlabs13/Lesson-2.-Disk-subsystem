# Lesson-2. Домашнее задание: работа с mdadm

## Задание
• Добавить в виртуальную машину несколько дисков
• Собрать RAID-0/1/5/10 на выбор
• Сломать и починить RAID
• Создать GPT таблицу, пять разделов и смонтировать их в системе.


## Выполнение
Подключил 5 дополнительных дисков
```
sadmin@lp-ubn4:~$ lsblk
NAME                      MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0  40G  0 disk
├─sda1                      8:1    0   1M  0 part
├─sda2                      8:2    0   2G  0 part /boot
└─sda3                      8:3    0  38G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0    0  19G  0 lvm  /
sdb                         8:16   0   2G  0 disk
sdc                         8:32   0   2G  0 disk
sdd                         8:48   0   2G  0 disk
sde                         8:64   0   2G  0 disk
sdf                         8:80   0   2G  0 disk
```

За нулим супер блоки, на всякий случай
```
sadmin@lp-ubn4:~$ sudo mdadm --zero-superblock --force /dev/sd{b,c,d,e,f}
[sudo] password for sadmin:
mdadm: Unrecognised md component device - /dev/sdb
mdadm: Unrecognised md component device - /dev/sdc
mdadm: Unrecognised md component device - /dev/sdd
mdadm: Unrecognised md component device - /dev/sde
mdadm: Unrecognised md component device - /dev/sdf
```

Выбираем RAID5
```
sadmin@lp-ubn4:~$ sudo mdadm --create --verbose --force /dev/md0 -l 5 -n 5 /dev/sd{b,c,d,e,f}
mdadm: layout defaults to left-symmetric
mdadm: layout defaults to left-symmetric
mdadm: chunk size defaults to 512K
mdadm: size set to 2094080K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```
--create создать массив
--verbose подробный вывод выполнения
/dev/md0 - имя массива
-l уровень массива
-n количиство дисков

Проверим, что массив создался
```
sadmin@lp-ubn4:~$ cat /proc/mdstat
Personalities : [raid0] [raid1] [raid4] [raid5] [raid6] [raid10] [linear]
md0 : active raid5 sdf[4] sde[3] sdd[2] sdc[1] sdb[0]
      8376320 blocks super 1.2 level 5, 512k chunk, algorithm 2 [5/5] [UUUUU]

unused devices: <none>


sadmin@lp-ubn4:~$ mdadm -D /dev/md0
mdadm: must be super-user to perform this action
sadmin@lp-ubn4:~$ sudo mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Wed Apr  8 16:23:04 2026
        Raid Level : raid5
        Array Size : 8376320 (7.99 GiB 8.58 GB)
     Used Dev Size : 2094080 (2045.00 MiB 2144.34 MB)
      Raid Devices : 5
     Total Devices : 5
       Persistence : Superblock is persistent

       Update Time : Wed Apr  8 16:23:14 2026
             State : clean
    Active Devices : 5
   Working Devices : 5
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : lp-ubn4:0  (local to host lp-ubn4)
              UUID : 4c349e5e:7d4935c2:e2a6222e:9999bdc5
            Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       2       8       48        2      active sync   /dev/sdd
       3       8       64        3      active sync   /dev/sde
       4       8       80        4      active sync   /dev/sdf


```
*выключил машину и отключил один из дисков*





