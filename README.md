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

### 6. grep '/fruit\\[5\\]/' code.txt

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




