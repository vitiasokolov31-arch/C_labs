# Практическое задание 9. Git

## Оценка 3

### Шаги 1–2. Текущая ветка и журнал

```bash
git status
git log
```

<!-- Скриншот: вывод git status и git log -->
![status и log](img/01.png)

### Шаги 3–6. Создание sort.c и добавление в stage
<!-- Скриншот: оба вывода git status - до add и после -->
![sort.c до и после add](img/02.png)

До `git add` файл находится в `Untracked files`: Git его ещё не отслеживает. После `add` он попадает в `Changes to be committed` как `new file`.

### Шаги 7–8. Коммит

<!-- Скриншот: коммит и git status -->
![коммит](img/03.png)

### Шаги 9–12. Изменение файла

В начало `sort.c` добавлен комментарий.


<!-- Скриншот: оба вывода git status -->
![modified](img/04.png)

### Шаги 13–15. Ещё одно изменение и коммит

убрал комментарий, после чего сразу сделан коммит без `git add`.


<!-- Скриншот: git status и git log --oneline -->
![коммит только staged](img/05.png)

В коммит попал комментарий, потому что он был в stage. `git commit` сохраняет содержимое stage, а не текущее состояние файла.

### Шаги 16–17. Коммит последнего изменения и push


<!-- Скриншот: вывод git push -->
![push](img/06.png)

---

## Оценка 3: ветки

### Шаги 1–5. Создание ветки mybranch


<!-- Скриншот: git branch и git status на mybranch -->
![mybranch](img/07.png)

После переключения `git status` пишет `On branch mybranch` вместо `On branch master`.

### Шаги 6–8. Коммит file1.txt в mybranch
```bash
echo "Виктор" > file1.txt
git add file1.txt
git commit -m "Add file1.txt"
git log --oneline --graph
```

### Шаги 9–12. Коммит file2.txt в master

<!-- Скриншот: последний git log --oneline --graph --all с развилкой -->
![развилка веток](img/08.png)

В `master` нет коммита `Add file1.txt`. На графе видна развилка: у каждой ветки свой коммит.

### Шаги 13–15. Сравнение веток


<!-- Скриншот: ls и git diff -->
![diff веток](img/09.png)

`file2.txt` пропал, потому что он есть только в `master`. При переключении ветки Git подставляет в папку файлы этой ветки.

### Шаги 16–17. Push веток на GitHub

<!-- Скриншот: страница репозитория на GitHub со списком веток master и mybranch -->
![ветки на GitHub](img/10.png)

---

## Оценка 4

### Шаги 1–4. Добавление main в sort.c



```bash
git diff
git diff --staged
```

<!-- Скриншот: git diff (видны строки с +) и пустой git diff --staged -->
![diff до add](img/11.png)

### Шаги 5–7. После add


<!-- Скриншот: пустой git diff и git diff --staged -->
![diff после add](img/12.png)

### Шаги 8–11. Изменение после add

В массиве удалено последнее число: `{4, 2, 0, 7, 1}` → `{4, 2, 0, 7}`.


<!-- Скриншот: git diff и git diff --staged -->
![два diff](img/13.png)

Сейчас у файла три версии: в последнем коммите (без `main`), в stage (`main` и пять чисел) и в рабочей папке (`main` и четыре числа). `git diff` сравнивает рабочую папку со stage и показывает только удалённое число. `git diff --staged` сравнивает stage с последним коммитом и показывает добавление `main`.

### Шаги 12–16. restore --staged и коммит


<!-- Скриншот: оба git status (sort.c дважды, затем один раз) и git log -->
![restore --staged](img/14.png)

`sort.c` дважды встречается в `git status`: один раз в stage, второй раз как изменение после `add`. `git restore --staged` убирает изменения из stage, но сам файл не трогает.

### Шаги 17–22. Отмена изменения через restore

В `main` добавлена строка `printf("hello git\n");`.


<!-- Скриншот: cat и git status до и после git restore -->
![restore](img/15.png)

`git restore` возвращает файл к версии из последнего коммита: строка с `printf` пропала, несохранённые изменения потеряны.

### Шаг 23

```bash
git push
```

---

## Оценка 4: ветки и ff-merge

### Шаги 1–9. greeting.txt и ветка feature/uppercase


<!-- Скриншот: git branch и git log --oneline --graph --all -->
![feature/uppercase](img/16.png)

### Шаги 10–14. Fast-forward merge

<!-- Скриншот: cat, diff, merge со словом Fast-forward и снова cat -->
![fast-forward](img/17.png)

Git выполнил **fast-forward**: в `master` не было новых коммитов после создания `feature/uppercase`, поэтому Git просто передвинул `master` вперёд, без отдельного коммита слияния.

### Шаги 15–17. Слияние mybranch в master


<!-- Скриншот: git log --oneline --graph --all после слияния -->
![merge mybranch](img/18.png)

Здесь fast-forward невозможен, потому что у обеих веток есть свои коммиты. Git создал коммит слияния с двумя родителями, на графе две линии сходятся в нём.

### Шаги 18–19

```
git push
```

---

## Оценка 5: конфликт в file.txt

### Шаги 1–6. Разные изменения одного файла в двух ветках

```bash
git switch -c branch1
echo "This is a relevant fact" > file.txt
git add file.txt
git commit -m "Add relevant fact"

git switch master
echo "This is an indispensable truth!" > file.txt
git add file.txt
git commit -m "Add indispensable truth"
git log --oneline --graph --all
```

### Шаги 7–8. Конфликт

```bash
git merge branch1
git status
```

<!-- Скриншот: сообщение CONFLICT и git status -->
![конфликт](img/19.png)

Обе ветки по-разному изменили одну строку, поэтому Git не может сам решить, что оставить.

### Шаг 9. Исправление конфликта

<!-- Скриншот: file.txt в VS Code с маркерами конфликта -->
![конфликт в VS Code](img/20.png)

Между `<<<<<<< HEAD` и `=======` лежит версия из `master`, между `=======` и `>>>>>>> branch1` лежит версия из `branch1`. Выбрано **Accept Both Changes**, чтобы оставить обе строки.


### Шаги 10–11


<!-- Скриншот: git log --oneline --graph с коммитом слияния -->
![после конфликта](img/21.png)

---

## Оценка 5: конфликт в mergesort.py

### Шаги 1–9. Две версии mergesort.py

<!-- Скриншот: git log --oneline --graph --all и git branch -->
![две версии](img/22.png)

### Шаг 10. Слияние и исправление конфликтов


<!-- Скриншот: mergesort.py в VS Code с конфликтами -->
![конфликты mergesort](img/23.png)

В файле четыре конфликта: разный порядок строк `left`/`right`, пустая строка между функциями и разные имена четвёртой части списка (`last` в `master`, `fourth` в `Mergesort-Impl`). Оставлен вариант из `master` и пустая строка между функциями. Имя переменной надо выбрать одинаково во всех местах, иначе получится несуществующая переменная и ошибка `NameError`.


### Шаги 11–12

```bash
git push
```

<!-- Скриншот: репозиторий на GitHub после финального push -->
![итог](img/24.png)
