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

Адрес DNS 
etc/resolv.conf Обычно адрес роутера и он ретранслирует.Если нет nameserver интернет будет доступен только по ip

`ip a` - отображение сетевых интерфейсов с данными.
```
$ ip r
default via 10.128.0.1 dev eth0 proto dhcp src 10.128.0.21 metric 100
10.128.0.0/24 dev eth0 proto kernel scope link src 10.128.0.21
10.128.0.1 dev eth0 proto dhcp scope link src 10.128.0.21 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
```
Пакет идет на шлюз 10.128.0.1 (его дал dhcp) если его нет в сети пакеты идут внутри сети.

`ss` - какие сетевые подключения Linux открыты, какие IP адреса используются или какие порты прослушиваются.  `ss -t` - только TCP  

![image](https://github.com/socrat16/Linux_command/assets/71122445/7f8cc3b4-d61b-4202-b2b7-48571a514af8)

tcp и udp (-unlp) какие по слушают и скаких ip

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

ФС- метод хранения файлы на диске (fat,ext,ntfs) и интрефейс меджу ПО и диском через Ядро 
Принцип хранения: 
ФС содержат inode
inode - хранит права доступа до файла и индекс файла (кол-во inode ограничено)
Блок - мин. ед-ца хр-ния данных на диске. (4кб в ext)

Монтирование:
Перечень дисков/блочных ус-тв тут: /dev/sd* (1,2 и тп - логич.диски)

С дисками как с файлами работаем благодаря VFS, монитрование - связка файла блоч ус-ва к каталогу. (мы работаем с VFS, а за ней связка ОС - драйвера реал. ФС )

`/etc/fstab` - подмантированные ФС про загрузке ОС

`lsblk` - инфо о блочных ус-вах

Посмотреь диск
```
$ sudo fdisk -l /dev/vda
Disk /dev/vda: 16 GiB, 17179869184 bytes, 33554432 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 8E70400A-DD3F-443D-95C1-844A5A72302B
```

Подключить диск:
```
1- Получить/вставить диск
2- $ sudo fdisk /dev/vd(имяньюдиска)
3- Command (m for help): g (созд гпт таб.разд.)
4- Command (m for help): n (новые партиции)
5 - Command (m for help): w ( записать и выйти)
6 - ~$ sudo mkfs.ext4 /dev/vdb1 создать файл. систему на нем
7 - $ mount /dev/vdb1 /opt/labadisk2/ подмонтировать его
```

Создать LVM (Обьединять нескл. дисков в 1 пул адресов.):
```
0) Создать в настройках VM 2 диска по 512 МБ
[-@linux-lab ~]$ sudo fdisk -l
[output ommited]
Disk /dev/sdb: 512 MiB, 536870912 bytes, 1048576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

Disk /dev/sdc: 512 MiB, 536870912 bytes, 1048576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

1) Создать по 1 партиции на каждом из дисков

[-@linux-lab ~]$ sudo fdisk /dev/sdb 

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x7259e965.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-1048575, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-1048575, default 1048575): 

Created a new partition 1 of type 'Linux' and of size 511 MiB.

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

[-@linux-lab ~]$ sudo fdisk /dev/sdc

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x88d6f91f.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-1048575, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-1048575, default 1048575): 

Created a new partition 1 of type 'Linux' and of size 511 MiB.

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

[-@linux-lab ~]$ fdisk -l
[output ommited]
Disk /dev/sdb: 512 MiB, 536870912 bytes, 1048576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x7259e965

Device     Boot Start     End Sectors  Size Id Type
/dev/sdb1        2048 1048575 1046528  511M 83 Linux LVM


Disk /dev/sdc: 512 MiB, 536870912 bytes, 1048576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x88d6f91f

Device     Boot Start     End Sectors  Size Id Type
/dev/sdc1        2048 1048575 1046528  511M 83 Linux LVM


2) Cоздать physical volumes
[-@linux-lab ~]$ sudo pvcreate /dev/sdb1 /dev/sdc1
  Physical volume "/dev/sdb1" successfully created.
  Physical volume "/dev/sdc1" successfully created.

[-@linux-lab ~]$ sudo pvdisplay 
  --- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               VG_demo
  PV Size               511.00 MiB / not usable 3.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              127
  Free PE               127
  Allocated PE          0
  PV UUID               vK3cmu-oDNv-2qJy-pK7z-97eh-em4z-BDVjhN
   
  --- Physical volume ---
  PV Name               /dev/sdc1
  VG Name               VG_demo
  PV Size               511.00 MiB / not usable 3.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              127
  Free PE               127
  Allocated PE          0
  PV UUID               2BqgbA-Nbeq-30uK-zscg-RB6S-7Tn2-H7rvvQ

3) Создать volume group VG_demo1 используя созданные pv
[-@linux-lab ~]$ sudo vgcreate VG_demo /dev/sdb1 /dev/sdc1
  Volume group "VG_demo" successfully created

[-@linux-lab ~]$ sudo vgdisplay 
  --- Volume group ---
  VG Name               VG_demo
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1016.00 MiB
  PE Size               4.00 MiB
  Total PE              254
  Alloc PE / Size       0 / 0   
  Free  PE / Size       254 / 1016.00 MiB
  VG UUID               l45eZt-O9SZ-77pn-36Az-7ktD-u1L6-1O7BkB


4) Создать logical volume используя все доступное место vg
[-@linux-lab ~]$ sudo lvcreate -l 100%FREE -n LV_demo_1 VG_demo
  Logical volume "LV_demo_1" created.

[-@linux-lab ~]$ sudo lvdisplay
sudo lvdisplay 
  --- Logical volume ---
  LV Path                /dev/VG_demo/LV_demo_1
  LV Name                LV_demo_1
  VG Name                VG_demo
  LV UUID                Oamnsc-Xfmo-VdNa-mgy9-oKtP-Y8Vw-PW5TYZ
  LV Write Access        read/write
  LV Creation host, time linux-lab, 2021-07-23 14:02:46 +0300
  LV Status              available
  # open                 0
  LV Size                1016.00 MiB
  Current LE             254
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:2
[-@linux-lab ~]$ sudo vgdisplay 
  --- Volume group ---
  VG Name               VG_demo
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1016.00 MiB
  PE Size               4.00 MiB
  Total PE              254
  Alloc PE / Size       254 / 1016.00 MiB
  Free  PE / Size       0 / 0   
  VG UUID               l45eZt-O9SZ-77pn-36Az-7ktD-u1L6-1O7BkB

5) Отформатировать lv в xfs
[-@linux-lab ~]$ sudo mkfs.xfs /dev/VG_demo/LV_demo_1 
meta-data=/dev/mapper/VG_demo-LV_demo_1 isize=512    agcount=4, agsize=65024 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1
data     =                       bsize=4096   blocks=260096, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=1566, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

6) Добавить информацию в /etc/fstab примонтировать lv
[-@linux-lab ~]$ cat /etc/fstab 
[output ommited]
/dev/VG_demo/LV_demo_1	/opt/lab-disk		xfs	defaults	0 0

[-@linux-lab ~]$ sudo mount -a

[-@linux-lab ~]$ df
Filesystem                    1K-blocks    Used Available Use% Mounted on
devtmpfs                         918616       0    918616   0% /dev
tmpfs                            935308       0    935308   0% /dev/shm
tmpfs                            935308    8828    926480   1% /run
tmpfs                            935308       0    935308   0% /sys/fs/cgroup
/dev/mapper/cl-root            30320164 2406096  27914068   8% /
/dev/sda1                        999320  188316    742192  21% /boot
tmpfs                            187060       0    187060   0% /run/user/1003
/dev/mapper/VG_demo-LV_demo_1   1034120   40256    993864   4% /opt/lab-disk

```
Расширение корневой LVM:

```
Увеличить корневую ФС без  перезагрузки сервера


0) Добавить диск в 512 MB

1) Создать партицию
Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xc97e9841.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-1048575, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-1048575, default 1048575): 

Created a new partition 1 of type 'Linux' and of size 511 MiB.

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.


2) Создать pvextend
[-@linux-lab ~]$ sudo pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.


3) Расширить root vg
-@linux-lab ~]$ lvdisplay 
  WARNING: Running as a non-root user. Functionality may be unavailable.
  /run/lock/lvm/P_global:aux: open failed: Permission denied
[-@linux-lab ~]$ sudo vgdisplay 
  --- Volume group ---
  VG Name               cl
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <31.00 GiB
  PE Size               4.00 MiB
  Total PE              7935
  Alloc PE / Size       7935 / <31.00 GiB
  Free  PE / Size       0 / 0   
  VG UUID               uBFcgL-QHcf-Eovy-mqSd-N3z0-G8Aq-p0Gxzj
   
[-@linux-lab ~]$ sudo vgextend cl /dev/sdb1
  Volume group "cl" successfully extended


4) Расширить root lv
[-@linux-lab ~]$ sudo lvextend -l +100%FREE /dev/cl/root
  Size of logical volume cl/root changed from <28.93 GiB (7406 extents) to <29.43 GiB (7533 extents).
  Logical volume cl/root successfully resized.


5) расширить корневую файловую систему
[-@linux-lab ~]$ sudo xfs_growfs /dev/cl/root 
meta-data=/dev/mapper/cl-root    isize=512    agcount=4, agsize=1895936 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1
data     =                       bsize=4096   blocks=7583744, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=3703, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 7583744 to 7713792

[-@linux-lab ~]$ df 
Filesystem          1K-blocks    Used Available Use% Mounted on
devtmpfs               918616       0    918616   0% /dev
tmpfs                  935308       0    935308   0% /dev/shm
tmpfs                  935308    8808    926500   1% /run
tmpfs                  935308       0    935308   0% /sys/fs/cgroup
/dev/mapper/cl-root  30840356 2614960  28225396   9% /
/dev/sda1              999320  188316    742192  21% /boot
tmpfs                  187060       0    187060   0% /run/user/1003
[-@linux-lab ~]$ 

```

```
ОС распазнает ФС с помщью суперблока в после сектора в 1 партиции

 pvs - посмотреь какие физтома в лвм есть
 vgs - какие волюм группы
 lvs - какие папки
 
из физ в Vфиз:

pvcrreate /dev/sdd1
созд волгруппу
vgcreate vg_test /dev/sdd1 
созд партицию в vg_test
lvcreate vp_test -n lv_01 -L 100M
Только потом создаем ФС  (На лдогич томе)
mkfs.ext4 /dev/vg_test/lv_01
И монтируем
mount /dev/vg_test/lv_01 /nmt/01


Расширить туже волгруппу (иниц и добавленеи):
vgextend vg_test /dev/sdd2 
Добавить в партицию lv_01 места:
lvextend /dev/mapper/vg_test/lv_01 -L +100M
потом говорим что увеличили
resize2fs /dev/mapper/vg_test/lv_01
Одной командой + 100%
lvextend /dev/mapper/vg_test/lv_01 -l +100%FREE -r
Перенос данных:
pvmove /dev/sdd1 /dev/sdd3
Вытащит диск из состава vg_test:
vgreduce vg_test /dev/sdd1
Ваще убрать:
pvremove /dev/sdd1
```
 
PV - метка на партицию

VG - обединие PV 

![image](https://github.com/socrat16/Linux_command/assets/71122445/964fe145-d9b4-42c6-9858-3e6ebc6c80ba)




`iostat -х` параметры ввода и вывода данных на диск, скорость записи и чтения данных (раздел)  
https://russianblogs.com/article/6318634594/  
https://losst.ru/opisanie-iostat-linux  

`iotop -p` пид процесс  отображает загрузку диска процессами 

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
1 клонируем user@SC-WS-00054:~/.ssh$ ssh -i code | git clone git@git.domain.tech:name/ansible.git
1- создаем ветку локально git checkout -b MON-3249 создаем новую ветку и переходим в нее 
2-меняем код  
3-добавляем,комитим  git commit -m "текст что поменяли"
3/4 - git pull опять чтобы актуализировать
4- делаем пуш в гит    git push --set-upstream origin task-3249 пушим, создав ветку мон в удаленном репозитории
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
Регулярные Выражения:
[A-Z]*  - любое слово из больших букв
[0-9]*   - сколько угодно подряд стоящих цифр    
[A-Za-z]*@[A-Za-z]*.com   – простое выражение емайлов с окончанием .com
www\.[a-z]*\.com  - любой вэб адресс  с окончанием .com

/dev/null   - устройство находящиеся в ж#$е
sort names.txt - сортировка текста внутри файла по имени
sort names.txt > sort_names.txt - сортировка имен внутри файла и создание 
нового файла с отсортированными именами
sort -n numbers.txt > sort_names.txt - сортирует номера в файле с номерами и 
перезапишет это в файл сорт_нэймс.тхт с удалением имеющейся в конечном файле 
информации.
sort names.txt >> sort_names.txt - добавить текст без уничтожения текста из 
конечного файла.
Если выполнить команду sort names.txt > names.txt , то система сначала создаст 
новый файл names.txt (или пересоздаст его с нулевым объемом), а затем уже 
начнет выполнять с ним какие-то функции. Поэтому данная команда просто 
уничтожит все данные в файле до сортировки.
grep denis /etc/* - выдает все ответы, в каких файлах встречается слово denis в 
подкаталогах каталога etc. 
grep denis /etc/* 2> errors.txt - плохие ответы permission denied и т.д. - 
неправильный поток с ошибками - уходит либо вникуда (если нет файла) либо в 
файл эррорс.тхт
grep denis /etc/* 2> /dev/null - перенаправил плохие ответы вникуда
grep denis /etc/* > good.txt 2> errors.txt - выданные хорошие ответы без ошибок 
будут записаны в файл гуд.тхт, а второсортные (плохие) ответы - в еррорс.тхт
grep denis /etc/* > good.txt - в гуд.тхт сохранятся только хорошие ответы
Если надо сохранить в одном файле и полоие и хорошие, используется команда
grep denis /etc/* &> results.txt - записывает в резалтс.тхт и плохие и хорошие 
ответы
`$ grep . -rnw -e 'This is your first post'` Поиск текста внутри файла
root@хзхз:~# grep .ru ~/y.y
test@mail.rugfhgfhgfhfg
```
`grep - c` - вывод кол-ва повтор строк -v исключить из поиска фразу. -i - учет регистра.  
`grep -P -n '^.{120,200}$' tt ` - ищим стороки в диапазоне кол-ва символов и потом ищем  
`wc -L` - максимальное число символов в строке

потоки STDIN 0, STDOUT 1 , STDERR 2

`ls -l > fielname` stdout или 1>

`ls -l ghgh 2> filename` stderr 

`>>` -добавить инфо в конец файла, > перезапись

`|` - stdout in stdin

`ls -l | tee -a filename` - добавить в файл и вывести на экран -a дозапись

`2>&1`  - указатель, STDERR туда же куда 1  



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

Пакеты - Исх коды - репозитории

Ubuntu/Debian/Kali/Mint Linux:

`apt-get install`     - скачать и установить программу

`apt-get remove`   - удалить программу

`dpkg –i`                - установить программу из файла .deb

`dpkg –r`                - удалить программу

RedHat/CenOS Linux:

`yum install`          - скачать и установить программу 

`yum remove`         - удалить программу 

`rpm –i `                 - установить программу из файла .rpm 

`rpm –e`                 - удалить программу 

`rpm -qa` - пак менеджер centos уст-ные пакеты в ОС

`rpm -ql httpd` - список файлов из уст-ного пакета

`yum searth httpd` - для работы с репозит (дает инфо в rmp) ищет в реп апач


<h2><b> Траблшутинг  </b></h2>

Зомби - процесс ничего не занмиает в ресурсах, сост между ехит и вейт.(если много - серв торозит или процесс не следит за своими потомками(не счит код возврата)).

<h2><b>Работа с Системой</b></h2>

Загрузка:
```
1 -Биос- проверка работы уств
2- Биос- смотрит MBR (512б) \ GPT 
(MBR вкл: код загрущика и таблицы разделов(sda sdb))
3- -Биос- считывает оттуда GRUB (позволяет ставить неск ОС на 1 диск)
4 - Загрузка ядра
5 -Загрузка системы инициалии 
(опр. порядок загр.служб) /lib/systemd/system
```
Ядро - часть отвечает за взаимодействие между желехом и user и предс прог инфержейс (posix) для сторонних ПО, работающих в прост-ве user
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
 
 `top` - вывод 1ых процессов кто больше проц времени ест
```
1 - кол-во ядер
us - скл-во проц ресов в про-ве user 
sy - в пр-ве ядра (про-мы с драйвером сетевая нагрузка)
ni - показывает пониж приоритет PR (чем ниже PR тем важнее процесс)
id - простой проц времени (если 100 и la высокий - это не изза проца)
wa - wait процессу выд проц время но он ждем  инфу с вв\вывода уст-тв (высоке - либо сеть било диск) если 5-10 уже видно хрень (чем занят iostat )
hi - скол-ко прерываний
si - часто ПО к ядру обращается
st- для vm если это вм из гипера иего проц нагружен (не на всех гиперах)
```

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
Процессы,порядок:

```
0-fork() - сисвызов, созд.новый процесс 
1-exec() - сисвыз заменет код в процессе
2-exit() - осв памяти закр даскрипторы,дает код возврата (0 корректно)
3-wait() - читает код и как считал процесс уходит из системы
```
`cp -r` копирование папок(без -r файлов) локально откуда куда  не забыть указать слеш в приемнике , а то перезапишеться (/* - без -r копировать все файлы из папки)-u - скопировать файл,если был изменён (сохраняется последнее редактированное)  
https://losst.ru/kopirovanie-fajlov-v-linux  
`vim / vimtutor` – редактор файлов.
`ps -efl` — «состояние процессов» мониторинга активных процессов по виртуальным файлам в файловой системе /proc  e - все f полное l long инфо о процессах
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

`man` – помощь

`info` – тоже помощь

`uptime`  - время с последнего включения

`lscpu` – данные процессора

`whatis` – показывает что делает комманда

`whereis` – показывает где файл

`locate` – показывает где файл

`ls`   - показать что в этой директории

`ls –la –R  /`    - показать все на компутере

`Ctrl+Z`  - отправить процесс на background

`Ctrl+C` – прекратить процесс вообще


`cd`  - сменить директорию

`ls` – вывести содержимое директории

`pwd` – вывести путь где мы сейчас

`~`    - сокращение нашей Home директории

`/`    - коренная директория Linux

`..`    - директория которая выше 

`.`     - директория где мы сейчас

`touch` – создать файл или обновить время


mkdir – создать директорию
rmdir – стереть пустую директорию
mv  - переименовать или перенести директорию

rm –R   – стереть не пустую директорию со всем что внутри
sudo rm –R  /   - замочить систему Linux
less -читть файл (y верх е вниз)


tar cf  mytar.tar  Folder1   - заархивировать Folder1
tar xf mytar.tar  - разархивировать архив
gzip     / bzip2     / xz      – скомпрессировать файл
gunzip /  bunzip2 / unxz  – раскомпресировать файл

tar cvzf myBZIP2.bz2  Folder1    – сжать Folder1
tar xvf  myBZIP2.bz2                  - распаковать архив
tar tf myBZIP2.bz2    - посмотреть что внутри архива

zip –r myZIP.zip Folder1   - Запаковать Folder1 в ZIP
unzip myZIP.zip                - Распаковать файл myZIP.zip






ps aux | grep bash  - найти все процессы bash от всех пользователей

pico  - новый редактов
nano – самый новый редактор

gedit – как и Notepad в Windows, работает только  если есть графический интерфейс

sudo  - запустить комманду используя Super User права
su   - сменить текушего пользователя

/etc/passwd    - тут хранятся все аккаунты
/etc/shadow   - тут хранятся все пароли аккаунтов
/etc/group    - тут хранятся все группы

whoami  - показать имя текущего пользователя
id   - показать к каким группам принадлежит пользователь
who – показать кто сейчас в системе
w   - показать кто сейчас в системе и что делает
last – показать последние логины

useradd  -m vasya   - создать юзера vasya с домашней  директорией
userdel –r vasya     - стереть юзера vasya с его домашней  директорией
/etc/skel    -  это шаблон домашней директории
passwd vasya   - изменить пароль для юзера vasya

groupadd Programmers  - создать группу Programmers
groupdel Programmers  - стереть группу Programmers

usermod –aG Programmers vasya  - добавить юзера vasya в группу Programmers
deluser vasya Programmers  - удалить юзера vasya  из групы Programmers


`chown` – изменить владельца файла / директории
`chgrp` – изменить группу файла / директории
`сhmod` – изменить права доступа на файл / директорию

`chmod  ugo+x  myfile.txt`   довавить X всем
`сhmod  g-rw   myfile.txt`   убрать RW у группы
`chmod  o=rw   myfile.txt`   установить RW всем остальным
 u = user
 g = group
 o = other
 a = ugo


`chmod  777   myfile.txt`   установить RWX всем
`chmod  741   myfile.txt ` установить:   RWX   владельцу, R - -    группе,  - - X   всем остальным
```
r = 4
w = 2
x = 1
```


`wget`    - скачать файл из интернета

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
`/MNT (MOUNT)` - МОНТИРОВАНИЕ блоч уст-тв внешние диски   
`/MEDIA` - СЪЕМНЫЕ НОСИТЕЛИ  
`/SRV (SERVER)` - Даныне для сервисов

Директория системных ресурсов Unix /usr  
`/USR` - ( Unix System Resourceс) ПРОГРАММЫ ПОЛЬЗОВАТЕЛЯ  
`/USR/LOCAL` - ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ  

`/VAR (VARIABLE)` - ПЕРЕМЕННЫЕ ФАЙЛЫ  
`/VAR/LOG` - ФАЙЛЫ ЛОГОВ  изменяемые данные
хранения файлов конфигурации  
`/BOOT` - ФАЙЛЫ ЗАГРУЗЧИКА  
`/ETC` - (ETCETERA) КОНФИГУРАЦИОННЫЕ ФАЙЛЫ (скрипты запуска и завершения системных демонов,)  

 в оперативной памяти  
`/DEV` - (DEVICES) ФАЙЛЫ УСТРОЙСТВ,жеские диски придатвлены тут   
`/PROC` - (PROCCESS) ИНФОРМАЦИЯ О ПРОЦЕССАХ  ОС в виде файлов (отсюда берут проги инфо)
/SYS` (SYSTEM) - ИНФОРМАЦИЯ О СИСТЕМЕ  и драйверах


`/OPT` (OPTIONAL APPLICATIONS) - ДОПОЛНИТЕЛЬНЫЕ ПРОГРАММЫ  

/RUN - ПРОЦЕССЫ  что запущено и с чем работает

`/lib`   `/bin и /sbin` обычно используют разделяемые библиотеки  


![image](https://github.com/socrat16/Linux_command/assets/71122445/f8dfce4a-8ceb-4599-9db6-310b7b6637b8)
Каталоги

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

```
./myscript.sh  Vasya  Petya 
$0 при этом равен  ./myscript.sh
$1 при этом равен Vasya
$2 при этом равен Petya
```

myOS=`uname –u`   - запускает uname –u и сохраняет  результат в переменную myOS

Сохранить ввод пользователя в переменную name: read –p “Please enter your name: “ name
```
####### Скрипт подсчета файлов

#!/bin/bash

find /etc -type f | wc -l


###### Запуск
# 1 - без прав
bash count_files.sh

# 2 - с правом на исполнение
chmod +x count_files.sh

# Относительный путь
./count_files.sh

# Абсолютный путь
/home/db/count_files.sh

# 3 - из директории для исполняемых файлов
echo $PATH
/usr/local/bin
cp ./count_files.sh /usr/local/bin/

############### Параметры

# count_files.sh
find $1 -type f | wc -l

bash count_files.sh /usr
bash count_files.sh /etc
bash count_files.sh


################ Переменные
training=Python
TRAINING=Linux

echo $TRAINING
echo $training
echo ${TRAINING}
echo ${training}

# count_files.sh
directory=$1
find $directory -type f | wc -l

############ Возврат значений из команд

# count_files.sh
directory=$1
num_of_files=$(find $directory -type f | wc -l)
echo $num_of_files

############ Ограничение на имена

1var=2

var =2

var = 2

VaR=2

Var1=2

var_=2

var_test=2

перем1=2

############ Специальные переменные

echo $HOSTNAME - имя хоста
echo $HOME - домашняя директория
echo $PWD - текущая директория
echo $UID - идентификатор пользователя
echo $$ - идентификатор процесса
echo $PS1 - вид командной строки
echo $? - код возврата

############ Подстановки

echo {0..10}

mkdir test{a..e}

echo {{a..e},z}

mkdir test{001..100}

echo ~

echo ~+

echo ~-

############ Циклы

#!/bin/bash

c=10

while [ $c -ge 0 ] 
do 
	echo "Test"
	let "c = c - 1"
done

for h in {01..24}
do
	echo $h
done

#!/bin/bash
for (( c=1; c<=5; c++ ))
do  
   echo "Попытка номер $c"
done

##### SELinux

sestatus

getenforce

# Отключить
sudo setenforce 0

# Конфиг: /etc/selinux/config поставить SELINUX в disabled

# Включить
sudo setenforce 1
```








