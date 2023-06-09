# Linux. ДЗ №3.
## Права доступа и безопасность

### Создать два произвольных файла.
 - mkdir homework03
 - cd homework03/
 - touch file1 file2

### Первому присвоить права на чтение и запись для владельца и группы, только на чтение — для всех. Второму присвоить права на чтение и запись только для владельца. Сделать это в численном и символьном виде.
 - sudo chmod u=rw,g=rw,o=r file1
 - sudo chmod u=rw,g=r,o=r file2
 - sudo chmod 664 file1
 - sudo chmod 644 file2

### Назначить новых владельца и группу для директории целиком.
 - sudo chown -R ivan:group-test01 /home/roman/homework03/
## Управление пользователями
### Cоздать пользователя, используя утилиту useradd и adduser.
 - sudo useradd -s /bin/bash -m -d /home/user7 user7
 - sudo adduser user9

### Удалить пользователя, используя утилиту userdel
 - sudo userdel user9
## Управление группами
### Cоздать группу с использованием утилит groupadd и addgroup
 - sudo groupadd group-test03
 - sudo addgroup group-test02

### Попрактиковаться в смене групп у пользователей
 - sudo usermod -aG group-test02 user7
 - sudo usermod -g group-test03 user7

### Добавить пользователя в группу, не меняя основной
- sudo usermod -aG group-test02 user5

### Создать пользователя с правами суперпользователя.
 - sudo useradd -s /bin/bash -m -d /home/userAdmin userAdmin
 - sudo passwd userAdmin
 - sudo usermod -aG sudo userAdmin

### Сделать так, чтобы sudo не требовал пароль для выполнения команд.
- sudo visudo

Добавляем **NOPASSWD** в группу sudo:

 - *%sudo   ALL=(ALL:ALL)NOPASSWD:ALL*

## Дополнительные (необязательные) задания
### Используя дополнительные материалы, выдать одному из созданных пользователей право на выполнение ряда команд, требующих прав суперпользователя (команды выбираем на своё усмотрение)

Синтаксис для редактирования **sudoers**:

*[пользователь] [хост]=([кем может стать]) [что может сделать]*

**user7 ALL=(root) /bin/apt-get update**

Теперь пользователь **user7** может выполнить с правами **root** команду **sudo apt-get update**

### Создать группу developer и нескольких пользователей, входящих в неё. Создать директорию для совместной работы. Сделать так, чтобы созданные одними пользователями файлы могли изменять другие пользователи этой группы.
Создаём нескольких пользователей:
 - sudo useradd -s /bin/bash -m -d /home/alex alex
 - sudo passwd alex
 - sudo useradd -s /bin/bash -m -d /home/petr petr
 - sudo passwd petr
 - sudo useradd -s /bin/bash -m -d /home/ivan ivan
 - sudo passwd ivan

Смотрим в каких группа пользователи:
 - sudo groups ivan alex petr

    ivan : ivan

    alex : alex

    petr : petr

Создаём новую группу:
 - sudo groupadd developer

Добавляем в группу **developer** новых пользователей:
 - sudo usermod -aG developer ivan
 - sudo usermod -aG developer petr
 - sudo usermod -aG developer alex

Проверяем состав группы **developer**:
 - cat /etc/group | grep developer

    developer:x:1006:ivan,petr,alex

Создаём директорию для работы пользотелей группы developer и выдаём права:
 - sudo mkdir /media/dev
 - sudo chgrp -R developer /media/dev/
 - sudo chmod 2774 /media/dev

Теперь в /media/dev группа пользователей, владеющая всеми файлами в директории - developer (группа пользователей, владеющая директорией)

drwxrwsr-- 2 root developer 4096 апр  4 11:46 ./
### Создать в директории для совместной работы поддиректорию для обмена файлами, но чтобы удалять файлы могли только их создатели.
- sudo mkdir /media/dev/share
- sudo chmod -R 1775 /media/dev/share
### Создать директорию, в которой есть несколько файлов. Сделать так, чтобы открыть файлы можно было, только зная имя файла, а через ls список файлов посмотреть было нельзя.
- sudo mkdir /media/dev/sec
- sudo chmod -R 2333 /media/dev/sec




