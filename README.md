
grep - утилита поиск в файле, поиска строк, соответствующих строке в тексте или содержимому файлов. regexp задает шаблон для поиска текста.Они регистрзависмы.
Пример:
1- точное совпадение встроено
2- вкл шаблон поиска : точка ( .) пример Regex: grep \.txt рез- file.txt (\. - именно точку ищем), несколько точки ва..я-васия 
3- допустимые : А[нл][нл]а - Анна [А-Яа-яЁё]  — все русские буквы (только диапазон [1-31]от 1-3)
4- ^ - такая хуембула : [^0-9]  — любой символ, кроме цифр [^а-в8]  — любой символ, кроме букв «а», «б», «в» и цифры 8
5 - экранирование Regex: fruits\[0\] Найдет: fruits[0] , пример 0[1-9]|[12][0-9]|3[01] 0, 2 символо в 1-9 или 1 ии 2 , 2 символ от 0-9 и т.д.
6 - Одного символа — используем [] Нескольких символов или целого слова — используем | cat y.y | grep  'ва(с|си)я' вывод - вася ,васия
Мета:
grep  -P '.*\d\d\.\d\d\.\d\d\d\d.' или (\d{2}\.\d{2}.\d{4})- 2 смвола цыфр, 2 и 4 ,  \w+@\w+\.\w+ - test@mail.ru 1 + - или более символов после \w * - 0 или более ? - 0 или 1
7- () - скобки это группа  квантификатор Regex: (data){2} , \1 повторение группы (Perl - $  Python group[1] PHP $matches[1])
8- Regex: \bарка\b арка - точное совпадение
9- ^ — начало текста (строки) $ — конец текста (строки) Regex: ^Я нашел!$ Я нашел!
10- Замена RegEx: (Оля) \+ Маша Замена: $1 Текст был: Оля + Маша Текст стал: Оля ,Замена даты  RegEx: (\d{2})\.(\d{2})\.(\d{4}) Замена: $3-$2-$1
https://habr.com/ru/post/545150/
root@хзхз:~# grep .ru ~/y.y
test@mail.rugfhgfhgfhfg

du специально предназначена для просмотра размера файлов  место в папке. выполняет запрос непосредственно к каждому найденному файлу в разделе
root@digmath:~# du -h ~/qqq.py
4.0K    /root/qqq.py
root@digmath:~#

df  к  файловой системе.
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
https://rtfm.co.ua/unix-df-i-du-raznye-znacheniya/


find -  для поиска файлов и каталогов
root@dgm:~# !1750
find / -name qqq.py
/root/qqq.py
/var/www/www-root/data/root/qqq.py
/var/www/www-root/data/var/qqq.py
root@dgm:~#
https://losst.ru/komanda-find-v-linux

head -n 10 первые 10 строк
tail -f -s 5 /var/log/syslog выводит  каждые 5 сек актуальные строки из хвоста лога


top / htop - просмотр процессов  / тоже но удобнее, с упр. мышкой (F2 потом serland process threads и др настройки.)
 
watch watch -d free -m вывод команд на экран. (2 сек.опрос можно увеличить.)


rsync (хотя бы привести две директории к одному виду)
scp

vim / vimtutor – редактор файлов.

ps 
process state — «состояние процессов» мониторинга активных процессов по виртуальным файлам в файловой системе /proc
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

pstree процессы в виде дерева

sort вывести отсортированный текст
uniq предназначена для поиска одинаковых строк в массивах текста.

diff построчное сравнение содержимого файлов

stat Access- права доступа к файлу, Modify- время когда в последний раз изменялся контент файла, Change - время, когда в последний раз изменялись атрибуты файла.
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


pgrep / pkill отображенормеров процесса / убить процесс

dig google.com  опрос DNS-серверов

lsof  утилита, служащая для вывода информации о том, какие файлы используются теми или иными процессами (если ошибка “Файлы используются“)
грепаем
lsof -i TCP:22
 lsof -i TCP:1-1024
 запущенные процессы на определенном порту
# lsof -i -u^root
Исключение пользователей
# lsof -i
всех сетевых подключений


strace - трассировка системных вызовов (сисколы)

crontab -e отредактировать файл /etc/crontab с расписание задач.

date - для работы с датой и временем

touch содание файла.

mount - ?

iostat / iotop / - ?

vmstat - инфо об использовании памяти, дисков, процессора.
root@dgm:~# vmstat 
Результат разбит на шесть колонок – procs (процессы), memory (память), swap, io (диск I/O), system (система/ядро), CPU (процессор).
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0 567552 464204 131536 825392    0    0     1    55    2    1  2  1 96  0  0


free общем объеме физической памяти и памяти подкачки

git: git blame, git diff, git status, git pull, git add, git commit, git push (полезно будет, мы юзаем)

ping - отправка ICMP пакетов на хост

traceroute
утилита для проверки проходжения пакетов через роутеря к конечному хосту
оманда traceroute linux использует UDP пакеты. Она отправляет пакет с TTL=1 и смотрит адрес ответившего узла, дальше TTL=2, TTL=3 и так пока не достигнет цели. Каждый раз отправляется по три пакета и для каждого из них измеряется время прохождения.
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



mtr, ip r, ip a, iftop

ip a - отображение сетевых интерфейсов с данными.

ss - какие сетевые подключения Linux открыты, какие IP адреса используются или какие порты прослушиваются.  ss -t - только TCP
netstat -tulpn
открытые порты
Отображение сервис айпи -порт -
порт принимает вх пакеты из инета

telnet уд. упр. компом , старый в основном ссх сейчас, для проверки портов(на упрвлемой тачке долн быь telnet-server)
root@dgm:~# telnet opennet.ru 80
Trying 217.65.3.21...
Connected to opennet.ru.
Escape character is '^]'.
^] (нажать ctrl+[)
telnet>
GET /
<HTML>

 
 
netcat netcat -u подключения оболочки к порту, установления соединений TCP/UDP
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
https://routerus.com/netcat-nc-command-with-examples/


curl, wget -  работа с URL страницами. для конектов к серву по разным протоколам с синтткс  урл. wget -b -o ~/wget.log http://ftp.gnu.org/gnu/wget/wget-1.5.3.tar.gz  -


time сколько времени требуется для выполнения данной команды
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
реальное или общее или прошедшее — это время от начала до конца выполнения. Это время с момента нажатия клавиши Enter до момента завершения команды курл .
user — количество процессорного времени, проведенного в пользовательском режиме.
system или sys — количество процессорного времени, потраченного в режиме ядра.

 
 
 
 Каталоги linx
/ -корень,к нему подкл. остальное.
хранения бинарных файлов
/BIN - (BINARIES) БИНАРНЫЕ ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ (однопольз. режиме или режиме восст., юзают все узеры)
/SBIN - (SYSTEM BINARIES) СИСТЕМНЫЕ ИСПОЛНЯЕМЫЕ ФАЙЛЫ, бинар для конфигурит сист.
/USR/BIN/ - ИСПОЛНЯЕМЫЕ ФАЙЛЫ (не нужные исп. айлы для заргузки)
/USR/SBIN/ - бинари бля сис.админства

для хранения данных
/TMP (TEMP) - ВРЕМЕННЫЕ ФАЙЛЫ
/MNT (MOUNT) - МОНТИРОВАНИЕ
/MEDIA - СЪЕМНЫЕ НОСИТЕЛИ
/SRV (SERVER) - СЕРВЕР

Директория системных ресурсов Unix /usr
/USR - ( Unix System Resourceс) ПРОГРАММЫ ПОЛЬЗОВАТЕЛЯ
/USR/LOCAL - ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ


/VAR (VARIABLE) - ПЕРЕМЕННЫЕ ФАЙЛЫ
/VAR/LOG - ФАЙЛЫ ЛОГОВ


хранения файлов конфигурации
/BOOT - ФАЙЛЫ ЗАГРУЗЧИКА
/ETC - (ETCETERA) КОНФИГУРАЦИОННЫЕ ФАЙЛЫ (скрипты запуска и завершения системных демонов,)

 в оперативной памяти
/DEV - (DEVICES) ФАЙЛЫ УСТРОЙСТВ,жеские диски придатвлены тут 
/PROC - (PROCCESS) ИНФОРМАЦИЯ О ПРОЦЕССАХ
/SYS (SYSTEM) - ИНФОРМАЦИЯ О СИСТЕМЕ





/OPT (OPTIONAL APPLICATIONS) - ДОПОЛНИТЕЛЬНЫЕ ПРОГРАММЫ

/RUN - ПРОЦЕССЫ

/lib   /bin и /sbin обычно используют разделяемые библиотеки

