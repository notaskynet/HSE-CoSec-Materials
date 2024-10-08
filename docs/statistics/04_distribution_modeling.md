---
prev:
  text: 'Теория оценок'
  link: '/statistics/03_evaluation_theory'
next:
  text: 'Порядковая статистика'
  link: '/statistics/05_ordinal_statistics'
outline: deep
---

# Моделирование распределений

## 1. Моделирование дискретных распределений

Пусть есть хороший источник случайных чисел $u_{i}\sim R[0;1]$. Тогда мы попадаем в лискретное значение $x_i$ с вероятностью $p_i$:

> $x_{1} \dots$ - некоторые случайные величины
> $p_{1} \dots$ - вероятности появления этих случайных величин

![alt](https://i.imgur.com/HvckFSB.png)

Код будет выглядеть примерно так:

```python
import numpy as np
u = np.random.uniform(0, 1)
prob_u = 0
probs = [ "Здесь будут вероятности возможных исходов" ]
i = 0
while(u > prob_u):
    prob += probs[i]
    i += 1
print(prob)
```

Для биномиальных и отрицательных биномиальных СВ, тоже есть свой подход.

## 2. Моделирование абстрактных непрерывных распределений

### 2.1. Для функций, представимым некоторой комбинацией элементарных функций

Тогда идея в следующем:
$$U_i \sim R[0;1]$$
$$F_{\xi}(x)=P(\xi\leq x)$$
$F_{\xi}^{-1}(x)$ - новая случайная величина. Тогда:
$$P(F_{\xi}^{-1}(u_{i})\leq x)=P(u_{i} \leq F_{\xi}(x)) = F_{\xi}(x) $$

### 2.2. Моделирование нормального распределения

Нормальное распределение:
$$f(x) = \frac{1}{\sqrt{ 2 \pi \sigma^2 }} e^\frac{-(x - \mu)}{2 \sigma^2}, x \in \mathbb{R}$$

**Преобразование Бохса-Мюллера:**

1. Берем последовательность наблюдений за генератором случаных величин $u_{1} \dots \sim R[0;1]$
2. Рассматриваем случайные велисины
$$ \xi_{1} = \sqrt{ -2\ln u_{1} } \cos 2 \pi u_{2}$$
$$ \xi_{2} = \sqrt{ -2\ln u_{1} } \sin 2 \pi u_{2}$$
    Если они независсимы, то они будут иметь нормальное распределение. На основании их можно восстановить нормальное расределение\

Идея преобразования Бохса-Миллера состоит в переходе к полярным координатам.
![alt](https://i.imgur.com/khU228j.png)
Получается так, что независимо от расположения(угол), вероятность попасть в тот же сектор той велечины не отлтчается, поэтому у угла равномерное распределение.
$$ \xi_{1}^{2} + \xi_{2}^{2} \sim \exp\left( \frac{1}{2} \right)$$
$$1 - e^{-x/2} = t, x>0$$
$$1 - t = e^{-x/2} \implies x = -2 \ln (1 - t)$$
Итого мы получим $\xi_{1}, \xi_{2}\sim N(0;1)$. Чтобы получить $N(\theta, \sigma^{2})$, нужно рассмотреть $\eta_{i} = \sigma \xi_{i} + \theta$.

### 2.3. Связанные с номальным распределения

Получались как погрешности при движение по прямой и по плоскости

#### Распределение Рэлея

$$ \xi_{1}, \xi_{2} \sim N(0;\sigma^2)$$
$$\eta = \sqrt{ \xi_1^2 + \xi_{2}^2 }$$

#### Распределение Максвела

$$ \xi_{1}, \xi_{2}, \xi_{3} \sim N(0;\sigma^2)$$
$$\eta = \sqrt{ \xi_1^2 + \xi_{2}^2 + \xi_{3}^2 }$$

#### Распределение хи-квадрат (частный случай гамма-распределения)

$$\xi_{1}\dots \xi_{n} \sim N(0;\sigma^2)$$
$$\eta = \xi_{1}^2 + \dots +\xi_{n}^2$$

$$P = \sum_{p=k}^{n} \frac{(p-1)!}{n!}$$
