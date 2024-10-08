---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Логистическая регрессия

**Задача**: Хотим оценивать вероятности классов (уверенность модули в своем ответе)
$$a(x) = +1 (\text{модель уверена на 80\%})$$
**Интерпретация**: a(x) = +1 с уверенностью 80% $\Longleftrightarrow$ Если взять все объекты с уверенностью 80%, среди них реально будет 80%

**Зачем это?**

1. **Пример**: $b(x)$ - вероятность, что клиент уйдет. Если $b(x) \geq 90\%$, звоним клиенту(это означает, что среди них не более 10% не уходит) $\to$ Можно выбирать порог более гибко.
2. **Пример**: Баннерная реклама
    ![alt](https://i.imgur.com/RbxV7z7.png)
**Формализация**:
    $b(x)$ - выдает вероятность положительного класса
    $b(x) = \sigma \langle w, x \rangle \in (0;1)$, где $\sigma(z) = \frac{1}{1 + e^{-z}}$

>[!note] Корректность оценки
> $b(x)$ корректно оценкивает вероятности, если для вероятности p среди всех объектов $x\in \mathbb{X}$ с $b(x)=p$ доля положительных равна p.

Пусть выборка состоит из одинаковых объектов:
$$ x_{1} = x_{2} = \dots = x_{n} \implies  b(x_{1}) = b(x_{2}) = \dots = b(x_{n})$$
Но при этом иожет быть ситуация, когда $y_{1} \neq \dots \neq y_{n}$. В этом случае было бы логично выдавать долю положительных объектов.

$$\mathop{\text{argmin}}_{b \in [0;1]} \frac{1}{n} \sum_{i=1}^{n} L(y_{i}, b) = \frac{1}{n} \sum_{i=1}^{n} [y_{i}=+1]$$
В этом случае функция потерь требует корректного оценивания вероятностей. Попробуем записать более формально:
При $n\to \infty$:
$$\frac{1}{n} \sum_{i=1}^{n} L(y_{i}, b) = \frac{1}{n}\sum_{i=1}^{n} [y=+1]L(+1, b) + {1}{n}\sum_{i=1}^{n} [y=-1]L(-1, b)=$$
$$L(+1, b) \frac{\sum_{i=1}^{n} [y_{i}=+1]}{n} + L(-1, b) \frac{\sum_{i=1}^{n} [y_{i}=-1]}{n}$$

Получим, что $\frac{\sum_{i=1}^{n} [y_{i}=+1]}{n} \to P(y=+1 \mid x)$ , $\frac{\sum_{i=1}^{n} [y_{i}=-1]}{n} \to P(y=-1 \mid x)$ , а значит выражение выше стремится к мат. ожиданию:
>[!tip] \*Требование к функции потерь (!)
$$\mathop{\text{argmin}}_{ b \in [0;1] }(\mathbb{E}_{y} (L(y, b) \mid x)) = p(y=+1 \mid x) \forall x \in \mathbb{X}$$

- **Почему это работает?** Чтобы модели не переобучались, мы ограничиваем их сложность. Следовательно, если объекты $x_{1}, x_{2}$ близки, то и предсказания на них будут близки. Поэтому будут возникать группы объектов, на которых предсказания почти равны.
- **Задача:** Мы хотим, чтобы $b(x)$ стремилась выдать на них коректную вероятность.

### Для каких функций потерь выполняется требование\*?

#### MSE

$$L(y, z) = (y - z)^{2} \text{ или } L(y, z) = (b - [y = +1])^{2}$$
$$E_{y} (b - [y=+1]) = p(y=+1\mid x) (b-1)^{2} + (1 - p(y=+1\mid x)) (b-0)^{2} $$
Находим аргминмимум:
$$\frac{\partial}{\partial b} = 2p(b-1)+2b(1-p)=0 \implies b = p(y=+1\mid x)$$
MSE можно оценивать веротности классов, однако это так себе идея:
![alt](https://i.imgur.com/uC3EuuQ.png)

Проблема воникает из-за горизонтальных ассимптот (Если будем брать в этих точках производную, производная бкднт маленькая, хотя ошибка будет огромная)

#### MAE

Для него требование(!) не выполняется

#### Придумаем что-то свое

Попробуем придумать **свою функцию распределения** из вероятностных соображений.

- Каждый ответ $y_{i}$ - некоторая случайная величина из распределения Бернулли
- $b(x_{i})$ - оценка вероятности $p(y_{i} = +1 \mid x)$

Запишем функию максимального правдоподобия:
$$L = \prod_{i=1}^{\ell} b(x_i)^{[y_{i} = +1]} (1 - b(x_i))^{[y_{i} = -1]} \to \mathop{\text{max}}_{ \ell } $$
Логорифмируем ее и берем с минусом(мы минимизиркем ошибку):
$$\text{log-loss} = \sum_{i=1}^{\ell} ( -[y_{i}=+1] \log b(x_{i}) - [y_{i} = -1] \log (1 - b(x_{i}))) \to \mathop{\text{min}}_{ \ell } $$
Эту функцию потерь назвают log-loss или binary cross entropy loss. Для нее требование\* будет выполняться.

Запишем нашу функцию потерь для сигмоиды: $b(x) = \sigma(\langle w, x \rangle)$
$$\frac{1}{\ell} \sum_{i=1}^{\ell}  ( -[y_{i}=+1] \log b(x_{i}) - [y_{i} = -1] \log (1 - b(x_{i}))) = \dots $$
$$= \frac{1}{\ell} \sum_{i=1}^{\ell} \log(1 + \exp(-y_{i}<w, x_{i}>)) \to \mathop{\text{min}}_{ w } $$
$$\implies L(y, z) = \log(1 + e^{-yz}) $$

Получили логистическую функцию потерь.
