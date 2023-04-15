---
# Front matter
lang: ru-RU
title: "Лабораторная работа №2"
subtitle: "Задача о погоне"
author: "Кузнецов Юрий Владимирович"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

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
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
    - parentracker=true
    - backend=biber
    - hyperref=auto
    - language=auto
    - autolang=other*
    - citestyle=gost-numeric
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

Решить задачу о погоне. Смоделировать кривую погони при помощи Julia и OpenModelica.

# Задачи

1. Провести аналогичные рассуждения и вывод дифференциальных уравнений, если скорость катера больше скорости лодки в n раз.
2. Построить траекторию движения катера и лодки для двух случаев.
3. Определить по графику точку пересечения катера и лодки

# Термины

* Задача о погоне — классических задача из области дифференциальных уравнений. 

* Кривая погони — кривая, представляющая собой решение задачи о «погоне». 

* Julia – это открытый свободный высокопроизводительный динамический язык высокого уровня, созданный специально для технических (математических) вычислений. Его синтаксис близок к синтаксису других сред технических вычислений, таких как Matlab и Octave. 

* OpenModelica — свободное открытое программное обеспечение для моделирования, симуляции, оптимизации и анализа сложных динамических систем. Основано на языке Modelica. 

# Теоретическая справка
Для построения кривой погони, нам необходимо знать начальные условия и уравнения кривой. Ориентируясь на рассуждения из пособия к лабораторной работы можно вывести общие формулы. Благодаря общим формулам можно будет написать программу, которая будет строить разные кривые погони в зависимости от исходных данных (расстояния и разницы в скорости).

Пусть 

* n - разница в скорости, то есть скорость катера в n раз больше лодки;

* a - расстояние между катером и лодкой в момент рассеивания тумана.

Тогда общие начальные условия для первого случая выглядят следующим образом:
$$
\left\{ 
\begin{array}{c}
\theta_0 = 0 \\ 
r_0 = \frac{a}{(n + 1)}
\end{array}
\right. 
$$

Для второго случая:
$$
\left\{ 
\begin{array}{c}
\theta_0 = -\pi \\ 
r_0 = \frac{a}{(n - 1)}
\end{array}
\right. 
$$

Уравнение кривой в общем случае выглядит следующим образом:
$$r(\theta) = r_0 e ^ {\frac{\theta}{\sqrt{n^2 - 1}}}$$


# Выполнение лабораторной работы.

1. Установим пакет в Julia необходимый для построения графика. 

import Pkg
Pkg.add("Plots")

![Установка пакета 'Plots'](src/1.png)

2. Для подключения графики импортируем пакет Plots при помощи команды using. Константы a и n - входные данные, thetadeg - направление лодки, dTheta - шаг для равномерного разбиения периода,  maxTheta - длина периода построения, cases - содержит два кейса, по которым мы будем итерироваться. 

![Начало написания скрипта](src/2.png)

Следующие итерации будут проходить в цикле следующим образом:

for item in list
    
end
Также будем использовать условные операторы:

if condition
  
else
  
end

![Скрипт кейсы](src/3.png)

![Полный скрипт](src/4.png)


Вывод программы:

![Кривая погони №1](src/5.png)

![Кривая погони №2](src/6.png)

На OpenModelica используя базовые настройки построение графика невозможно. В базовой настройке, графики могут выстраиваться исключительно в системе полярных координат. 

# Код
```Julia
using Plots

const a = 18.5
const n = 6.1
const thetadeg = 295
const dTheta = 0.02
const maxTheta = 4pi
const cases = [" 1st ", " 2nd "]

function F(theta)
    return r0 * exp.(theta / sqrt.(n^2 - 1))
end

for case in cases
    global r0 = -1
    theta0 = -1

    if case==" 1st "
        r0 = a / (n + 1)
        theta0 = 0
    else
        r0 = a / (n - 1)
        theta0 = -pi
    end

    theta1 = theta0 + maxTheta
    thetaHunt = theta0:dTheta:theta1
    thetaPray = thetadeg * pi / 180 + 2 * theta0

    plt = plot(proj=:polar, aspect_ratio=:equal, dpi=500, title= case * "Solution", legend=true)
    plot!(plt, [theta0, theta0], [a, F(theta0)], label=false, color=:purple)
    plot!(plt, thetaHunt, F, label=:"Траектория охраны" ,color=:purple)
    plot!(plt, [0, thetaPray], [0, F(thetaPray) + 20], label=:"Траектория браконьеров", color=:yellow)

    plot!(plt, [theta0], [a], seriestype=:scatter, label=:"Точка начала охраны", color=:purple)
    plot!(plt, [0], [0], seriestype=:scatter, label=:"Точка начала браконьеров", color=:yellow)
    plot!(plt, [thetaPray], [F(thetaPray)], seriestype=:scatter, label=:"Точка встречи", color=:red)

    savefig(plt, "Lab02" * case * "Case.png")
end
```

# Вывод

Ознакомились с Julia подробнее – что дало возможность улучшить навыки в решении дифференциальных уравнений. 

# Ресурсы

*	Задача о погоне (https://esystem.rudn.ru/mod/resource/view.php?id=967233)
*    Кривая погони (https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%BF%D0%BE%D0%B3%D0%BE%D0%BD%D0%B8)
*	Julia (http://www.unn.ru/books/met_files/JULIA_tutorial.pdf)
*	OpenModelica (https://ru.wikipedia.org/wiki/OpenModelica)
