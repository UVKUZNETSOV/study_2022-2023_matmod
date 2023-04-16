---
# Front matter
lang: ru-RU
title: "Лабораторная работа 1"
subtitle: "GitHub. Markdown"
author: "Кузнецов Юрий Владимирович"

# Formatting
toc-title: "Содержание"
toc: true # Table of contents
toc_depth: 2
lof: true # Список изображений
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: true
pdf-engine: lualatex
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы
Создать каталоги в системе для работы. Создать репозиторий. Написать отчет – презентацию, используя MarkDown.

# Термины
* Система контроля версий — это система, регистрирующая изменения в одном или нескольких файлах с тем, чтобы в дальнейшем была возможность вернуться к определённым старым версиям этих файлов.

* Репозиторий — это папка, в которую Git отслеживает изменения. На компьютере может быть любое количество репозиториев, каждое из которых хранится в собственной папке. Каждый репозиторий Git в системе является независимым, поэтому изменения, сохраненные в одном репозитории Git, не влияют на содержимое другого.

* Markdown — это облегченный язык разметки с синтаксисом форматирования обычного текста. 

* Pandoc — это бесплатный конвертер документов с открытым исходным кодом, широко используемый в качестве инструмента написания (особенно учеными) и в качестве основы для публикации рабочих процессов.

# Ход работы.
  В домашнем каталоге создаём подкаталог work.
```
mkdir work/study/2022-2023/"Математическое моделирование"/matmod
```
![Подготовка рабочей среды](img/6.png)
![Подготовка рабочей среды](img/5.png)
   
  Создадим репозиторий с названием study_2022-2023_matmod. 
   
![Создание репозитория](img/4.png)

  В каталоге создадим следующие файлы:
```
touch LICENSE.md, touch CHANGELOG.md, touch README.md, touch .gitignore
```
![Создание README.md, .gitignore, CHANGELOG.md, LICENSE.md](img/3.png)

  Заполняем файлы информацией. В файле .gitignore пописываем элементы для языка Julia. 

![.gitignore для Julia](src/2.png)

  На github сгенерируем токен для отправки файлов на сервер. 

![Git Token Classic](img/1.png)

  Инициализируем git и делаем коммит. 
```
git init
git add LICENSE.md
git add CHANGELOG.md
git add README.md
git add .gitignore
git commit -m "init commit"
git branch -M main
git remote add origin https://github.com/UVKUZNETSOV/study_2022-2023_matmod.git
git push https://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@github.com/UVKUZNETSOV/study_2022-2023_matmod.git
```
![Init commit (local)](img/7.png)

![Init commit (git)](img/8.png)

  Создадим папку labs. В папке создадим каталог lab01, в котором пропишем report.md. 
```
mkdir -p labs/lab01, cd labs/lab01/, touch report.md
```
![Создание report.md](img/9.png)

  Создадим коммит отчета.
``` 
git add labs/lab01/report.md,
git commit -m "empty report",
git push https://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@github.com/av-ilin/study_2022-2023_mathmod.git
```
![Commit report.md](img/10.png)
  Конвертируем отчет используя pandoc. 
```
pandoc report.md -o report.pdf
pandoc report.md -o report.docx
```
![Markdown to PDF](img/11.png)

# Вывод
Мы повторили алгоритм работы с github и ознакомились с базовой структурой написания отчётов на MarkDown. 

# Ресурсы
* github.com
* en.wikipedia.org
