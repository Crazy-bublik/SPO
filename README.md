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

grep -w World sample2.txt

grep -w Hi sample2.txt

### 6. grep '\fruit\\[5\]' code.txt

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







