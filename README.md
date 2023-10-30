### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

----

### Выполнения задания 1

Выполним резервное копировани каталога пользователя в /tmp/backup/ :

```
 rsync -avc --delete --prorgess --exclude '.*' /home/byzgaev/ /tmp/backup/

```

![image.jpg](https://github.com/Byzgaev-I/Rsync-backup-copying/blob/main/1.png)

![image.jpg](https://github.com/Byzgaev-I/Rsync-backup-copying/blob/main/2.png)


----

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

----

### Выполнения задания 2


Резервное копирование домашней директории 

```
rsync -avc --delete --exclude '.*' /home/byzgaev/ /tmp/backup/

```

создаем скрипт /home/homework.sh и выполняем по расписанию каждый день

Далее настраиваем crontab

```
crontab -e

0 * 1-31 * * /home/homework.sh

```

![image.jpg](https://github.com/Byzgaev-I/Rsync-backup-copying/blob/main/3.png)

Скриптом проверяем вывод rsync и выводим информацию в системный log

```
#!/bin/bash

source=$HOME 
dest='/tmp/backup/'

rsync_sending=$(rsync -avc --delete --exclude '.*' $source $dest  2>&1 | sed  '/^#\|^$\| *#/d' | awk 'NR==1 {print $1}')

if [ -d "$dest" ]; then
        if [ $rsync_sending == "sending" ] ; then
                logger "backup created Successfully";
        else
                logger "backup no create";
        fi
else
        logger "backup dir not found";
fi

```

![image.jpg](https://github.com/Byzgaev-I/Rsync-backup-copying/blob/main/4.png)

![image.jpg](https://github.com/Byzgaev-I/Rsync-backup-copying/blob/main/5.png)














