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
[root@rocky-master ~]# rsync -av --delete --checksum --exclude '.*' ~/ /tmp/backup
```


---

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

### Решение Задание 2



---

## Задания со звёздочкой*
Эти задания дополнительные. Их можно не выполнять. На зачёт это не повлияет. Вы можете их выполнить, если хотите глубже разобраться в материале.

---

### Задание 3*
- Настройте ограничение на используемую пропускную способность rsync до 1 Мбит/c
- Проверьте настройку, синхронизируя большой файл между двумя серверами
- На проверку направьте команду и результат ее выполнения в виде скриншота

### Решение Задание 3*


---

### Задание 4*
- Напишите скрипт, который будет производить инкрементное резервное копирование домашней директории пользователя с помощью rsync на другой сервер
- Скрипт должен удалять старые резервные копии (сохранять только последние 5 штук)
- Напишите скрипт управления резервными копиями, в нем можно выбрать резервную копию и данные восстановятся к состоянию на момент создания данной резервной копии.
- На проверку направьте скрипт и скриншоты, демонстрирующие его работу в различных сценариях.

### Решение Задание 4*
------
