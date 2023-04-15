---
## Front matter
title: "Лабораторная работа №4"
subtitle: "Модель гармонических колебаний"
author: "Кузнецов Юрий Владимирович"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Рассмотреть уравнение гармонических колебаний. Построить модель гармонических колебаний при помощи OpenModellica и Julia.

# Задачи

Построить фазовый портрет гармонического осциллятора и решение уравнения гармонического осциллятора для следующих случаев

1. Колебания гармонического осциллятора без затуханий и без действий внешней силы $\dot x + 3.3x = 0$.

2. Колебания гармонического осциллятора c затуханием и без действий внешней силы $\ddot x + 3\dot x + 0.3x = 0$.

3. Колебания гармонического осциллятора c затуханием и под действием внешней силы $\ddot x + 3.3\dot x + 0.3x = 3.3\sin(3t)$.

На интервале $t \in [0; 33]$ (шаг $0.05$) с начальными условиями $x_0 = 1.3, \ y_0 = 0.3$.

# Среда

-   Julia – это открытый свободный высокопроизводительный динамический язык высокого уровня, созданный специально для технических (математических) вычислений. Его синтаксис близок к синтаксису других сред технических вычислений, таких как Matlab и Octave. [@unn-julia]

-   OpenModelica — свободное открытое программное обеспечение для моделирования, симуляции, оптимизации и анализа сложных динамических систем. Основано на языке Modelica. [@wiki-om]

# Теоретическое введение

Движение грузика на пружинке, маятника, заряда в электрическом контуре, а также эволюция во времени многих систем в физике, химии, биологии и других науках при определенных предположениях можно описать одним и тем же дифференциальным уравнением, которое в теории колебаний выступает в качестве основной модели. [@rudn-task]

Эта модель называется линейным гармоническим осциллятором. Уравнение свободных колебаний гармонического осциллятора имеет следующий вид:

$$\ddot x + 2 \gamma \dot x + \omega_0^2 x = 0$$

где $x$ – переменная, описывающая состояние системы (смещение грузика, заряд конденсатора и т.д.), $\gamma$ – параметр, характеризующий потери энергии (трение в механической системе, сопротивление в контуре), $\omega_0$ – собственная частота колебаний, $t$ – время. (Обозначения $\ddot x = \frac{\partial^2x}{\partial^2t}$, $\dot x = \frac{\partial x}{\partial t}$)

При отсутствии потерь в системе вместо вышекуказанного уравнения получаем уравнение консервативного осциллятора энергия колебания которого сохраняется во времени

$$\ddot x + \omega_0^2x = 0$$

Для однозначной разрешимости уравнения второго порядка необходимо задать два начальных условия вида

$$
\left\{
\begin{array}{c}
x(t_0) = x_0
\\ \\
\dot x (t_0) = y_0
\end{array}
\right.
$$

Уравнение консервативного осциллятора энергия колебания которого сохраняется во времени можно представить в виде системы двух уравнений первого порядка:

$$
\left\{
\begin{array}{c}
\dot x = y
\\ \\
\dot y = - \omega_0^2x
\end{array}
\right.
$$

Начальные условия для системы примут вид:

$$
\left\{
\begin{array}{c}
x(t_0) = x_0
\\ \\
y (t_0) = y_0
\end{array}
\right.
$$

Независимые переменные $x$, $y$ определяют пространство, в котором «движется» решение. Это фазовое пространство системы, поскольку оно двумерно будем называть его фазовой плоскостью.

Значение фазовых координат $x$, $y$ в любой момент времени полностью определяет состояние системы. Решению уравнения движения как функции времени отвечает гладкая кривая в фазовой плоскости. Она называется фазовой траекторией. Если множество различных решений (соответствующих различным начальным условиям) изобразить на одной фазовой плоскости, возникает общая картина поведения системы. Такую картину, образованную набором фазовых траекторий, называют фазовым портретом.

# Ход работы

Запустим Pluto для выполнения первых задач.

![Julia. Запуск Pluto](src/1.png)

Подключаем Plots и DifferentialEquations. Объявляем начальные данные при помощи констант. Устанавливаем начальное условие для системы ДУ и промежуток времени для визуализации. 

```
using Plots
using DifferentialEquations

const startT = 0
const endT = 33
const stepT = 0.05
const x0 = 1.3
const y0 = 0.3

u0 = [x0, y0]
spanT = (startT, endT)
```

![Julia. Начало написания скрипта для моделирование колебания гармонического осциллятора](src/2.png)

В следующей ячейке Pluto построим фазовый портрет и решение уравнения гармонического осциллятора.  Воспользуемся данным списком, чтобы построить фазовый портрет. 

```
w = 3.3

function Fluctuations!(df, u, p, t)
  df[1] = u[2]
  df[2] = -w * u[1]
end

prob = ODEProblem(Fluctuations!, u0, spanT)
sol = solve(prob, dtmax=stepT)

X = [u[1] for u in sol.u]
Y = [u[2] for u in sol.u]

plt01 = plot(sol,
      dpi=500,
      xlabel="Время (s)",
      ylabel="x, y",
      legend=false)
savefig(plt01, "labart/result.png")

plt02 = plot(X, Y,
      dpi=500,
      xlabel="x",
      ylabel="y",
      legend=false)
savefig(plt02, "labart/result2.png")

println("Success!")
```

![Julia. Скрипт. Колебания гармонического осциллятора без затуханий и без действий внешней силы](src/3.png)

![Julia. Модель. Решение уравнения гармонического осциллятора без затуханий и без действий внешней силы](labart/result.png)

![Julia. Модель. Фазовый портрет осциллятора без затуханий и без действий внешней силы](labart/result2.png)

Доработаем данный скрипт, чтобы построить решение уравнения и фазовый портрет гармонического осциллятора c затуханием и без действий внешней силы. Для этого нам неободимо добавить новый параметр - затухание. Также необходимо изменить функцию системы ДУ. 

```
w = 0.3 
g = 3 

function Fluctuations!(df, u, p, t)
  df[1] = u[2]
  df[2] = -w * u[1] - g * u[2] 
end

prob = ODEProblem(Fluctuations!, u0, spanT)
sol = solve(prob, dtmax=stepT)

X = [u[1] for u in sol.u]
Y = [u[2] for u in sol.u]

plt01 = plot(sol,
      dpi=500,
      xlabel="Время",
      ylabel="x, y",
      legend=false)
savefig(plt01, "labart/result_1.png")

plt02 = plot(X, Y,
      dpi=500,
      xlabel="x",
      ylabel="y",
      legend=false)
savefig(plt02, "labart/result_2.png")

println("Complete!")
```

![Julia. Скрипт. Колебания гармонического осциллятора с затуханием и без действий внешней силы](src/4.png)

![Julia. Модель. Решение уравнения гармонического осциллятора с затуханием и без действий внешней силы](labart/result_1.png)

![Julia. Модель. Фазовый портрет осциллятора с затуханием и без действий внешней силы](labart/result_2.png)

Еще раз доработаем скрипт, чтобы построить решение уравнения и фазовый портрет гармонического осциллятора c затуханием и под действием внешней силы. Для этого нам неободимо добавить новый параметр - функция внешней силы. Также необходимо изменить функцию системы ДУ. 

```
w = 3 
g = 3.3 
f(t) = 3.3 * sin.(3 * t) 

function Fluctuations!(df, u, p, t)
  df[1] = u[2]
  df[2] = -w * u[1] - g * u[2] - f(t) 
end

prob = ODEProblem(Fluctuations!, u0, spanT)
sol = solve(prob, dtmax=stepT)

X = [u[1] for u in sol.u]
Y = [u[2] for u in sol.u]

plt01 = plot(sol,
      dpi=500,
      xlabel="Время",
      ylabel="x, y",
      legend=false)
savefig(plt01, "labart/result_1_1.png")

plt02 = plot(X, Y,
      dpi=500,
      xlabel="x",
      ylabel="y",
      legend=false)
savefig(plt02, "labart/result_1_2.png")

println("OK!")
```

![Julia. Скрипт. Колебания гармонического осциллятора с затуханием и под действием внешней силы](src/5.png)

![Julia. Модель. Решение уравнения гармонического осциллятора с затуханием и под действием внешней силы](labart/result_1_1.png)

![Julia. Модель. Фазовый портрет осциллятора с затуханием и под действием внешней силы](labart/result_1_2.png)

Построим модель колебания гармонического осциллятора без затуханий и без действий внешней силы на Modelica.

```
model Var1
    constant Real w = 3.3;
    Real x;
    Real y;
    Real t = time;
initial equation
    x = 1.3;
    y = 0.3;
equation
  der(x) = y;
  der(y) = -w * x;
  annotation(experiment(StartTime = 0, StopTime = 33, Interval = 0.05));
end Var1;
```

![Modelica. Скрипт. Колебания гармонического осциллятора без затуханий и без действий внешней силы](src/6.png)

![Modelica. Модель. Решение уравнения гармонического осциллятора без затуханий и без действий внешней силы](src/MO.lab04-010.png)

![Modelica. Модель. Фазовый портрет осциллятора без затуханий и без действий внешней силы](src/MO.lab04-011.png)

Построим модель колебания гармонического осциллятора с затуханием и без действий внешней силы на Modelica. 

```
model Var2
    constant Real w = 0.3;
    constant Real g = 3;
    Real x;
    Real y;
    Real t = time;
initial equation
    x = 1.3;
    y = 0.3;
equation
  der(x) = y;
  der(y) = -w * x - g * y;
  annotation(experiment(StartTime = 0, StopTime = 33, Interval = 0.05));
end Var2;
```

![Modelica. Скрипт. Колебания гармонического осциллятора с затуханием и без действий внешней силы](src/7.png)

![Modelica. Модель. Решение уравнения гармонического осциллятора с затуханием и без действий внешней силы](src/MO.lab04-020.png)

![Modelica. Модель. Фазовый портрет осциллятора с затуханием и без действий внешней силы](src/MO.lab04-021.png)

Построим модель колебания гармонического осциллятора с затуханием и под действием внешней силы на Modelica. 

```
model Var3
    constant Real w = 0.3;
    constant Real g = 3;
    Real x;
    Real y;
    Real t = time;
initial equation
    x = 1.3;
    y = 0.3;
equation
  der(x) = y;
  der(y) = -w * x - g * y;
  annotation(experiment(StartTime = 0, StopTime = 33, Interval = 0.05));
end Var3;
```

![Modelica. Скрипт. Колебания гармонического осциллятора с затуханием и под действием внешней силы](src/8.png)

![Modelica. Модель. Решение уравнения гармонического осциллятора с затуханием и под действием внешней силы](src/MO.lab04-030.png)

![Modelica. Модель. Фазовый портрет осциллятора с затуханием и без под действием внешней силы](src/MO.lab04-031.png)

# Вывод

Повысили навыки в написании программ на Julia. Улучшили понимание моделирования на OpenModelica. Познакомились с Pluto. Повысили навыки в решении ДУ.

# Ресурсы

* 	Julia. URL: http://www.unn.ru/books/met_files/JULIA_tutorial.pdf.
* 	OpenModelica. URL: https://ru.wikipedia.org/wiki/OpenModelica.
* 	Модель гармонических колебаний. RUDN. URL: https://esystem.rudn.ru/mod/resource/view.php?id=967241.
* 	Pluto. URL: https://plutojl.org/.
* 	Plots in Julia. URL: https://docs.juliaplots.org/latest/tutorial/.
* 	Differential Equations in Julia. URL: https://docs.sciml.ai/DiffEqDocs/stable/getting_started/.
