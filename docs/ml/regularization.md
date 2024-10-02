---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Регуляризация

>[!info] Важно
>Для того, чтобы регуляризация работала нормально, обязательно нужно нормализовать данные.

## L1-регуляризация

**Особенности**: Способна занулять веса

Зачем занулять часть весов?
 - Мы накидали ы признаки, все что есть
 - Нужно быстродейсвтие модели
 - $\ell \ll d$

Почему?

- **Объясненение 1:**

$$
Q(w) + \lambda ||w|| \to min_{w} \implies \begin{equation}
    \left\{  
    \begin{array}{}  
        Q(w) \to min_{w}\\  
        ||w|| \leq const\\  
    \end{array}  
    \right.
\end{equation}
$$
![alt](https://i.imgur.com/oGXLfPf.png)

> Больше вероятность, того, что признак занулиться

- **Объясненение 2:**
    $$ w = (1, \varepsilon), 0 < \delta < \varepsilon$$
    Оптимизируем по маленькой величине:
    $$ || w - (\delta, 0)||^2 = 1 - 2 \delta + \delta^2 + \varepsilon^2$$
    $$ || w - (\delta,0)||^1=1-\delta+\varepsilon$$
    Оптимизируем по большой величине:
    $$ || w - (0, \delta)  ||^2= 1 - 2 \varepsilon \delta + \delta^2 + \varepsilon^2$$
    $$ || w - (\delta,0)||^1=1-\delta+\varepsilon$$
    C точки зрения $L^1$ регуляризации нет разницы, по какой величине оптимизировать. $L^2$ регуляризация склонна в сторону больших выбросов.

- **Объяснение 3:** Проксимальный метод:
$$w^{(k)} = S_{\eta \alpha} ( w^{(k-1)} - \eta \nabla_{w} Q(w^{(k-1)}))$$

$$
S_{\eta \alpha} (w_{i}) = \begin{equation}
    \left\{  
    \begin{array}{}  
        w_{i} - \eta \alpha, w_{i} \geq \eta \alpha,\\  
        0, |w_{i}| < \eta \alpha  \\
w_{i} + \eta \alpha, w_{i} < - \eta \alpha
    \end{array}  
    \right.
\end{equation}
$$
    Эта процедура должна находить минимум у модели.
