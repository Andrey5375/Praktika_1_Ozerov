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



