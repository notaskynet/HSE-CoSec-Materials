---
prev:
  text: 'Регулярные модели. Неравенство Рао-Крамера. Эффективные оценки'
  link: '/statistics/06_regular_models'
next:
  text: 'Условное математическое ожидание'
  link: '/statistics/08_cond_math_expectation'
outline: deep
---

# Экспоненциальные семейства

**Определение**. Семейство распределений $\{f(x, \theta)\}$ называется экспоненциальным, если плотность вероятности (вероятность в точке)
$$ f(x, \theta) = \exp(A(\theta) \cdot B(x) + C(\theta) + D(x))$$
$$ L(x, \theta) = \prod_{i=1}^{n} f(x_{i}, \theta) = \exp\left(A(\theta) \sum_{i=1}^{n} B(x_{i}) + n \cdot C(\theta) + \sum_{i=1}^{n} D(x_{i})\right)$$
$$\ln L = A(\theta) \sum_{i=1}^{n} B(x_{i}) + n \cdot C(\theta) $$
$$
\begin{array}{lcl}
\frac{\partial}{\partial \theta} \ln L &=& A'(\theta) \sum_{i=1}^{n} B(x_{i}) + n \cdot C'(\theta) + \sum_{i=1}^{n} D(x_{i}) =  \\
&=& A'(\theta) \left( \sum_{i=1}^{n} B(x_{i}) + n \cdot \frac{C'(\theta)}{A'(\theta)}\right) = \\
&=& A'(\theta) \left( T(\theta) - n \cdot \tau(\theta) \right)
\end{array}
$$
Рассмотрим примеры.

### 1. Распределение Паретто

$$ f(x) = \theta x^{-(\theta + 1)} = e^{\ln \theta - (\theta + 1) \ln x}, x \geq 1$$

$$
\begin{array}{lcl}
B(x) = \ln x \\
A(\theta) = - (\theta + 1) \\
C(\theta) = \ln (\theta)
\end{array} \implies
\begin{array}{lcl}
T(\vec{x}) = \frac{1}{n} \sum_{i=1}^{n} \ln (x) \\
\tau(\theta) = -\frac{1}{\theta} \cdot (-1) = \frac{1}{\theta}
\end{array}
$$

Вычислим информацию по Фишеру:

$$ i_{n}(\theta) = DV(\vec{x}, \theta) = -\mathbb{E} \frac{\partial^{2}}{\partial \theta^{2}} \ln L$$

$$
\begin{array}{lcl}
\frac{\partial}{\partial \theta} L &=& A'(\theta) \sum_{i=1}^{n} B(x_{i}) + n C'(\theta)  \\
 \frac{\partial^{2}}{\partial \theta^{2}} L &=& A''(\theta) \sum_{i=1}^{n} B(x_{i}) + n C''(\theta) 
\end{array}
$$

Получим:

$$ \begin{array}{lcl}
i_{n}(\theta) &=& -\mathbb{E}\left( A''(\theta) \sum_{i=1}^{n} B(x_{i}) + n C''(\theta) \right) = \\
&=& -A''(\theta) \mathbb{E} \sum_{i=1}^{n} B(x_{i}) - n C''(\theta) =  \\
&=& \left[ E\sum_{i=1}^{n} B(x_{i}) = -n \frac{C'(\theta)}{A'(\theta)} \right] = \\
&=& n A'(\theta) \left( -\frac{C'(\theta)}{A'(\theta)} \right)'
\end{array}
$$

>[!info] Сокращенная запись для информации Фишера
>$$ i_{n}(\theta) = n A'(\theta) \left( -\frac{C'(\theta)}{A'(\theta)}\right)'$$

Тогда
$$D\frac{1}{n}\sum_{i=1}^{n} B(x_{i}) = \frac{[\tau'(\theta)^{2}]}{nA'(\theta)\tau'(\theta)} = \frac{\tau'(\theta)}{nA'(\theta)} = \frac{A''(\tau)C'(\tau) - C''(\tau)A'(\tau)}{n(A'()\tau)^{3}}$$
Проверим полученные формулы на другом распределений.

### 2. Распределение Бернулли

$$ \xi \sim Bi(1, \theta)$$
$$f(x, \theta) = \theta^{x}(1 - \theta)^{(1-x)}$$
$$
\begin{array}{lcl}
\ln L &=& x \ln(\theta) + (1 - x) \ln(1 - \theta) = \\
&=& x(\ln \theta - \ln (1 - \theta)) + \ln(1 - \theta) = \\
&=& \left\{
\begin{array}{lcl}
A(\theta) = \ln \theta - \ln (1 - \theta) \\
C(\theta) = \ln(1 - \theta) \\
\end{array} \\
\right\} = \\
&=& x \cdot A(\theta) + C(\theta)
\end{array}
$$
$$i_{n}(\theta) = n \cdot A'(\theta) \left( - \frac{C'(\theta)}{A'(\theta)}\right)'$$
Проверим:
$$ \tau(\theta) = - \frac{-1}{1 - \theta} = \frac{1}{1 - \theta}$$
$$ A''(\theta) = -\frac{1}{\theta^{2}} + \frac{1}{(1 - \theta)^{2}} = \frac{-(1 - \theta)^{2} + \theta^{2}}{\theta^{2} (1 - \theta^{2})} = \frac{2 \theta - 1}{\theta^{2}(1 - \theta)^{2}}$$
$$ C''(\theta) = \frac{1}{(1 - \theta)^{2}} $$
$$
\begin{array}{lcl}
i_{n}(\theta) &=& \frac{1}{n} \left[ \frac{2 \theta - 1}{\theta^{2} (1 - \theta)^{2}} \left( -\frac{1}{1 - \theta} \right) + \frac{1}{(1-\theta^{2})} \cdot \frac{1}{\theta(1 - \theta)}\right]' (\theta) (1 - \theta)^{3} =  \\
&=& \frac{1}{n} \left[ - \frac{2 \theta - 1}{\theta^{2}} + \frac{1}{\theta}\right] \theta^{3} = [\theta^{2} - 2 \theta^{2} + \theta] = \frac{\theta - \theta^{2}}{n} =  \\
&=&\frac{\theta(1-\theta)}{n}
\end{array}
$$

### Вывод

Таким образом для построения оптимальных и эффективных оценок мы декомпозируем плотность вероятности и из нее выводим оценку. Основная проблема в том, что мы зажаты в маленьком классе оценок. Есть некоторые обощения этого метода.

>[!info] Условие построение оптимальной оценки
>$$ V(\vec{x}, \theta) = \alpha(\theta) \frac{L'(\vec{x}, \theta)}{L(\vec{x}, \theta) = T(\vec{x}) - \tau(\theta)}$$
>Если $\sum_{i=1}^{n} \alpha_{i} (\theta) \frac{\partial^{i}}{\partial \theta^{i}} L \frac{1}{L} = T(x)- \tau(\theta)$, мы можем построить оптимальную (но не эффективную) оценку.
