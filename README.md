<h2><b> Работа с Сетью  </b></h2>
`ping`  отправка ICMP пакетов на хост 
`traceroute` утилита для проверки проходжения пакетов через роутеря к конечному хосту использует UDP пакеты 
Она отправляет пакет с TTL=1 и смотрит адрес ответившего узла, дальше TTL=2, TTL=3 и так пока не достигнет цели. Каждый раз отправляется по три пакета и для каждого из них измеряется время прохождения.  
```
root@dgm:~# traceroute -I sbercloud.ru (-I, --icmp Use ICMP ECHO for probes)
traceroute to sbercloud.ru (178.248.232.192), 30 hops max, 60 byte packets
 1  node193-msk1.cloudvps.reg.ru (37.140.193.6)  0.383 ms  0.348 ms  0.341 ms
 2  kiae-r1.hosting.reg.ru (31.31.194.4)  0.411 ms  0.406 ms  0.400 ms
 3  * * *
 4  150-192-212-88.host.exepto.ru (88.212.192.150)  0.390 ms  0.411 ms  4.449 ms
 5  mskn17.mskn202.transtelecom.net (188.43.19.222)  4.442 ms  4.435 ms  4.428 ms
 6  10.99.99.9 (10.99.99.9)  4.421 ms  1.127 ms  1.522 ms
 7  HLL-gw.transtelecom.net (188.43.15.237)  0.918 ms  0.912 ms  0.900 ms
 8  178.248.232.192 (178.248.232.192)  0.861 ms  0.856 ms  0.880 ms
root@dgm:~#
```


`mtr -n sedicomm.com` аналог трейроута -n покажет ip маршрутирзаторов -b итотито

`ip a` - отображение сетевых интерфейсов с данными.

`ss` - какие сетевые подключения Linux открыты, какие IP адреса используются или какие порты прослушиваются.  `ss -t` - только TCP  
`netstat -tulpn` Отображение сервис айпи -порт - открытые порты порт принимает вх пакеты из инета     

`telnet` уд. упр. компом , старый в основном ссх сейчас, для проверки портов(на упрвлемой тачке долн быь telnet-server)  
```
root@dgm:~# telnet opennet.ru 80
Trying 217.65.3.21...
Connected to opennet.ru.
Escape character is '^]'.
^] (нажать ctrl+[)
telnet>
GET /
<HTML>
```
 `netcat -u` подключения оболочки к порту, установления соединений TCP/UDP
```
nc [options] host port -u - сканировать порты UDP 
nc: connect to 194.67.91.196 port 440 (tcp) failed: Connection refused
root@dgm:~# nc -zvn  lol.ru   21 25 80 440
nc: getaddrinfo for host "lol.ru" port 21: Name or service not known
root@dgm:~# nc -zv  lol.ru   21 25 80 440
nc: connect to lol.ru port 21 (tcp) failed: Connection refused
Connection to lol.ru 25 port [tcp/smtp] succeeded!
Connection to lol.ru 80 port [tcp/http] succeeded!
nc: connect to lol.ru port 440 (tcp) failed: Connection refused
root@dgm:~#
root@dgm:~# nc -C hackware.ru 80
GET / HTTP/1.0
Host: hackware.ru
HTTP/1.1 302 Found
Server: nginx
Date: Thu, 02 Dec 2021 12:42:42 GMT
Content-Type: text/html; charset=iso-8859-1
Content-Length: 204
Connection: close
Location: https://hackware.ru/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://hackware.ru/">here</a>.</p>
</body></html>
root@dgm:~#
```
https://routerus.com/netcat-nc-command-with-examples/  


`curl`-для конектов к серву по разным протоколам с синткс  урл Libcurl либа для курла (апи)  
StringIO и BytesIO из import io временные файлы, которые не нужно постоянно хранить на диске, можно использовать в памяти, надо закрыть освобожденное пространство памяти вовремя после использования . (читаь из оп быстрее чем открывать файл с хд)  
`curl -o /dev/null -s https://sbercloud.ru/ru --write-out '%{time_total}\n'`  - общее время, которое потребовалось для успешного выполнения запроса  
`root@naagapov-zabbix-db-serv-proxy-ag2:~# curl -o /dev/null -s sbercloud.ru --write-out '%{num_redirects}\n' -L` кол-во редиректов (2 т.к хттп + ведет на /ru
2  

https://qastack.ru/programming/18215389/how-do-i-measure-request-and-response-times-at-once-using-curl  
https://mcs.mail.ru/blog/10-komand-curl-kotorye-vam-sleduet-znat  
https://habr.com/ru/company/ruvds/blog/568614/  


` wget `-  работа с URL страницами. для конектов к серву по разным протоколам с синттк сурл.
`wget -b -o ~/wget.log http://ftp.gnu.org/gnu/wget/wget-1.5.3.tar.gz`  


`bind`  - DNS сервер настройки в  статье https://www.k-max.name/linux/howto-dns-server-bind/ 
`dig` - DNS (ttl на днс лучше выставлять менее 600 с. чтобы обновляись записи быстрее, но общее время зависит кнч от внешних днсов)
`dig google.com +noall` Резолв (resolve) — преобразование из имени в IP-адрес и наборот. 
 опрос DNS-серверов https://losst.ru/komanda-dig-v-linux  

 `host -t A linux-faq.ru` - -t тип записи, для отправки запросов серверам доменных имен.  
 `host linux-faq.ru ns1.r01.ru` использование определенного сервера доменных имен  
 

`lsof`  утилита, служащая для вывода информации о том, какие файлы используются теми или иными процессами (если ошибка “Файлы используются“)  
```
lsof -i TCP:22
 lsof -i TCP:1-1024
 запущенные процессы на определенном порту
# lsof -i -u^root
Исключение пользователей окрытых файлов
# lsof -i
всех сетевых подключений
```

<h2><b>Работа с Дисками  </b></h2>
<h2><b> Работа С Git   </b></h2>

`git init` - инициализация пустого репозитория(. (точка значит скрытая папка, где хранится история измекнений файлов)).  
`git status` - просмотр статуса репозитория  
`git add` - добавитьтрекать истории изщменения файла (если модифайд - в комит не уйдет, нужно иммено после изменений делать git add)  
`git commit -m '1st comit' ` комит файлов  
`git rm` - удалить файл из гита  
`git rm --cached `удалить из истории гита, первод в антркед статус  
`HEAD` - последний коммит в истории  
`git log` Отображение последних комитов  
`git checkout ID` Возврат по ID к старому комиту (первые 4 символа из git log коммита)  
Ветвление  
`git checkout -b brch1 `Создание новой ветки  
`git checkout brch1 `Переключение между ветками  
`git merge brabc name ` слияние веток (прежде надо выйти из бранча в мастер, гит в таком случае сам делает коммит)  
Визуально посмотреть историюgit log --graph --pretty=oneline --abbrev-commit  
Работа с удаленным репозиторием  
копирование с гита git clone https://github.com/xlopik/Toto.git   
Отправка локальных изменений в гит репуgit push https://github.com/xlopik/Toto.git   
Скачать с репы обновления (merge and fetch) git pull  https://github.com/xlopik/Toto.git  
Просто скачать без мерджаgit fetch https://github.com/xlopik/Toto.git  
Смерждить изменения из git fetch с локальной веткой git merge FETCH_HEAD  
`origin` - удаленная ветка  

Как комитить в гит через пул реквест  
`Pull request` — предложение изменения кода в чужом репозитории  
1- создаем ветку локально  
2-меняем код  
3-добавляем,комитим  
4- делаем пуш в гит  git push origin br1  
5- делаем пул реквест  вгитхаб  
6-вуаля комит в мейне  
Есть еще гит форк - когда чужой код копируеш в свою репу,меняешь коди , и потом пушишь в чужой код.  
Работа над задачей  
Сделать git pull для получения последних изменений  
Сделать новую ветку и все изменения вести там  
Ветку назвать по номеру задачи  
После того как работа завершена переключится в main и повторно выполнить git pull для получения обновлений  
Переключиться в новую ветку и сделать rebase  
После rebase можно спокойно мерджить ветку в main и пушить в remote  
rebase  - команда копирует комиты(изменения файлы),в ветку (b1),тем самым пермещая метку main в конец.  
1-`git checaut b1`  
2-`git rebase main` - нереносим\kопируем комиты  
3- `git checaut master `  
4- `git merge b1` делаем Fast-forward  
Cоздать репу через кли https://cli.github.com/manual/gh_repo_create#:~:text=Create%20a%20new%20GitHub%20repository,--private%20%2C%20or%20--internal%20  
https://askdev.ru/q/chto-oznachaet-fetch-head-in-git-10965/  
`$ ssh -i id_rsa | git clone git@git.**e.git` - скачать репу, с готовым ключем.


<h2><b> Работа с Файлами  </b></h2>
`grep `-  поиск в файле, поиска строк(подстрок(посл-сть символов в строке)), соответствующих строке в тексте или содержимому файлов. regexp задает шаблон для поиска текста.Они регистрзависмы.  
Пример:    
1- точное совпадение встроено      
2- вкл шаблон поиска : точка ( .) пример Regex: grep \.txt рез- file.txt (\. - именно точку ищем), несколько точки ва..я-васия   
3- допустимые : А[нл][нл]а - Анна [А-Яа-яЁё]  — все русские буквы (только диапазон [1-31]от 1-3)  
4- ^ - каретка : [^0-9]  — любой символ, кроме цифр [^а-в8]  — любой символ, кроме букв «а», «б», «в» и цифры 8  
5 - экранирование Regex: fruits\[0\] Найдет: fruits[0] , пример 0[1-9]|[12][0-9]|3[01] 0, 2 символо в 1-9 или 1 ии 2 , 2 символ от 0-9 и т.д.  
6 - Одного символа — используем [] Нескольких символов или целого слова — используем | cat y.y | grep  'ва(с|си)я' вывод - вася ,васия  
Мета:  
grep  -P '.*\d\d\.\d\d\.\d\d\d\d.' или (\d{2}\.\d{2}.\d{4})- 2 смвола цыфр, 2 и 4 ,  \w+@\w+\.\w+ - test@mail.ru 1 + - или более символов после \w * - 0 или более ? - 0 или 1  
7- () - скобки это группа  квантификатор Regex: (data){2} , \1 повторение группы (Perl - $  Python group[1] PHP $matches[1])  
8- Regex: \bарка\b арка - точное совпадение  
9- ^ — начало текста (строки) $ — конец текста (строки) Regex: ^Я нашел!$ покажет: Я нашел!  
10- Замена RegEx: (Оля) \+ Маша Замена: $1 Текст был: Оля + Маша Текст стал: Оля ,Замена даты  RegEx: (\d{2})\.(\d{2})\.(\d{4}) Замена: $3-$2-$1  
https://habr.com/ru/post/545150/  
http://espressocode.top/zgrep-command-in-linux-with-examples/  
```
`$ grep . -rnw -e 'This is your first post'` Поиск текста внутри файла
root@хзхз:~# grep .ru ~/y.y
test@mail.rugfhgfhgfhfg
```
`grep - c` - вывод кол-ва повтор строк -v исключить из поиска фразу. -i - учет регистра.  
`grep -P -n '^.{120,200}$' tt ` - ищим стороки в диапазоне кол-ва символов и потом ищем  
`wc -L` - максимальное число символов в строке
  

`$ sudo tail -f cat  /var/log/nginx/access.log  | grep -Po '^\d{1,3}\.\d{1,3}.\d{1,3}.\d{1,3} '` - вывод последних ip в реальном времени  
`$ cat vprg/bas.txt | grep.exe  -Po "getTurnir\(\'match\',\'\d{1}\'\,\'\d{1,8}\',\'\d{1,2}\',\'\'\)" > vprg/basend.txt

` vp, сперва 
![image](https://user-images.githubusercontent.com/71122445/198233196-d5404435-cdbe-4df9-b442-348573a36d5c.png)

`awk` - для обработки и фильтрации текста  
```
root@dgm:~# cat virt-sysprep-firstboot.log | grep  -P '^(\w){7} ' | awk '{print $1 "|" $2}'
Scripts|dir:
Removed|/etc/systemd/system/sshd.service.
Removed|/etc/systemd/system/multi-user.target.wants/ssh.service.
Created|symlink
Created|symlink
root@dgm:~#
```
`head -n 10` первые 10 строк
`tail -f -s 5 /var/log/syslog` выводит  каждые 5 сек актуальные строки из хвоста лога  

`sort` вывести отсортированный текст `root@dgm:/var/log# ls --full-time | sort -rk6` сортировка по 6 стобцу  

`uniq` предназначена для поиска одинаковых строк в массивах текста.  

```
na@test:~$ diff -y lol1 lol3  
2                                                               2  
2                                                               2  
2                                                             | 3  
2                                                             | 3  
                                                              > 5  
nav@test:~$
``` 
построчное сравнение содержимого файлов . 2,3c2,4 - 2 и 3 стоки были изменены в 1 файле.



`stat Access`- права доступа к файлу, `Modify`- время когда в последний раз изменялся контент файла, `Change` - время, когда в последний раз изменялись атрибуты файла.  
`Inode` - cструктура данных в которой хранится информация о файле или директории в файловой системе (кол-во айнод ограничено файлсистой в рамках диска,их кол-во можно задать при создании фс)(это сколько файлов можно записать на диск)  
```
root@dgm:~# stat qqq.py 
  File: qqq.py
  Size: 2965            Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d      Inode: 11074       Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-12-02 14:11:04.021486523 +0300
Modify: 2021-11-29 21:16:49.418980688 +0300
Change: 2021-11-29 21:16:49.422980685 +0300
 Birth: -
root@dgm:~#
```
<h2><b> Работа с ПО </b></h2>
<h2><b> Траблшутинг  </b></h2>

<h2><b>Работа с Системой</b></h2>
`du` специально предназначена для просмотра размера файлов  место в папке. выполняет запрос непосредственно к каждому найденному файлу в разделе  

```
root@digmath:~# du -h ~/qqq.py
4.0K    /root/qqq.py
root@digmath:~#
```
`df`  к  файловой системе.df  -i - сколько inode дескрипторов использовано для файловой системы  
```
root@dgm:~# df -h 
Filesystem      Size  Used Avail Use% Mounted on
udev            986M     0  986M   0% /dev
tmpfs           199M  756K  199M   1% /run
/dev/sda1        15G  6.7G  7.9G  46% /
tmpfs           994M     0  994M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           994M     0  994M   0% /sys/fs/cgroup
tmpfs           199M  4.0K  199M   1% /run/user/0
root@dgm:~#
```
https://rtfm.co.ua/unix-df-i-du-raznye-znacheniya/  


`logrotate` - для автоматизации обработки журналов, кнфг - /etc/logrotate.conf  
https://losst.ru/nastrojka-logrotate  
https://max-ko.ru/33-logi-v-linux-1.html  
 

Права:
`$ sudo adduser ubuntuserver и $ sudo adduser ubuntuserver sudo првоерить входит ли в гурппу су sudo -l -U ubuntuserver` - создать юзера и предоставить права на судо 
`sudo deluser ubuntuserver sudo` - удалить  судо  
`$ sudo deluser ubuntuserver` - удалть узера  
`chmod` опции права /путь/к/файлу  - редактор права https://losst.ru/komanda-chmod-linux   
`chmod g+x lol7` - даем права на исполнение лол7 группе  

`find` -  для поиска файлов и каталогов удаления  
```
root@dgm:~# find -user root  -name "*.py" -and -size +1k
./qqq.py
./gjgh.py
root@dgm:~#
find . -name "*.jpg" -exec cp {} /backups/fotos \; -копировать все изображения
find / -name qqq.py
/root/qqq.py
/var/www/www-root/data/root/qqq.py
/var/www/www-root/data/var/qqq.py
root@dgm:~#

root@dgm:~/sbcloud# find . -name "lol*"
./lol45435344353
./lol4
./lol
./lol4543534
./lol454353443535555555555555555555

root@dgm:~/sbcloud# find . -name "lol*" -delete
root@dgm:~/sbcloud# ll
```
https://losst.ru/komanda-find-v-linux  




`top / htop` - просмотр процессов  / тоже но удобнее, с упр. мышкой (F2 потом serland process threads и др настройки.)  
 
`watch watch -d free -m` вывод команд на экран. (2 сек.опрос можно увеличить.)  
`last` - последнии входы в систему  
```
root@dgm:/var/log# last | head -n 4
root     pts/3        31.173.83.162    Sat Dec 25 22:27 - 22:27  (00:00)
root     pts/2        109.63.195.105   Sat Dec 25 22:26   still logged in
root     pts/1        109.63.195.105   Sat Dec 25 19:52   still logged in
root     pts/2        109.63.195.105   Sat Dec 25 14:33 - 14:40  (00:07)
root@dgm:/var/log#
```
 
```
journalctl -u python_demo_service.service --since today | tail -n 2  - глянуть логи сервисиа за сегодня 
journalctl /usr/bin/docker - логи процесса
 journalctl -p err -b | tail -  фильтрации по уровню ошибки
journalctl -n 20 недавние события 
лог journalctl пишутся сообщения при старте сервисов, а также различные системные сообщения.
```

`rsyslog` - сервис для управления логами конфиг /etc/rsyslog.conf , есть правила сортировки: источник и приоритет, затем действие.*.* - это вообще все .Тут есть кактегории и уровни логов.сообщения, со всеми уровнями.  
https://www.dmosk.ru/miniinstruktions.php?mini=rsyslog   
https://losst.ru/nastrojka-rsyslog-v-linux   

`rsync  -avz  --delete /root/sisi/ /root/lol`   удалит в приемнике файлы кот. не в источнке. передается то, что только из изменено Между хостами  
https://losst.ru/rsync-primery-sinhronizatsii
```
naagapov@pd11-zabbixproxy-06:~/test$ ll
total 8
drwxrwxr-x 2 naagapov naagapov 4096 Feb 22 12:51 ./
drwxr-xr-x 5 naagapov naagapov 4096 Feb 22 12:59 ../
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 1
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 2
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 3
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 4
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 5
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:51 6
-rw-rw-r-- 1 naagapov naagapov    0 Feb 22 12:49 7
naagapov@pd11-zabbixproxy-06:~/test$

Пишем рсинк:
naagapov@pd11-zabbixproxy-06:~$ rsync -avz ./test 10.0.244.12:~/test
sending incremental file list
test/
test/1
test/2
test/3
test/4
test/5
test/6
test/7
sent 427 bytes  received 153 bytes  1,160.00 bytes/sec
total size is 0  speedup is 0.00
naagapov@pd11-zabbixproxy-06:~$

Заходим на 10.0.244.12

И видим там все эти файлы
```

`scp` - удаленное копирование между тачками локальная-удаленyая и наоборот  
  `$  scp -P 1616 8800jj 8883434 8884 root@194.67.91.196:/root/66` - без ключа  
  `$   scp -i /c/Users/nikit/.ssh/777 -v -p 22  44 naagapov@10.10.121.10:/home/naagapov` - с ключом  

  работает по ssh. Перезапишет если одинаковое имя файла.  

`cp -r` копирование папок(без -r файлов) локально откуда куда  не забыть указать слеш в приемнике , а то перезапишеться (/* - без -r копировать все файлы из папки)-u - скопировать файл,если был изменён (сохраняется последнее редактированное)  
https://losst.ru/kopirovanie-fajlov-v-linux  
`vim / vimtutor` – редактор файлов.
`ps - process state` — «состояние процессов» мониторинга активных процессов по виртуальным файлам в файловой системе /proc
```
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
процессы, использующие максимум ОЗУ
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
-A, -e, (a) - выбрать все процессы;
-a - выбрать все процессы, кроме фоновых;
-d, (g) - выбрать все процессы, даже фоновые, кроме процессов сессий;
-N - выбрать все процессы кроме указанных;
-С - выбирать процессы по имени команды;
-G - выбрать процессы по ID группы;
-p, (p) - выбрать процессы PID;
--ppid - выбрать процессы по PID родительского процесса;
-s - выбрать процессы по ID сессии;
-t, (t) - выбрать процессы по tty;
-u, (U) - выбрать процессы пользователя.
```
`pstree` процессы в виде дерева  
`pgrep ssh -l` - идентификаторы процессов запущенной программы на основе заданных критериев.(-i номер проца с идентфикром.)  

`pidof firefox`- поиск всех идентификаторов процессов и (удаление sudo kill -9 2551 2514 1963 1856 1771)  

`pkill` - убить процесс по имени.  
`kill -9 25609` отправляет сигнал указанным процессам или группам процессов  
`kill -l` все доступные сигналы  
`9)` -уничтожение навсегда
`killall -s 9 gcalctool`  -удивает процессы команды глактул  
`strace` - трассировка системных вызовов (сисколы)  

`crontab -e` отредактировать файл `/etc/crontab` с расписание задач.  

`date` - для работы с датой и временем  

`touch ` или `cat > namefile` создание файла.  
`type ls` что верить что такое ls, узнать тип команд  
`which car`  путь к исп. файлу  
`file aaa.zip` тип данных узнать    
 
`vmstat` - инфо об использовании памяти, дисков, процессора.  
```
root@dgm:~# vmstat 
Результат разбит на шесть колонок – procs (процессы), memory (память), swap, io (диск I/O), system (система/ядро), CPU (процессор).
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0 567552 464204 131536 825392    0    0     1    55    2    1  2  1 96  0  0
```
`free` общем объеме физической памяти и памяти подкачки  
`time` сколько времени требуется для выполнения данной команды  
```
root@194-67-91-196:/bin# time -p  curl -I  https://digmath.ru/
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Tue, 09 Nov 2021 16:03:07 GMT
Content-Type: text/html
Connection: keep-alive
Vary: Accept-Encoding
Strict-Transport-Security: max-age=31536000;
real 0.08
user 0.02
sys 0.01
root@194-67-91-196:/bin#
```
реальное или общее или прошедшее — это время от начала до конца выполнения. Это время с момента нажатия клавиши Enter до момента завершения команды курл .  
`user` — количество процессорного времени, проведенного в пользовательском режиме.  
`system или sys` — количество процессорного времени, потраченного в режиме ядра.  

 Каталоги linx:  
`/` -корень,к нему подкл. остальное.  
хранения бинарных файлов  
`/BIN` - (BINARIES) БИНАРНЫЕ ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ (однопольз. режиме или режиме восст., юзают все узеры)  
`/SBIN` - (SYSTEM BINARIES) СИСТЕМНЫЕ ИСПОЛНЯЕМЫЕ ФАЙЛЫ, бинар для конфигурит сист.  
`/USR/BIN/` - ИСПОЛНЯЕМЫЕ ФАЙЛЫ (не нужные исп. айлы для заргузки)  
`/USR/SBIN/` - бинари бля сис.админства  

для хранения данных  
`/TMP (TEMP) `- ВРЕМЕННЫЕ ФАЙЛЫ  
`/MNT (MOUNT)` - МОНТИРОВАНИЕ   
`/MEDIA` - СЪЕМНЫЕ НОСИТЕЛИ  
`/SRV (SERVER)` - СЕРВЕР  

Директория системных ресурсов Unix /usr  
`/USR` - ( Unix System Resourceс) ПРОГРАММЫ ПОЛЬЗОВАТЕЛЯ  
`/USR/LOCAL` - ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ  

`/VAR (VARIABLE)` - ПЕРЕМЕННЫЕ ФАЙЛЫ  
`/VAR/LOG` - ФАЙЛЫ ЛОГОВ  
хранения файлов конфигурации  
`/BOOT` - ФАЙЛЫ ЗАГРУЗЧИКА  
`/ETC` - (ETCETERA) КОНФИГУРАЦИОННЫЕ ФАЙЛЫ (скрипты запуска и завершения системных демонов,)  

 в оперативной памяти  
`/DEV` - (DEVICES) ФАЙЛЫ УСТРОЙСТВ,жеские диски придатвлены тут   
`/PROC` - (PROCCESS) ИНФОРМАЦИЯ О ПРОЦЕССАХ  
/SYS` (SYSTEM) - ИНФОРМАЦИЯ О СИСТЕМЕ  


`/OPT` (OPTIONAL APPLICATIONS) - ДОПОЛНИТЕЛЬНЫЕ ПРОГРАММЫ  

/RUN - ПРОЦЕССЫ  

`/lib`   `/bin и /sbin` обычно используют разделяемые библиотеки  
`uptime` - время, время работы после загрузки, количество текущих пользователей в к    
```
load average: 0.13, 0.30, 0.27
1 5 15 min
1 0 0 -растет
0 0 1 - падает
0 0 0 - простой
```
Меньше 1 простой, =1 система загуржена на максимум,но очереди нет, > 1 сис. перегружена и есть очередь.
4.2 при 2 ядрах - надо искать что засоряет.    
`load avarage` - это «средние значения нагрузки системы», показывающие потребность в исполняемых потоках (задачах) в виде усреднённого количества исполняемых и ожидающих потоков. 
Проге надо проц.время, проц. выделяет "тик" (время на выполнение процесса) - в порядке очереди каждому процессу на выполнение  

<h2><b> Работа с Bash </b></h2>


