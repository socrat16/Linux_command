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
vim / vimtutor – банально файл открывать/закрывать надо уметь
ps uaxwf (что каждая опция значит)
pstree
sort, uniq
diff
stat (что такое Access, Modify, Change?)
pgrep / pkill
dig
lsof
strace
crontab
date
touch (что конкретно делает?)
mount
iostat / iotop /
vmstat / free
git: git blame, git diff, git status, git pull, git add, git commit, git push (полезно будет, мы юзаем)
ping, traceroute, mtr, ip r, ip a, iftop
netstat, ss


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


