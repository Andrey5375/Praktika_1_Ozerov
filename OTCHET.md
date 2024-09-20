# Задача 1
Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).
## Код
```bash
grep '.*' /etc/passwd | cut -d: -f1 | sort
```

```bash
localhost:~# grep '.*' /etc/passwd | cut -d: -f1 |sort
adm
at
bin
cron
cyrus
daemon
dhcp
ftp
games
guest
halt
lp
mail
man
news


```

# Задача 2
Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:
## Код
```bash
awk '{print $2, $1}' /etc/protocols | sort -nr | head -n 5
```
```bash
awk '{print $2, $1}' /etc/protocols | sort -nr | head -n 5
262 mptcp
143 ethernet
142 rohc
141 wesp
140 shim6
```

# Задача 3
Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):
## Код
```bash
#!/bin/bash

text=$*
length=${#text}

for i in $(seq 1 $((length + 2))); do
    line+="-"
done

echo "+${line}+"
echo "| ${text} |"
echo "+${line}+"
```
```bash
User@DESKTOP-OBLBJ4I MINGW64 ~/desktop
$ ./banner.sh "Druid"
+-------+
| Druid |
+-------+


```

# Задача 4
Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).
## Код
```bash
#!/bin/bash

file="$1"

id=$(grep -o -E '\b[a-zA-Z]*\b' "$file" | sort -u)

```
```bash
User@DESKTOP-OBLBJ4I MINGW64 ~/desktop
$ grep -oE '\b[a-zA-Z_][a-zA-Z0-9_]*\b' Nomer4.sh | grep -vE '\b(int|void|return|if|else|for|while|include|stdio)\b' | sort | uniq
E
Z
a
b
bash
bin
file
grep
id
o
sort
u
zA

```

# Задача 5
Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).
## Код
```bash
#!/bin/bash

file=$1

chmod 755 "./$file"

sudo cp "$file" /usr/local/bin/
```

Например, пусть программа называется reg:
```bash
User@DESKTOP-OBLBJ4I MINGW64 ~/Desktop
$ ./reg.sh banner.sh
User@DESKTOP-OBLBJ4I MINGW64 ~/Desktop
$ ls /usr/local/bin
banner.sh  ngrok
```

# Задача 6
Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
## Код
```bash

#!/bin/bash

for file in $(find . -type f \( -name "*.c" -o -name "*.js" -o -name "*.py" \)); do
    first_line=$(head -n 1 "$file")

    case "$file" in
        *.c|*.js)
            if [[ "$first_line" =~ ^(//|/\*) ]]; then
                echo "The file $file contains a comment in the first line."
            else
                echo "The file $file does not contain a comment on the first line."
            fi
            ;;
        *.py)
            if [[ "$first_line" =~ ^# ]]; then
                echo "The file $file contains a comment in the first line."
            else
                echo "The file $file does not contain a comment on the first line."
            fi
            ;;
    esac
done

```

```bash
User@DESKTOP-OBLBJ4I MINGW64 ~/Desktop/proverka
$ ./6.sh
The file ./proverka.c does not contain a comment on the first line.
The file ./proverka.js does not contain a comment on the first line.
The file ./proverka.py does not contain a comment on the first line.

```

# Задача 7
Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
##Код
```bash
#!/bin/bash

# Проверяем, что передан аргумент (директория)
if [ -z "$1" ]; then
    # Если аргумент не передан, выводим сообщение об использовании и завершаем выполнение
    echo "Usage: $0 <directory>"
    exit 1
fi

# Создаем временный файл для хранения контрольных сумм
temp_file=$(mktemp)

# Находим все файлы в указанной директории и считаем для них хэш SHA256
# Результаты сохраняем во временный файл
find "$1" -type f -exec sha256sum {} + > "$temp_file"

# Сортируем и находим дубликаты по контрольным суммам
# Используем awk для обработки сортированных данных
sort "$temp_file" | awk '
    {
        # Если хэш уже встречался
        if ($1 in hashes) {
            # Если дубликат для этого хэша еще не был выведен
            if (!seen[$1]) {
                # Выводим первое вхождение этого хэша
                print hashes[$1]
                # Помечаем, что дубликат для этого хэша уже был выведен
                seen[$1] = 1
            }
            # Выводим текущую строку (дубликат)
            print $0
        } else {
            # Если хэш встречается впервые, сохраняем его
            hashes[$1] = $0
        }
    }
' > duplicates.txt

# Проверяем, есть ли дубликаты
if [ -s duplicates.txt ]; then
    # Если файл duplicates.txt не пустой, выводим дубликаты
    echo "Duplicates:"
    cat duplicates.txt
else
    # Если файл duplicates.txt пустой, выводим сообщение о том, что дубликатов нет
    echo "No duplicates."
fi

# Удаляем временный файл
rm "$temp_file"


```

```bash
User@DESKTOP-OBLBJ4I MINGW64 ~/Desktop/dubl
$ ./7.sh .
Dublicates:
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka (2).c
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka (2).js
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka (2).py
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka.c
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka.js
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 *./proverka.py


```
