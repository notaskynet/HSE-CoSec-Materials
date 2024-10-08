---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Градиентный бустинг (Gradient Boosting Machine)

![alt](https://i.imgur.com/e3VqY7l.png)

**Идея**: Пусть каждая следующая модель корректирует ошибки предыдущих

1. Бустинг для MSE
$$\frac{1}{\ell} \sum_{i=1}^{\ell} (a_{i} - y_{u})^{2} \to \mathop{\text{min}}_{ a } $$
Пусть $a(x) = \sum_{n=1}^{N} \gamma_{n}b_{n}(x), \gamma_{n} \in \mathbb{R}$

Будем послудовательно обучать модели:
- $b_{1}(x)$:
$$
\begin{equation}
    \left[  
    \begin{array}{lcl}  
        \frac{1}{\ell}\sum_{i=1}^{\ell} (b_{1}(x_{i})-y_{i})^{2} \to \mathop{\text{min}}_{ b_{1}(x ) }\\  
        \gamma_{1}=1\\  
    \end{array}  
    \right.
\end{equation} \implies \text{Фиксируем } b_{1}(x) \text{ для след. шагов}
$$
- $b_{2}(x)$:
$$b_{1}(x) + b_{2}(x) = s_{i} \in \mathbb{R}$$
$$\frac{1}{\ell} \sum_{i=1}^{\ell} (b_{2}(x_{i}) - s_{i})^{2} \to \mathop{\text{min}}_{ b_{2}  }  $$
$$\gamma_{2} = \mathop{\text{argmin}}_{ \gamma \in \mathbb{R}} \frac{1}{\ell} \sum_{i=1}^{\ell} (b_{1}(x_{i}) + \gamma b_{2}(x_{i}) - y_{i})^{2} $$
- $b_{3}:$ ... (ну и так далле)
- Когда останавливаться: ГБ переобучается, пот\тому его необходимо следить по валидационной выборке
![alt](https://i.imgur.com/ZwT44JC.png)

## 2. Общий случай

Рассмотрим общий случай:

- $L(y, z)$ - дифференцируемая функция потерь
- $a_{N}(x) = \frac{1}{N} \sum_{i=1}^{N} b_{n}(x)$ (пока без $\gamma_{n}$)

Как обучать?

>[!note] Почему бы просто не повторить алгоритм для MSE?
$$ b_{N}: \frac{1}{\ell} \sum_{i=1}^{\ell} L(y_{i} - a_{N -1}(x_{i}), b_{N}(x_{i})) \to \text{Это плохой подход!}$$
Этот подход плохой, так как мы никак не меняем прогнозы на признаковом пространстве(мы никак не меняем прогнозы на $x \in \mathbb{X}$ ), а просто подстраиваем нашу модель под обучающую выборку.

Строим метод:
- $b_{1}(x)$ - обучаем как есть
- $b_{N}(x)$: будем считать, что $b_{1}(x)\dots b_{N-1}(x)$ уже построены:
$$ b_{N}(x): \frac{1}{\ell} \sum_{i=1}^{\ell} L(y_{i}, a_{N-1}(x_{i}) + b_{N}(x_{i})) \to \text{Как обучать?}$$
![alt](https://i.imgur.com/ScONOxq.png)
> Чтобы понять, куда двигаться, нусжно смотреть на частную производную

$$
\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        s_{i} = - \frac{\partial}{\partial z}L(y_{i}, z)|_{x=a_{N-1}(x_{i})} - \text{сдвиг}\\  
        \frac{1}{\ell} \sum_{i=1}^{\ell} (b_{N}(x_{i}) - s_{i})^{2} \to \mathop{\text{min}}_{ b_{N} } \\
        a_{N}(x) = a_{N-1}(x) + b_{N}(x) 
    \end{array} 
    \right. 
\end{equation} \implies \text{Алгоритм обучения град. бустинга}
$$
Примечание: $s_{i}$ аоказывает как нужно поменять прогноз на каждом объекте

Запишем функционал:
$$Q(z_{1} \dots z_{\ell}) = \frac{1}{\ell} \sum_{i=1}^{\ell}L(y_{i}, a_{N-1}(x_{i})+z_{i}) $$
$$(s_{1}\dots s_{\ell}) = - \nabla_{z}Q(z)$$
*Пример*: $X=\left((x_{1}, y_{1}), (x_{2}, y_{2})\right)$
$$\frac{1}{2}(L(y_{1}, z_{1}) + L(y_{2}, z_{2})) \to \text{минимизируем}$$
![alt](https://i.imgur.com/DXipNam.png)
> Градиентный бустинг - это градиентный спуск в пространстве прогнозов композиции на обучающей выборке. Основное оnличие GB от GD в том, что при обучении шага GB, мы прибавляем не сам антиградиент $s_{i}$, а модель, которая этот антиградиент $s_{i}$ апроксимирует.

**Примечание**: В градиентном бустинге мы считаем не производные самой модели, а производные функции потерь (Поэтому важна дифференцируемость функции потерь)
## 3. Регуляризация

Можно показать экспериментально, что в бустинге

![alt](https://i.imgur.com/Z3rUIem.png)

**Следствие**: В бустинге нужно мсотреть содели с низким variance => Нужно использовать неглубокие деревья

**Проблемы**:
1. Если базовые модели простые, качество будет так себе, потому что они могут испортить композицию
2. Если базовые модели сложные, есть риск моментально переобучится

**Решение** => Недоверять базовым решениям $b_{N}(x)$ (т.е. ввести learning rate)

**Сокращение шага:**
$$a_{N}(x) = a_{N-1} + \eta b_{N}(x), \eta \in (0;1] \text{ - гиперпараметр (l.r.)}$$
- Чем меньше $\eta$, тем больше будет оптимальные $N$

### Стохастический градиентный бустинг
Так как мы работаем с простыми моделями, к них низкая емкость (т.е. они могут запомнить ограниченное количество информации, а в данных сильно больше информации, чем нужно на отдельном шаге), имеет смысл обучаться не на всей выборке, а на подвыборке. 
- $|\mathbb{X}_{N}|$ (размер подвыборки) - гиперпараметр

## Функции потерь
### Для задач регрессия

#### MSE

$$\begin{array}{}
L(y, z) = (y - z)^{2} \\
s_{i} = -\frac{\partial L}{\partial z} (y_{i}, x_{i})|_{z = a_{N}(x_{i})} = y_{i} - a_{N-1}(x_{i})
\end{array}$$
#### MAE

$$\begin{array}{}
L(y, z) = |y - z| \\
s_{i} = -\frac{\partial L}{\partial z} (y_{i}, x_{i})|_{z = a_{N}(x_{i})} = -sign(y_{i} - a_{N-1}(x_{i}))
\end{array}$$

### Для задач классификации

- $a_{N}(x) \in \mathbb{R}$ - уверенность в положительном классе
- $\text{sign } a_{N}(x)$ - итоговый прогноз
$$L(y, z) = \log(1 + \exp(-yz))$$
$$\frac{1}{\ell} \sum_{i=1}^{\ell} \left( b_{N}(x_{i}) - \frac{y_{i}}{1 + \exp(y_{i}a_{N-1}(x_{i}))} \right)^{2} \to \mathop{\text{min}}_{ b_{N} } $$
![alt](https://i.imgur.com/UphLmeV.png)
> Логистическая функция потерь устойчива к выбросам

## Градиентный бустинг над деревьями (GBDT)
 
Вспомним аналитическую функцию для деревьев:
$$b_{n}(x) = \sum_{j=1}^{J_{n}} b_{nj}[x \in R_{nj}] $$
$a_{N}(x) = a_{N-1}(x) + \sum_{j=1}^{J_{n}} b_{Nj}[x\in R_{Nj}]$

**Идея**: подберем прогнозы $b_{nj}$, чтобы они были оптимальны с точки зрения исходной функции потерь.

- Обучаем дерево как обычно, а затем переподбираем предсказания:

$$\sum_{i=1}^{\ell} L\left( y_{i}, a_{N-1}(x_{i}) \sum_{j=1}^{N} c_{j} [x_{i} \in R_{Nj}]  \right) \to \mathop{\text{min}}_{ c_{1}\dots c_{J_{N}} } $$
- Упростим:
$$\sum_{j=1}^{J_{N}} \sum_{(x_{i}, y_{i}) \in R_{Nj}} L\left( y_{i}, a_{N-1} + c_{j} \right) \to \mathop{\text{min}}_{ c_{1}\dots c_{J_{N}} } $$
$\implies$Задача распадается на $J_{N}$ подзадач

$$\sum_{(x_{i}, y_{i}) \in R_{Nj}} L\left( y_{i}, a_{N-1} + c_{j} \right) \to \mathop{\text{min}}_{ c_{j} \in \mathbb{R} }$$
$\implies$Далее решаем задачу одномерной оптимизации.

*Пример*: $L(y, z) = \log(1+\exp(-yz))$
$$\sum_{(x_{i}, y_{i}) \in R_{Nj}} \log(1+\exp(-y_{i}(a_{N-1} + c_{j}))) \to \mathop{\text{min}}_{ c_{j} \in \mathbb{R} }$$
 Сделаем для этой функции 1 шаг метода Ньютода-Рафсона из точки $c_{j}=0$:
 $$c_{j} = - \frac{\sum_{(x_{i}, y_{i} \in R_{Nj})} S_{i} }{\sum_{(x_{i}, y_{i} \in R_{Nj}} |S_{i}|(1 - |S_{i}|)}$$
 Длина шага не отменяется (Для каждого листа подбираем свой шаг)

## Разное
### Взвешивание

В функциях классификации почти всегда участвует функция потерь зависит от отступа $$ L(y, z) = \bar{L}(yz)$$
Тогда: $$ s_{i} = y_{i} \left(\left. -\frac{\partial \bar{L} }{\partial \mu} (\mu) \right|_{\mu = y_{i} a_{N-1}(x_{i})} \right) = y_{i} w_{i}$$
### Устойчивость к выбросам

- $L(y, z) = e^{-yz}$ - экспоненциальная функция потерь
- $s_{i} = y_{i} \exp(-y_{i}a_{N-1}(x_{i})) = y_{i} w_{i}$
- $y_{i} a_{N-1}(x_{i} ) \ll 0 \implies w_{i}\to + \infty \implies$ Функция потрь будет не устойчива к выбросам(нужно проерять для каждой функции потерь) 

## Бустинг второго порядка

Обучение N-ой модели:
$$\sum_{i=1}^{\ell} L(y_{i}, a_{N-1} + b_{N}(x_{i})) \to \mathop{\text{min}}_{ b_{n} } $$
Разложим функцию в ряд Тейлора по второму аргументы с центром в точке $a_{N}(x_{i})$ до второго члена:

$$ = \sum_{i=1}^{\ell} \left(L(y_{i}, a_{N-1}(x_{i})) \left. +\frac{\partial }{\partial z} L(y_{i}, z) \right|_{z = a_{N-1}(x_{i})} b_{n}(x_{i}) + \left. \frac{\partial^{2} }{\partial z^{2}} L(y_{i}, z) \right|_{z = a_{N-1}(x_{i})} \frac{b_{n}^{2}(x_{i})}{2} \right) \implies $$
$$= \sum_{i=1}^{\ell} \left( -s_{i} b_{N}(x_{i}) + \frac{1}{2} h_{i} b_{N}^{2}(x_{i}) \right) \to \mathop{\text{min}}_{ b_{N} }
$$
Полученная функция очень похожа на выражение, полученное при минимизации MSE. Также стоит замеьтить, что в такой модели нам получилось добавить информацию о кривизне функции потерь ($h_{i}$).

## Еще несколько способов регуляризовать бустинг

$$b(x) = \sum_{j=1}^{J_{n}} b_{j} [x \in R_{j}]$$
Показатели сложности дерева:
1. Количество листьев($J_{n}$)
2. $||b|| = \sum_{j=1}^{J} b_{j}^{2}$ - чем больше, тем выше шанс регуляризации.
3. ...
С учетом предположений, получим функционал вида:
$$ \sum_{i=1}^{\ell} \left( -s_{i} b_{N}(x_{i}) + \frac{1}{2} h_{i} b_{N}^{2}(x_{i}) \right) + \gamma J + \frac{\lambda}{2} \sum_{j=1}^{J} b_{j}^{2} \to \mathop{\text{min}}_{ b_{N}(x) }   $$
Попробуем его минимизировать

Пусть структура дерева уже фиксирована. Чем равны прогнозы в листьях?

$$ = \sum_{j=1}^{J} \sum_{(x_{i}, y_{i}) \in \mathbb{R}} \left( -s_{j} b_{j} + \frac{1}{2} h_{j}b_{j}^{2}\right) + \gamma \sum_{j=1}^{J} 1 + \frac{\lambda}{2} \sum_{j=1}^{J} b_{j}^{2} =$$
$$=\sum_{j=1}^{J} \left(  b_{j} \sum_{(x_{i}, y_{j}) \in \mathbb{R}} s_{i}  + \frac{1}{2} b_{j}^{2} \sum_{(x_{i}, y_{i} \in \mathbb{R})} h_{i} + \gamma + \frac{\lambda^{2}}{2} b_{j}^{2} \right)$$
Обозначим суммы $S_{j}$ и $H_{j}$:
$$\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        H_{j} = \sum_{(x_{i}, y_{i} \in \mathbb{R})} h_{i}\\  
        S_{j} = \sum_{(x_{i}, y_{i} \in \mathbb{R})} s_{i}
    \end{array}  
    \right. 
\end{equation} \implies $$
$$\implies \sum_{j=1}^{J} \left( -S_{j} b_{j} + \frac{1}{2} (H_{j} + \lambda) b_{j}^{2} + \gamma \right) $$
Тут в сковках только один параметр: $b_{j}$. Тогда, чтобы найти оптимальное разбиение для дерева, нам нужно решить простое квадратное уравнение:
$$b_{j}^{*} = \frac{S_{j}}{H_{j} + \lambda}$$
Подставим:
$$= -\frac{1}{2}\sum_{j=1}^{J} \frac{S_{j}^{2}}{H_{j} + \lambda} + \gamma J = H(B)$$
Получим $H(B)$ - функционал ошибки для дерева, при оптимальных прогнозах в листьях.
![](https://i.imgur.com/mXvyeM5.png)

Будем использовать $H(B)$ как impurity!
$$Q(R, j, t) = H(R) - H(R_{l}) - H(R_{r})$$
Что мы сделали:
1. Учли вторые производные функции потерь $L(y, z)$
2. Добавили регуляризаторы
3. Ввели impurity, напрямую щависящую от функции потерь
=> XGBoost
