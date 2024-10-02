---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Бэггинг (Bagging)

- $\mu(X)$ - метод обучения
- $\bar{X}$ - случайная подвыборка (генерируется бутстрапом)

$$
\begin{array}{}
b_{1}(x) = \mu(\mathbb{\bar{X}}_{1}) \\
\vdots \\
b_{n}(x) = \mu(\mathbb{\bar{X}}_{n})
\end{array}
$$

где $b_1(x) \dots b_{n}(x)$ называются базовые модели

$$a_{n}(x) = \frac{1}{n}\sum_{i=1}^{n} b_{i}(x) = \frac{1}{n} \sum_{i=1}^{n} \mu(\mathbb{\bar{X}}_{i})$$Тут $a_{n}(x)$ - композиция моделей (бэггинг)
<p style="text-align:center;">bagging = <b>b</b>ootstrap <b>agg</b>rigat<b>ing</b></p>
-**Примечание 1**: Работает для всех способов генерации выорок, где участвует случайность.

**Примечание 2**:$\text{bias}(a_{N})) = \text{bias}(b_{n}) \implies$ Если базовые модели убучения слабые, то и $a_{N}$ тоже будет слабый $\implies$ Нужно использовать модели посильнее.

**Примечание 3**: Вместо $a_{N}$ должен быть метод обучения.
$$\text{var}(a_{N}) = \frac{1}{N} \text{var}(b_{N}) + \frac{N(N-1)}{N^{2}} \text{cov}(b_{n}(x), b_{m}(x))$$
тут $\text{cov}$:
$$\mathbb{E}_{x, y}\mathbb{E}_{\mathbb{X}}\text{ cov}(\mu(\bar{\mathbb{X}}_{m} - \mathbb{E}\mu(\bar{\mathbb{X}}_{m}))\cdot(\mu(\bar{\mathbb{X}}_{n} - \mathbb{E}\mu(\bar{\mathbb{X}}_{n}))$$
**Вывод**: Чем менее скорелированы базовые модели, тем лучше $\implies$ Можем придумать супер метод: [случайный лес](random_forest.md)
