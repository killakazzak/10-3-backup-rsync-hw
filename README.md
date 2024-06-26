# Домашнее задание к занятию "`Резервное копирование`" - `Тен Денис`


### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

### Решение Задание 1

```bash
rsync -av --delete --checksum --exclude '.*' ~/ /tmp/backup
```
Вывод

```screenshot
[root@rocky-master ~]# rsync -av --delete --checksum --exclude '.*' ~/ /tmp/backup
sending incremental file list
created directory /tmp/backup
./
@
anaconda-ks.cfg
ci.jpg
dummy.jpg
http_8000.py
http_9000.py
http_index_6666.py
http_index_7777.py
http_index_8888.py
http_index_9999.py
index.html
index1.html
index3.html
key.json
kube-flannel.yml
kube-flannel.yml.1
kube-flannel.yml.2
kube-flannel.yml.3
posgtres.yml
roles.tar.gz
terraform.tfstate
v
ansible/
ansible/nginx.yml
git/
git/8-1-git-hw/
git/8-1-git-hw/README.md
git/8-1-git-hw/md-instruction.md
git/8-1-git-hw/screen-instruction.md
git/8-1-git-hw/img/
git/8-1-git-hw/img/img15.png
git/8-1-git-hw/img/img16.png
git/8-1-git-hw/img/img17.png
git/8-1-git-hw/img/img18.png
git/8-1-git-hw/img/img19.png
git/8-1-git-hw/img/img20.png
git/netology/
git/netology/README.md
git/netology/main.sh
git/netology/test.sh
terraform/
terraform/cloud-config.txt
terraform/main.tf
terraform/meta.txt
terraform/terraform.tfstate
terraform/terraform.tfstate.backup
yandex-cloud/
yandex-cloud/completion.bash.inc
yandex-cloud/completion.zsh.inc
yandex-cloud/path.bash.inc
yandex-cloud/bin/
yandex-cloud/bin/docker-credential-yc
yandex-cloud/bin/yc

sent 128,986,634 bytes  received 964 bytes  85,991,732.00 bytes/sec
total size is 128,951,063  speedup is 1.00
[root@rocky-master ~]#
[root@rocky-master ~]#
[root@rocky-master ~]#
[root@rocky-master ~]# ll /tmp/backup/
total 212
-rw-r--r--. 1 root root  2150 Apr  4 04:31 @
-rw-------. 1 root root  1026 Feb  9 15:19 anaconda-ks.cfg
drwxr-xr-x. 2 root root    23 Feb 28 04:53 ansible
-rw-r--r--. 1 root root 49532 Apr  4 05:08 ci.jpg
-rw-r--r--. 1 root root 62453 Apr  4 05:08 dummy.jpg
drwxr-xr-x. 4 root root    40 Feb 28 19:57 git
-rwxr-xr-x. 1 root root   251 Apr  1 19:45 http_8000.py
-rwxr-xr-x. 1 root root   251 Apr  1 19:47 http_9000.py
-rwxr-xr-x. 1 root root   588 Apr  4 05:29 http_index_6666.py
-rwxr-xr-x. 1 root root   588 Apr  4 05:29 http_index_7777.py
-rwxr-xr-x. 1 root root   589 Apr  4 05:29 http_index_8888.py
-rwxr-xr-x. 1 root root   589 Apr  4 05:14 http_index_9999.py
-rw-r--r--. 1 root root    15 Apr  4 05:31 index1.html
-rw-r--r--. 1 root root    27 Apr  4 05:17 index3.html
-rw-r--r--. 1 root root    15 Apr  4 05:31 index.html
-rw-------. 1 root root  2491 Feb 28 05:36 key.json
-rw-r--r--. 1 root root  4468 Mar 18 10:27 kube-flannel.yml
-rw-r--r--. 1 root root  4468 Mar 18 10:27 kube-flannel.yml.1
-rw-r--r--. 1 root root  4468 Mar 18 10:27 kube-flannel.yml.2
-rw-r--r--. 1 root root  4468 Mar 18 10:27 kube-flannel.yml.3
-rw-r--r--. 1 root root   402 Feb 28 05:15 posgtres.yml
-rw-r--r--. 1 root root    45 Feb 18 14:04 roles.tar.gz
drwxr-xr-x. 2 root root   118 Feb 28 05:23 terraform
-rw-r--r--. 1 root root   180 Feb 27 17:39 terraform.tfstate
-rw-r--r--. 1 root root   176 Feb 18 14:04 v
drwxr-xr-x. 3 root root    91 Feb 28 05:23 yandex-cloud

```


---

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

### Решение Задание 2
Создаём скрипт на РК
```
vim backup.sh
```
```bash
#!/bin/bash

source_dir="/root/"
target_dir="/tmp/backup/"

rsync -av --delete "$source_dir" "$target_dir" > /dev/null  2>&1


if [ $? -eq 0 ]; then
    echo "Резервное копирование успешно завершено"
else
    echo "Ошибка при выполнении резервного копирования"
fi
```
Установка прав на запуск
```
chmod +x backup.sh
```
Редактируем crontab
```bash
crontab -e
0 0 * * * /backup.sh >> /var/log/messages 2>&1
```
Проверка

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/0ad283dc-f1f4-431d-95e1-9bb46dbfb95b)

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/5f92fa12-b5f2-489c-aeb8-bc9386fc5a30)


---

## Задания со звёздочкой*
Эти задания дополнительные. Их можно не выполнять. На зачёт это не повлияет. Вы можете их выполнить, если хотите глубже разобраться в материале.

---

### Задание 3*
- Настройте ограничение на используемую пропускную способность rsync до 1 Мбит/c
- Проверьте настройку, синхронизируя большой файл между двумя серверами
- На проверку направьте команду и результат ее выполнения в виде скриншота

### Решение Задание 3*

```bash
rsync -av --delete --checksum --exclude '.*' --bwlimit=128  ~/ root@10.159.86.73:/tmp/backup
```

Проверка

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/adab3043-4a50-417d-9e51-277801d58e1c)


---

### Задание 4*
- Напишите скрипт, который будет производить инкрементное резервное копирование домашней директории пользователя с помощью rsync на другой сервер
- Скрипт должен удалять старые резервные копии (сохранять только последние 5 штук)
- Напишите скрипт управления резервными копиями, в нем можно выбрать резервную копию и данные восстановятся к состоянию на момент создания данной резервной копии.
- На проверку направьте скрипт и скриншоты, демонстрирующие его работу в различных сценариях.

### Решение Задание 4*

```bash
#!/bin/bash
HOME_DIR=/root
BACKUP_DIR=root@10.159.86.73:/root/backup_data
TIMESTAMP=$(date +%Y-%m-%d_%H-%M-%S)

# Функция для выполнения инкрементного резервного копирования
perform_backup() {
                rsync -avzhHl /root/source_data $BACKUP_DIR/backup-$TIMESTAMP
                ssh root@10.159.86.73 'cd /root/backup_data  && ls -t . | tail -n +6 | xargs rm -r'
                echo "Резервное копирование завершено успешно!"

}

# Функция для восстановления данных из резервной копии
restore_backup() {
    # Получаем список доступных резервных копий
    backups=$(ssh root@10.159.86.73 "ls -t /root/backup_data")

    if [ -z "$backups" ]; then
        echo "Нет доступных резервных копий."
    else
        echo "Доступные резервные копии:"
        echo "$backups"
        read -p "Выберите номер резервной копии для восстановления: " backup_number
        rsync -av --delete $BACKUP_DIR/$backup_number/* $HOME_DIR
        #scp -r $BACKUP_DIR/$backup_number/*  $HOME_DIR
        echo "Восстановление данных завершено успешно!"
    fi
}

# Основное меню скрипта
while true; do
    echo "1. Выполнить резервное копирование"
    echo "2. Восстановить данные из резервной копии"
    echo "3. Выйти"
    read -p "Выберите опцию: " option

    case $option in
        1) perform_backup ;;
        2) restore_backup ;;
        3) exit ;;
        *) echo "Некорректный выбор. Попробуйте снова." ;;
    esac

    echo
done
```

Проверка на ошибку (отсутсвуют бэкапы)

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/efd4666d-5b98-40e0-b40b-12ec025f04de)

Создание бэкапа

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/7da79a85-a26d-4408-8639-8efa3880291e)

Проверка создания бэкапа и удаление старых копий

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/9355b9d3-ccb4-4af9-98ec-e9e50d53d5ea)

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/352dcd94-78ef-4a22-ad65-d43b5b97ea22)

Восстановление из бэкапа

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/39ad3d43-e289-4c64-a709-37dc8c02b2c6)

Проверка восстановления и синхронизация файлов

![image](https://github.com/killakazzak/10-3-backup-rsync-hw/assets/32342205/a968899a-797a-4bf1-82f8-de993fb1db1f)










------
