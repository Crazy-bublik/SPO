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


# Ivm

### 1.
#### a. Создаем новый раздел на втором жестком диске:

   sudo fdisk /dev/sdb
   
   n (создать новый раздел)
   
   p (применить раздел типа primary)
   
   1 (выбрать первый раздел)
   
   +150M (размер раздела)
   
   n (создать второй раздел)
   
   p (применить раздел типа primary)
   
   2 (выбрать второй раздел)
   
   +150M (размер раздела)
   
   w (сохранить изменения)

#### b. Форматируем первый раздел в файловую систему Ext4 и примонтируем к папке /mnt/ext4:

   sudo mkfs.ext4 /dev/sdb1 (создаем файловую систему ext4 на разделе '/dev/sdb1')
   
   sudo mkdir /mnt/ext4
   
   sudo mount /dev/sdb1 /mnt/ext4 (монтируем раздел '/dev/sdb1' в созданную директорию '/mnt/ext4')

#### c. Добавляем точку монтирования /mnt/ext4 в /etc/fstab:

   echo "/dev/sdb1 /mnt/ext4 ext4 defaults 0 0" | sudo tee -a /etc/fstab
   
tee -a: читает стандартный ввод и записывает его в файл, добавляя к содержимому файла

### 2.

На втором разделе создаем локальный том (LVM) размером 100 Мб:

   sudo pvcreate /dev/sdb2 (создаем физический том (physical volume) на устройстве '/dev/sdb2')
   
   sudo vgcreate myvg /dev/sdb2 (создаем группу томов (volume group) с именем 'myvg', добавляя в нее физический том '/dev/sdb2')
   
   sudo lvcreate -L 100M -n mylv myvg (создаем логический том (logical volume) с размером 100 мегабайт и именем 'mylv' в группе томов 'myvg')
   
-L 100M : указывает размер логического тома в мегабайтах

-n mylv : определяет имя для нового логического тома

myvg : указывает группу томов, в которой будет создан логический том

Создем файловую систему XFS на созданном логическом томе:

   sudo mkfs.xfs /dev/myvg/mylv
   
Примонтируем файловую систему к папке /mnt/xfs:

   sudo mkdir /mnt/xfs
   
   sudo mount /dev/myvg/mylv /mnt/xfs
   
### 3.

Создаем файл /mnt/ext4/file1 и наполняем его произвольным содержим:

   sudo touch /mnt/ext4/file1
   
   sudo echo "Произвольное содержимое" | sudo tee /mnt/ext4/file1
   
Скопируем его в file2:

   sudo cp /mnt/ext4/file1 /mnt/ext4/file2
   
Создем символическую ссылку file3 на file1:

   sudo ln -s /mnt/ext4/file1 /mnt/ext4/file3
   
-s : указывает, что это символическая ссылка

Создем жёсткую ссылку file4 на file1:

   sudo ln /mnt/ext4/file1 /mnt/ext4/file4
   
Посмотрим, какие inode у файлов:

   ls -i /mnt/ext4/file1 /mnt/ext4/file2 /mnt/ext4/file3 /mnt/ext4/file4
   
ls -i : отображает индексы `inode` файлов, перечисленных в аргументах

Удалим file1:

   sudo rm /mnt/ext4/file1
   
После удаления file1, file2 и file4 останутся неизменными, так как они по-прежнему ссылаются на то же место в памяти. Однако символическая ссылка file3 будет битой, так как она больше не будет указывать на файл

Попробуем вывести остальные файлы на экран:

   cat /mnt/ext4/file2
   
   cat /mnt/ext4/file3
   
   cat /mnt/ext4/file4
   
### 4.

Создаем жёсткую ссылку вида /mnt/xfs/link, по которой будет доступен /mnt/ext4/file4:

   ln /mnt/ext4/file4 /mnt/xfs/link

   
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

sudo apt update

sudo apt install nginx

cd /srv/

sudo mkdir www

cd www

sudo nano main

sudo mkdir conf

cd conf

sudo nano main.cfg

sudo nano nginx.conf

Меняем файл:
```
events {}
http {
    server {
        listen 80;
        server_name 192.168.43.187;
         location /main {
            root /srv/www;
        }
         location /main.cfg {
            root /srv/www/conf;
        }
    }
}
```

sudo systemctl reload nginx.conf

wget http://192.168.43.187/main

wget http://192.168.43.187/main.cfg


# First_Bash

### 1.

sudo nano file1.sh
```
#!/bin/bash

number=$1
if [ $((number%2)) -eq 0 ]
then
  echo "Четное"
else
  echo "Нечетное"
fi
```
sudo chmod +x file1.sh

./file1.sh 3

### 2.
```

#!/bin/bash

check_divisibility() {
    n=$1
    x=$2
    y=$3
    if [ $((n%x)) -eq 0 ] && [ $((n%y)) -eq 0 ]
    then
        echo "true"
    else
        echo "false"
    fi
}

check_divisibility $1 $2 $3
```


# Bash (Task_1)
### 1.
```
#!/bin/bash

file="copy.txt"
info=$(ls -la "$file")
echo "$info"
```
### 2.
```
#!/bin/bash

echo "Number:"
read num
for ((i=1;i<=num;i++))
do
  for ((j=1;j<=i;j++))
  do
    echo -n "$j "
  done
  echo ""
done
```
### 3.
```
#!/bin/bash

echo "Number:"
read num
count=1
for ((i=1;i<=num;i++))
do
  for ((j=1;j<=i;j++))
  do
    echo -n "$count "
    ((count++))
  done
  echo ""
done
```
### 4.
```
#!/bin/bash

echo "Year:"
read year
if [ $(( year % 4 )) -eq 0 && $(( year % 100 )) -ne 0 ] || [ $(( year % 400)) -eq 0 ];
then
  echo "Year $year is high-grade"
else
  echo "Year $year is not high-grade"
fi
```
### 5.
```
#!/bin/bash

echo "Number:"
read num
for ((i=1;i<=num;i++))
do
  for ((j=1;j<=num;j++))
  do
    if (( (i+j)%2 == 0)); then
      echo -e -n "\e[40m" " "
    else
      echo -e -n "\e[47m" " "
    fi
  done
  echo -e "\e[0m" " "
done
```
-e : это опция команды `echo` для интерпретации escape-последовательностей

-n : это опция команды `echo` для подавления добавления символа новой строки в конце выводимой строки. Это позволяет выводить текст без добавления перевода строки на следующую строку

### 6.
```
#!/bin/bash

addLetters() {
  if [ "$#" -eq 0 ]; then
    echo 'z'
    return
  fi
  sum=0
  for letter in "$@"
  do
    total=$(printf "%d" "'$letter")
    sum=$((sum + total - 96))
  done
  sum=$(( (sum - 1) % 26 + 1))
  result=$(printf \\$(printf '%03o' $((sum + 96))))
  echo "$result"
}

addLetters "$@"
```
printf "%d" : преобразует аргумент в числовое значение, используя таблицу ASCII

printf '%03o' : преобразует новое числовое значение обратно в символ алфавита

### 7.
```
#!/bin/bash

is_valid_ipv4() {
    local ip="$1"
    local regex="^([1-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"

    if [[ $ip =~ $regex ]]; then
        echo "Действительный IPv4 адрес"
    else
        echo "Недействительный IPv4 адрес"
    fi
}

ip_address=$1
is_valid_ipv4 "$ip_address"
```


# Bash (Task_2)
### 1.
```
#!/bin/bash

echo "Введите число:"
read num
total=0
for ((i=1;i<=$num;i++))
do
  total=$((total + i))
done
echo "Сумма чисел от 1 до $num равна $total"
```
### 2.
```
#!/bin/bash

s1="xyaabbbccccdefww"
s2="xxxxyyyyabklmopq"
combined=$(echo -e "$s1\n$s2" | grep -o . | sort -u | tr -d '\n')
echo "Результат: $combined"
```
grep -o . : выбирает каждый символ отдельно (каждый символ с новой строки)

sort -u : сортирует символы и удаляет повторяющиеся

tr -d '\n' : удаляет символы новой строки

### 3.
```
#!/bin/bash
read s
s_upper=$(echo $s | tr '[:lower:]' '[:upper:]')
IFS=';' read -ra guests <<< "$s_upper"
names=()
last_names=()
for guest in "${guests[@]}"
do
  IFS=':' read -r name last_name <<< "$guest"
  names+=("$name")
  last_names+=("$last_name")
done
sorted_guests=()
for i in "${!last_names[@]}"
do
  sorted_guests+=("${last_names[$i]},${names[$i]}")
done
sorted_guests=($(printf "%s\n" "${sorted_guests[@]}" | sort))
```
tr '[:lower:]' '[:upper:]' : преобразование строки в верхний регистр

-r : это опция read для отключения обратного слэша (Backslash) в строке. Это используется для чтения данных так, как они есть, без интерпретации специальных символов

-a : это опция read для чтения слов в массив, где каждое слово разделено пробелом







