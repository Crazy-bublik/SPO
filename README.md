# SPO

# grep_task

### 1. cat /etc/passwd | grep -v /bin/bash$

-v : исключает из поиска строки, содержищие шаблон

$  : символ, определяющий позицию шаблона в конце строки текста

### 2. netstat -tapln | grep "LISTEN"

### 3. grep -ir an sample.txt

-ir : ишет заданый шаблон без учета регистра

### 4. grep -w do sample2.txt

-w : ищет заданный шаблон с учетом регистра

### 5. grep -ir he sample2.txt

grep -w World sample2.txt | grep -w Hi sample2.txt

### 6. grep '\fruit\\[5\\]' code.txt

grep  '\fruit\\[5\\]' code.txt : отобразит строку содержащюю fruit[<номер по списку>\]

### 7. grep t -m 2 sample.txt

-m 2 : отображает первые две строки с искомым шаблоном


# sed_task

### 1. echo 'They ate 5 apples and 5 mangoes' | sed 's/5/fife/1'

echo  : отображает строки текста или строки, передаваемые в качестве аргиментов

.../1 : позволяет сделать замену только для первого появления

### 2. echo 'They ate 5 apples and 5 mangoes' | sed 's/5/fife/g'

.../g : заменяет все вхождения

### 3. cat hex.txt | sed 's/0xA0/0x50/g' | sed 's/0xFF/0x7F/g'

### 4.  echo 'goal new user sit eat dinner' | sed 'y/oaeui/OAEUI/'

'y/oaeui/OAEUI/' : меняет каждый шаблон символов на соответствующее символы

### 5. echo 'a sunny day' | sed 's/sunny/cloudy/g'


# awk_task часть 2 (второй лист)

### 1. cat lab3.data | awk -F ':' '{print $2}'

-F ':' : указывает что : является разделителем столбцов

'{print $2}' :  печатает 2 столбец 

### 2. cat lab3.data | awk -F ':' '/Dan/{print $2}'

'/Dan/{print $2}' : печатает 2 столбец (телефон)  только в строке где есть Dan

### 3. cat lab3.data | awk -F ':' '/Susan/{print $1, $2}'

awk -F '/Susan/{print $1, $2}' : печатает 1 и 2 столбецы (имя и телефон) только в строке где есть Susan. Разделитель строк - :

### 4. cat lab3.data | awk '$2~/^D/{print $2}'

$2~^D : первой буквой 2 строки должна быть D

awk '/^D/{print $2}' : печатает 2 столбец (фамилия) только в строке где фамилия начинается с буквы D. 

### 5. cat lab3.data | awk '$1~/^C/ || $1~/^E/ {print $1}' 

$1~/^C/ || $1~/^E/ : в начале буква C или буква E

### 6. cat lab3.data | awk -F ':' '$2~/^\(916)/ {print $1}'

$2~/^\(916)/ : столбец 2 начинается с (916)


# Сеть

### 1.

Создаем файл с расширением .yaml

sudo nano 01-config.yaml

Внутри файла прописываем:

'''

  network:
  
    ethernets:
    
      enp0s0:
      
         addresses:
         
          - 192.168.43.187/24
          
         gateway4: 192.168.43.1

         nameservers:
         
           addresses:
           
             - 0.0.0.0
             
    version: 2
    
'''

Копируем файл в директорию /etc/netplan:

cp ~/01-config.yaml /etc/netplan

Запускаем применение новой конфигурации:

sudo netplane apply

### 2.

Запускаем ping и проверяем доступность: 

ping ya.ru

### 3.

sudo tcpdump -i enp0s0 icmp -s0 -n | grep echo requesr

tcpdump : команда используется для захвата и анализа сетевого трафика

-i : параметр указывает на сетевой интерфейс, через который следует захватывать пакеты

-s0 : указывает, что необходимо захватывать и фнфлизировать все содержимое пакетов(0 сохраняет все данные пакетов для последующего анализа)

-n : адреса выводятся в числовом формате

### 4.

Проверяем доступность основных портов:

nc -vz ya.ru 80 443

nc -vz habr.com 80 443

nc -vz google.com 80 443

nc : команда для чтения и записи данных через сетевые соединения

-v : указывает на вывод более подробной информации о ходе выполнения команды

-z : указывает на проверку только открытого порта, без передачи данных

### 5.

sudo tcpdump -w sqlc.pcap

-w : указывает записать весь захватываемый трафик в файл sqlc.pcap


# Права и группы 

### 1.

sudo useradd -m -G group1 -s /bin/bash user1

sudo userdel -r user1

### 2.

sudo groupadd group1

sudo useradd -m -G group1 -s /bin/bash user1

sudo useradd -m -G group1 -s /bin/bash user2

sudo groupadd group2

sudo useradd -m -G group2 -s /bin/bash user3

sudo usermod -G group1 user3

sudo deluser user3 group1

### 3.

sudo visudo

user1 ALL=(ALL) NOPASSWD: ALL

### 4.

touch exemple.txt

chmod u=rw,g=rw,o=r exemple.txt

touch exemple

chmod 600 exemple

### 5.

sudo groupadd developer

sudo useradd -m -G developer -s /bin/bash user1

sudo useradd -m -G developer -s /bin/bash user2

sudo mkdir dir_test

sudo chgrp developer dir_test/

sudo chmod g+rwx dir_test/

sudo chmod g+s dir_test/


# WEB/TFTP

### 1.

sudo apt update

sudo apt install tftpd-hpa

sudo nano /etc/default/tftpd-hpa

внутри файла добавляем --create опцию, так как без неё на сервер невозможно будет загружать новые файлы: TFTP_OPTIONS="--secure --create"

sudo chown tftp:tftp /srv/tftp/ : Меняем владельца и группу папки

sudo systemctl restart tftpd-hpa.service

tftp 192.168.43.187 : запускаем клиентскую программу tftp сервера

tftp> get tftp_data : Скачивает файл с сервера tftp

tftp> put local_data : Загружает файл на сервер tftp

tftp> quit

### 2.



