**Данный проект предназначен для завершения процессов на удалённых серверах.**

В локальном **`inventoy.ini`** требуется задать список необходимых IP адресов серверов, а также изменить ***`ansible_user`*** на своего. 

Если получаете ошибку в момент выполения плейбука, то сначала воспользуйтесь плейбуком из проекта [create_ssh_users](https://github.com/sherbettt/create_ssh_users), т.к. именно в нём создаются пользователя и ключи SSH. 

Имитация:
На необходимых серверах запустить несколько процессов sngrep и вывести в background (bg) комбинацией *`Ctrl + Z`*, после проверить джобы.
```bash
# запускаем sngrep
root@msk-vps3:~# sngrep

[1]+  Остановлен    sngrep
root@msk-vps3:~# sngrep

[2]+  Остановлен    sngrep
root@msk-vps3:~# jobs -l
[1]- 3725536 Остановлено  sngrep
[2]+ 3726439 Остановлено  sngrep

# выполнить поиск процесса
root@msk-vps3:~# ps aux | grep -E 'sngrep.*$' 
root     3725536  0.1  0.0  94648 10132 pts/0    Tl   12:17   0:00 sngrep
root     3726439  0.1  0.0  94648 10228 pts/0    Tl   12:18   0:00 sngrep
root     3727221  0.0  0.0   6404   648 pts/0    S+   12:19   0:00 grep --color=always -E sngrep.*$
```
И если процессов немного, можно одну из джоб вернуть в fg по номеру.
```bash
fg %1    # для задания [1]
# или
fg %2    # для задания [2]
# по PID процесса
fg %?3726439   
```
После чего можно воспользоваться плейбуком.

Примеры запуска:
```bash
# типовой запуск
ansible-playbook kp_multiple.yml -i inventory.ini -v

# с дебагом
ANSIBLE_FORCE_COLOR=1 ansible-playbook kp_multiple.yml -i inventory.ini -vvv 2>&1 | tee debug_$(date +%Y%m%d_%H%M%S).log
```



