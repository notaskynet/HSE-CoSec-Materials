---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Бутстрап (Bootstrap)

**Идея**: Генерация подвыборки из $\mathbb{X}$ выбором $\ell$ объектов с возвращением

$$\mathbb{X} \to \mathbb{X}_{1} \dots \mathbb{X}_{N} $$
- y(x) - истинный ответ
- p(x) - плотнрость на $\mathbb{X}$
- $\varepsilon_{j} (x) = b_{j}(x) - y(x)$ - ошибка на j-том объекте
- $\mathbb{E}_{x} (b_{j}(x) - y(x))^{2} = \mathbb{E}_{x} \varepsilon_{j}^{2} = E_{1}$ - аналог MSE, но для всей выборки

Предположим:
1. $\mathbb{E} \varepsilon_{j}(x) = 0$ - несмещенная оценка (нет систематической ошибки)
2. $\mathbb{E} \varepsilon_{i}(x) \mathbb{E} \varepsilon_{j}(x)= 0, i\neq j$ - ошибки на разных моделях не  скорелированы (вообще это не правда, потому что мы обучаем модели на почти одних и тех же данных)

Построим модель:
$$a(x) = \frac{1}{N} \sum_{i=1}^{N} b_{j} (x)$$
Посмотрим на ошибку:
$$\mathbb{E} (\sum_{i=1}^{N} b_{j} (x) - y(x))^{2} = \mathbb{E} (\sum_{i=1}^{N} b_{i} (x) - \sum_{i=1}^{N} y_{i}(x))^{2} = $$
$$ = \mathbb{E} \left( \frac{1}{N}\sum_{i=1}^{N} \varepsilon_{j}(x) \right)^{2} = \frac{1}{N^{2}} \mathbb{E} \left( \sum_{i=1}^{N} \varepsilon_{i}^{2}(x) + \sum_{i\neq j} \varepsilon_{i}(x) \varepsilon_{j}(x)\right) = $$
$$ \frac{1}{N^{2}} \sum_{i=1}^{N} \mathbb{E} \varepsilon_{i}(x) = \frac{1}{N^{2}} N E_{1} = \frac{E_{1}}{N} $$
То есть просто усреднив модели, мы получили ошибку в N раз меньше! Но так как второе предположение не корректно, утверждение не совсем верное :(
