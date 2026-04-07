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



