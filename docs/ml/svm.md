---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Метод опорных векторов (Support Vector Machine)

**Функция потерь** (называется кусочно линейной функцией потерь):
$$\bar{L}(\mu) = \mathop{\text{max}} (0, 1 - \mu)$$
**Идея**:
![alt](https://i.imgur.com/rJ5EGvm.png)
> Рассмотрим разные случаи.

### Линейно-разделимый случай:
$$\exists w\colon y_{i} \langle w, x_{i} \rangle > 0 \qquad \forall i=1\dots \ell$$
$a(x)=sign(\langle w, x_{i} \rangle + w_{0} )$
Домножим на $\alpha$:
$a(x)=sign( \alpha \langle w, x_{i} \rangle + \alpha w_{0} ) \implies$ Предсказание не меняется при домножении на костанту

Введем важное требование:
>[!note] Важное требование(1)
$$\mathop{\text{min}}_{ x_{i} \in \mathbb{X}} |\langle w, x_{i} \rangle + w_{0}| = 1$$
Этого требования можно добиться масштабированием параметров, поэтому далее будем считать, что это требование выполнено. (Зачем оно нужно, будет объяснено ниже)

Расстояние от точки $x$ до прямой с вектором нормали $w$:
$$ \rho(a, x) = \frac{|\langle w, x \rangle + w_{0} |}{||w||}$$
Посчитаем расстояние от разделяющей гипреплоскости $a$ до ближайшего объекта $x_i$:
$$ \mathop{\text{min}}_{ x_{i} \in \mathbb{X} } \rho(a, x) = \mathop{\text{min}}_{ x_{i} \in \mathbb{X} } \frac{|\langle w, x \rangle + w_{0} |}{||w||} = \frac{1}{||w||} \mathop{\text{min}}_{ x_{i} \in \mathbb{X} } |\langle w, x_{i} \rangle+ w_{0}|$$
Из требования (1) мы знаем, что $\mathop{\text{min}}_{ x_{i} \in \mathbb{X} } |\langle w, x_{i} \rangle+ w_{0}| = 1$, а значит выражение выше будет выглядеть так:
$$\mathop{\text{min}}_{ x_{i} \in \mathbb{X} } \rho(a, x) = \frac{1}{||w||}$$
Тогда получим требование: $\frac{1}{||w||} \to \mathop{\text{max}}_{ w }$. Однако в таком случае проще всего вщять прямую на бесконечности, поэтому не обходимо ввести доп. требования:
- $y_{i} (\langle w, x_{i} \rangle+w_{0}) >0  \qquad \forall i \in 1\dots \ell$ - модель не ошибкается на объектах.
- $\mathop{\text{min}}_{ x_{i} \in \mathbb{X} } |\langle w, x_{i} \rangle + w_{0}| = 1$
Запишем наши требования в систему:
$$
\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        \frac{1}{||w||} \to \mathop{\text{max}}_{ w, w_{0} }\\  
        y_{i} (\langle w, x_{i} \rangle+w_{0}) >0  \qquad \forall i \in 1\dots \ell \\
        \mathop{\text{min}}_{ x_{i} \in \mathbb{X} } |\langle w, x_{i} \rangle + w_{0}| = 1
    \end{array}  
    \right.
\end{equation}
$$
Упростим выражение:
$$
\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        ||w||^{2} \to \mathop{\text{min}}_{ w, w_{0} }\\  
        y_{i} (\langle w, x_{i} \rangle+w_{0}) \geq 1  \qquad \forall i \in 1\dots \ell \\
    \end{array}  
    \right.
\end{equation}
$$
Примечание: В выражении 2 стоит нестрогое равенство, так как при строгом равентстве, выражение можно будет еще сильнее уменьшить, что противоречит условию.

В результате получим условия задачи SVM в линейно-разделимом случае.

### Линейно-неразделимый случай

$$\forall w, w_{0} \exists x_{i} \in \mathbb{X} \colon y_{i} \langle w, x_{i} \rangle < 0$$
Можно заметить, что требования для линейно-разделимого случае всегда не выполняются! Чтобы заданные требования работали, ослабим требования:
$$
\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        \frac{1}{2}||w||^{2} + C \sum_{i=1}^{n} \xi_{i} \to \mathop{\text{min}}_{ w, w_{0}, \xi_{i} }\\  
        y_{i} (\langle w, x_{i} \rangle+w_{0}) \geq 1 - \xi_{i} \\
\xi_{i} \geq 0
    \end{array}  
    \right.
\end{equation}
$$
где:
- $\xi_{i}$ - это гиперпараметр
- $||w||^{2}$ - штраф за веса
- $C$ - штраф за маленькие расстояния о ближайшего объекта
- $\sum_{i=1}^{n}\xi_{i}$ - траф за ошибки модели

Таким образом мы получим **условия SVM для общего случая**. (2)

![](https://i.imgur.com/yIIiq3B.png)
> Тут $C$ - гиперпараметр, который нам нужно оптимизировать.

**Примечание**: Берем L1, а не L2 норму, потому что хотим, чтобы вектор $\xi_{i}$ был *разреженным* (нам выгоднее делать больше правильных предсказаний, но с наличием некоторых ошибок, чем ценой большего количества ошибок делать суммарно меньшую ошибку)

Попробуем упростить требования (2). Запишем условия для $\xi_{i}$
$$
\begin{equation}
    \left\{  
    \begin{array}{lcl}  
        \xi_{i} \geq 1 - y_{i} (\langle w, x_{i} \rangle+w_{0})\\
\xi_{i} \geq 0
    \end{array}  
    \right.
\end{equation} \implies \xi_{i} = \mathop{\text{max}} (0, 1 - y_{i} ( \langle w, x \rangle + w_{0}))
$$
Подставь в требования (2.1) получившиеся требования:
$$\frac{1}{2}||w||^{2} + C \sum_{i=1}^{\ell} \mathop{\text{max}} (0, 1 - y_{i} ( \langle w, x \rangle + w_{0})) \to \mathop{\text{min}}_{ w, w_{0} }$$
Получим безусловную задачу оптимизации. Также можно заметить, что наша функция потерь выглядит очень похоже на то, что мы видели ранее:
- $\mathop{\text{max}} (0, 1 - y_{i} ( \langle w, x \rangle + w_{0})) = \mathop{\text{max}} (0, 1-\mu)$: некоторая функция потерь.
- $\sum_{i=1}^{\ell} \mathop{\text{max}} (0, 1 - y_{i} ( \langle w, x \rangle + w_{0}))$: функционал ошибки
- $\frac{1}{2}||w||^{2}$: регуляризация 

**Важное замечание:** На регуляризацию можно смотреть как на треобование о том, чтобы модель делала минимум предположений о данных.
## Отличия LR и SVM
![alt](https://i.imgur.com/6KkFE9J.png)

- При $\mu < 0$ поведение одинаковое
- При $\mu > 0$:
    - Для SVM домтаточное, чтобы $\mu \geq 1$
    - LR хочет, чтобы $\mu \to +\infty$

**Вывод**: SVM приводит к лучшему значению метрик качества классификаии. LR будет стремится предсказывать вероятности.