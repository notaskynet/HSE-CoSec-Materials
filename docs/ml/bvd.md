---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Bias-Variance decomposition (BVD)

$\mathbb{X} = (x_{i}, y_{i})_{i=1}^{\ell}, y \in \mathbb{R}$ (Для классификации вывод сложнее, но он также работает)
Допустим, что мы заранее знаем распределение данных на $\mathbb{X}  \times \mathbb{Y}$.
$$L(y, a) = (y - a)^2$$
Среднеквадратичный риск:
$$R(a) = \mathbb{E}_{x, y} (y - a(x))^{2} = \int_{\mathbb{X}}\int_{\mathbb{Y}} p(x, y) dx dy$$
Можно показать, что оптимальную модель можно выписать:
$$a_{*} (x) = \int_{\mathbb{Y}} y \cdot p(y | x)dy$$
Метод обучения:
$$\mu: (\mathbb{X} \times \mathbb{Y})^{\ell} \to A (\text{семейство моделей})$$
- $L(\mu) = \mathbb{E}_{x}\mathbb{E}_{x, y} (y - \mu(\mathbb{X})(x))^{2}$ - ошибка метода обучения, где  $\mu(\mathbb{X})$ - модель, а $\mu(\mathbb{X})(x)$ - предсказание

$L(\mu)$ - можно расписать через несколько слагаемых:
- $\mathbb{E}_{x, y} (y - \mathbb{E}(y(x)))^{2}$ - **шум** (noise) - ошибка лучшей модели($\mathbb{E}(y(x))$ - лучшая модель), или нижняя граница на ошибку модели. Примечание: $\mathbb{E}_{x, y}$ - усреднение по всем тестовым объектам
- $E_{x}\left(E_{\mathbb{X}} \mu(\mathbb{X}) - \mathbb{E}(y|X)\right)^{2}$ - **смещение** (bias) - показывает отклонение лучшей модели от средней($\mathbb{E}_{\mathbb{X}} \mu(\mathbb{X})$ - средняя модель), т.е. насколько семейство моделей подходит для решения задачи
- $\mathbb{E}_{x}\mathbb{E}_{\mathbb{X}}(\mu(\mathbb{X})-\mathbb{E}_{\mathbb{X}}\mu(\mathbb{X}))^{2}$ - **разброс**(variance) - показывает, насколько устойчим метод обучения - чем выше, тем больше переобученность

То есть получим вот такую смешную формулу для ошибки метода обучения:
>[!info] Bias-Variance Decomposition
$$L(\mu) = \mathbb{E}_{x, y} (y - \mathbb{E}(y(x)))^{2} + E_{x}\left(E_{\mathbb{X}} \mu(\mathbb{X}) - \mathbb{E}(y|X)\right)^{2} + \mathbb{E}_{x}\mathbb{E}_{\mathbb{X}}(\mu(\mathbb{X})-\mathbb{E}_{\mathbb{X}}\mu(\mathbb{X}))^{2}$$

Рассмотрим примеры:
- Для линейных моделей:
    ![alt](https://i.imgur.com/m6PKPKi.png)
- Для деревьев:
    1. Глубина - 2
        ![alt](https://i.imgur.com/ccI3oDA.png)
    2. Глубина - 5
        ![alt](https://i.imgur.com/BfPgJBg.png)
    3. Глубина - 8
        ![alt](https://i.imgur.com/9FjfVxD.png)
    4. Строим деревья глубины 8 $b_{1}(x) \dots b_{N}(x)$, а затем усредняем;
        ![alt](https://i.imgur.com/CtQzsWK.png)
    **Предположение**: При усреднении bias такой же, а varience уменьшается
