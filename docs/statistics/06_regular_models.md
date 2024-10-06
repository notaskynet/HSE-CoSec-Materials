---
prev:
  text: 'Порядковая статистика'
  link: '/statistics/05_ordinal_statistics'
next:
  text: 'Экспоненциальные семейства'
  link: '/statistics/07_exp_functions'
outline: deep
---

# Регулярные модели. Неравенство Рао-Крамера. Эффективные оценки

**Определение 1.** Семейство распределений $\{ F(t, \theta), f(t, \theta) \mid \theta \in \Theta \}$ назовем *регулярными*, если для любой статистики $T(\bar{X})$ выполняется:
$$ \frac{\partial}{ \partial \theta} \int_{\mathbb{R}} T(x) f(x, \theta) dx = \int_{\mathbb{R}} T(x) \frac{\partial}{\partial \theta} f(x, \theta) dx$$

**Определение 2.** Функцией максимального правдободобия для выборки $\bar{X}=X_{1}\dots X_{n}$ и параметрического семейста $\{ F(t, \theta), f(t, \theta) \mid \theta \in \Theta\}$
$$ L(\bar{X}, \theta)=\prod_{i=1}^n f(x_{i}, \theta)$$
$$ \frac{\partial}{ \partial \theta} \int_{\mathbb{R}^n} T(x) L(\bar{X}, \theta) dx = \int_{\mathbb{R}^n} T(x) \frac{\partial}{\partial \theta} L(\bar{X}, \theta) dx$$

**Утверждение**. Достаточными условиями для того, чтобы распределение было регулярным, являются:

1. $\sqrt{ f(t, \theta) }$ непрерывна и дифференцируема по $\theta$;
2. $\int_{\mathbb{R}^n} \frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta} L(\bar{x}, \theta)d \bar{x}$ - непрерывна по $\theta$ (для мажорирующего распределения);

*Можно ли что-то сказать об оценках?*
- Запишем условие нормировки:
$$\int_{\mathbb{R}^n} L(\bar{x}, \theta) d \bar{x}=1$$
- Из равенства выше следует, что:
$$ \frac{\partial}{\partial \theta}\int_{\mathbb{R}^n} L(\bar{x}, \theta) d \bar{x}=0 $$
- Из регулярности распределения также следует, что
$$\frac{\partial}{\partial \theta} \int_{\mathbb{R}^n} L(\bar{(x), \theta}) d \bar{x}= \int_{\mathbb{R}^n} \frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta} L(\bar{x}, \theta)d \bar{x} = E \frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta} $$
- Следовательно, получим
$$E \frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta} = 0$$
- **Пояснение.**  Внимательно рассмотрим аргументы под интегралом. Можно заметить что $\frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta}$ - это значения, которые принимает ФМП, а $L(\bar{x}, \theta)$ - вероятность появления значений. Из этого следует, что интегральное выраение - это математическое ожидание указанных величин.

**Определение 3.** $\frac{\partial \ln L(\bar{x}, \theta)}{\partial \theta}=V(\bar{X}, \theta)$ - вклад выборки.

**Определение 4.** $D V(\bar{X}, \theta)=E(V(\bar{x}, \theta))^2 = i_{n}(\theta)$ - информация по Фишеру в n наблюдениях

### Теорема Рао-Крамера

В регулярных семействах распределений $\{f(t, \theta)\}$, для любой несмещенной оценки $T(\bar{x})$ параметрической функции $\tau(\theta)$,  верно:

$$ D T(\bar{x}) \geq \frac{[\tau'(\theta)]^2}{i_{n}(\theta)}$$

**Доказательство.**
$$\tau(\theta) = \int_{R^{n}} T(\vec{x}) L(\vec{x}, \theta) d \vec{x}$$
Так как $\tau(\theta)$ регулярное, можем записать:
$$
\tau'(\theta) = \int_{R^{n}} T(\vec{x}) \frac{\partial}{\partial \theta} L(\vec{x}, \theta) d \vec{x} = \int_{R^{n}} T(\vec{x}) \left( \frac{\partial L}{\partial \theta} \cdot\frac{1}{L} \right)\cdot  L(\vec{x}, \theta) d \vec{x} =
$$
$$
 = \int_{R^{n}} T(\vec{x}) (\ln L)'_{\theta}\cdot  L(\vec{x}, \theta) d \vec{x} = \int_{R^{n}} T(\vec{x}) V(\vec{x}, \theta)  L(\vec{x}, \theta) d \vec{x} = 
$$
$$ = \mathbb{E} T(\vec{x}) V(\vec{x}, \theta) = \text{cov} (T(\vec{x}), V(\vec{x}, \theta))$$

>[!info] Будем учитывать, что
>$$ \text{cov} (\xi, \eta) = \mathbb{E}\left( (\xi - \mathbb{E}\xi)(\eta - \mathbb{E}\eta)\right) = \mathbb{E}(\xi \eta - \eta \mathbb{E}\xi) $$
>$$E\xi + E\eta - \mathbb{E}\xi \cdot \mathbb{E} \eta = \mathbb{E} \xi \eta$$

Получим:
$$ \tau'(\theta) = |\text{cov} (T(\vec{x}, V(\vec{x}, \theta)))| \leq \sqrt{ \mathbb{D} T(x) \cdot \mathbb{D} V(\vec{x}, \theta) }$$
$$ \mathbb{D} T(\vec{x}) \geq \frac{[\tau'(\theta)]^{2}}{\mathbb{D} V(\vec{x}, \theta)} = \frac{[\tau'(\theta)]^{2}}{i_{n}(\theta)}$$

**Определение**. Пусть $T(x)$ - несмещенная оценка $\tau(\theta)$. Будем называть ее эф-
фективной оценкой $\tau(\theta)$, если для нее достигается равенство в неравенстве Рао-Крамера, то есть для всех $\tau \in \Theta$.

### Критерий эффективности (Теорема)

Для того, чтобы в регулярных семействах $\{ f(x, \theta) \mid \theta \in \Theta \}$ несмещенная оценка $T(\vec{x})$ для параметрической функции $\tau(\theta)$ достигала нижней границе в неравенстве Рао-Крамера, необходимо и достаточно
$$ V(\vec{x}, \theta) = a(\theta) T(\vec{x}) + b(\theta) = a(\theta) [T(\vec{x}) - \tau'(\theta)]$$
**Доказательство**. [см. тут(страница 48)](https://teach-in.ru/file/synopsis/pdf/math-statistics-lectures-shabanov-M2.pdf)
