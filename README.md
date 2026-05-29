# Домашнее задание к занятию "`Резервное копирование`" - `Александров Денис`

---
### Задание 1

Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup

Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые).

Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.

На проверку направить скриншот с командой и результатом ее выполнения.

### Ответ: 

### Решение 1

Создаем папку backup в директории /tmp

### sudo mkdir backup

![1.1](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/9-1.jpg)

Создание зеркальной копии домашней директории текущего пользователя в директорию /tmp/backup

### Команда: 

### sudo rsync -ahavP --include '${HOME}' --exclude='.*' /home/${USER} /tmp/backup

-а --arhive: копирование в режиме отправки файлов с настройками по умолчанию, которые приближены к команде "cp -a"

-h --human-readable: список файлов в легко читаемом формате 

-v --verbose: сообщать каждую операцию при выполнении

-P --progress: отображение прогресса работы

![1.2](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/9-2.jpg)

![1.3](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/9-3.jpg)

Rsync подсчитывает хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.

### Команда:
### rsync -ahavP --checksum --exclude=".*" ~/ /tmp/backup

![1.4](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/9-4.jpg)




### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.
------

![2.1](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/1-9.jpg)

![2.2](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/2-9.jpg)


### /usr/local/bin/backup.sh


      echo "$(date +%Y-%m-%d_%H:%M:%S) - Starting the backup of the home directory" >> /tmp/backup/alex-den/backup.log

      rsync -ahavP --checksum --exclude=".*" /home/alex-den/ /tmp/backup/ > /dev/null 2>> /tmp/backup/alex-den/backup.log

      if [ $? -eq 0 ]; then

        echo "Backup was completed successfully" >> /tmp/backup/alex-den/backup.log

      else

        echo "Backup completed with an error" >> /tmp/backup/alex-den/backup.log

      fi



Backup записывается в файл:

### /tmp/backup/vm/backup.log


![2.3](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/5-9.jpg)

![2.4](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/3-9.jpg)




### crontab -e

Выполним изменения и добавим в конец файла:

### 00 00 * * * /usr/local/bin/backup.sh

Это означает, что выполнение скрипта backup.sh будет производиться каждый день каждого месяца в 00:00.


### crontab -e

Показывает все сценарии автоматического резервирования.

### Синтаксис: 

 ММ  ЧЧ  ДД  МТ ВТ Команда

-  мм минута 0-59
-  чч час 0-23
-  дд день месяца 1-31
-  мт месяц 1-12
-  вт день недели 0-7 (воскресенье = 0 или 7)
-  команда: то, что мы хотим выполнить, все числовые значения можно заменить на " * "

------
