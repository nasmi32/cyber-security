---
## Front matter
lang: ru-RU
title: Лабораторная работа №5
subtitle: Математическое моделирование
author:
  - Мишина А. А.
date: 17 апреля 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Мишина Анастасия Алексеевна
  * НПИбд-02-22
  * <https://github.com/nasmi32>

:::
::: {.column width="30%"}


:::
::::::::::::::

## Цели и задачи

- Исследовать математическую модель Лотки-Волтьерры.


## Задача

Для модели «хищник-жертва»:

$$\begin{cases}
    &\dfrac{dx}{dt} = - 0.34 x(t) + 0.051 x(t)y(t) \\
    &\dfrac{dy}{dt} = 0.33 y(t) - 0.031 x(t)y(t)
\end{cases}$$

Построить график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при следующих начальных условиях: $x_0 = 9, y_0 = 30.$ Найти стационарное состояние системы.

# Выполнение лабораторной работы

# Реализация на Julia

## Код

```Julia
# Используемые библиотеки
using DifferentialEquations, Plots

# Задание системы ДУ, описывающей модель Лотки-Вольтерры
function LV(u, p, t)
    x, y = u
    a, b, c, d = p
    dx = a*x - b*x*y
    dy = -c*y + d*x*y
    return [dx, dy]
end
```

## Код

```
# Начальные условия
u0 = [9, 30]
p = [-0.34, -0.051, -0.33, -0.031]
tspan = (0.0, 50.0)

# Постановка проблемы и ее решение
prob = ODEProblem(LV, u0, tspan, p)
sol = solve(prob, Tsit5())

# Построение графика
plot(sol, title = "Модель Лотки-Вольтерры", xaxis = "Время", yaxis = "Численность популяции", label = ["жертвы" "хищники"], c = ["green" "red"], box =:on)
```

## График

![График изменения численности хищников и численности жертв](image/1.png){#fig:1 width=70%}

## График

![График зависимости численности хищников от численности жертв](image/2.png){#fig:2 width=70%}

## Стационарное состояние системы

$$\begin{cases}
  &x_0 = \dfrac{\gamma}{\delta}\\
  &y_0 = \dfrac{\alpha}{\beta}
\end{cases}
$$

## Стационарное состояние системы


В результате, $x_0 = \dfrac{0.33}{0.031} = 10.645161290322582$, а $y_0 = \dfrac{0.34}{0.051} = 6.666666666666668$.

## Проверка

```Julia
u0_c = [x_c, y_c]
prob2 = ODEProblem(LV, u0_c, tspan, p)
sol2 = solve(prob2, Tsit5())

plot(sol2, xaxis = "Жертвы", yaxis = "Хищники", label = ["Жертвы" "Хищники"], c = ["green" "purple"], box =:on)
plot((x_c, y_c), seriestype=:scatter, xlims=(3, 15), ylims=(3, 15), box =:on, c = "blue", markersize=10, label = "Стационарная точка")
```

## Проверка

![График изменения численности хищников и численности жертв в стационарном состоянии](image/3.png){#fig:3 width=70%}

## Проверка

![График зависимости численности хищников от численности жертв в стационарном состоянии](image/4.png){#fig:4 width=70%}

# Реализация на OpenModelica

## Код

```
model lab5_1
  parameter Real a = -0.34;
  parameter Real b = -0.051;
  parameter Real c = -0.33;
  parameter Real d = -0.031;
  parameter Real x0 = 9;
  parameter Real y0 = 30;

  Real x(start=x0);
  Real y(start=y0);
equation
    der(x) = a*x - b*x*y;
    der(y) = -c*y + d*x*y;
end lab5_1;
```

## График

![График изменения численности хищников и численности жертв в OpenModelica](image/5.png){#fig:5 width=70%}

## График

![График зависимости численности хищников от численности жертв в OpenModelica](image/6.png){#fig:6 width=70%}

## Стационарное состояние системы

```
model lab5_2
  parameter Real a = -0.34;
  parameter Real b = -0.051;
  parameter Real c = -0.33;
  parameter Real d = -0.031;
  parameter Real x0 = 0.33/0.031;
  parameter Real y0 = 0.34/0.051;

  Real x(start=x0);
  Real y(start=y0);
equation
    der(x) = a*x - b*x*y;
    der(y) = -c*y + d*x*y;
end lab5_2;
```

## График

![График изменения численности хищников и численности жертв в стационарном состоянии](image/7.png){#fig:7 width=70%}

## Вывод

В результате выполнения лабораторной работы я построила математическую модель Лотки-Вольтерры на Julia и в OpenModelica.