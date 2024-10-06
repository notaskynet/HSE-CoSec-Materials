---
prev:
  text: 'Экспоненциальные семейства'
  link: '/statistics/07_exp_functions'
next:
  text: 'К содержанию'
  link: '/statistics/statistics_index'
outline: deep
---

# Условное математическое ожидание

Пусть задано вероятностное простраство $\langle \Omega, F, P \rangle \to P_{B}(A) = P(A|B)$
$$ P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad A, B \subset \Omega $$
Зададим условную вероянтность для одного события:
$$\mathbb{E} (\xi \mid B) = \int_{\Omega} \xi \cdot P(d\omega \mid B) = \int_{\Omega}\xi \cdot \frac{P(d \omega \cap B)}{P(B)} = \frac{1}{P(B)} \int_{B} \xi(\omega) P(d \omega)$$
Построим условную вероятность для некоторого набора событий(разбиения):
$$B = \{ B_{1} \dots B_{n} \}, P(B_{i}) > 0$$
Тогда:
$$ \mathbb{E} (\xi \mid \beta) = \sum_{i=1}^{n} \mathbb{I}(\omega \in B) \cdot \mathbb{E}(\xi \mid \beta_{i})$$
Из вида функции можно заметить основное свойство, которое нам пригодится далее:
$$
\begin{array}{lcl}
\mathbb{E} (\mathbb{E} (\xi \mid \beta)) &=& \mathbb{E} \sum_{i=1}^{n} \mathbb{I} (\omega \in \beta_{i}) \frac{1}{P(\beta_{i})} \int_{\beta_{i}} \xi(\omega) P(d \omega) = \\
&=& \sum_{i=1}^{n} \frac{1}{P(\beta_{i})} \int_{\beta_{i}} \xi(\omega) P(d \omega) = \mathbb{E} (\mathbb{I}[\omega_{i} \in \beta_{i}]) = \\
&=& \sum_{i=1}^{n} \int_{\beta_{i}} \xi(\omega) P(d \omega) = \int_{\beta_{1} \cup \dots \cup \beta_{n}} \xi(\omega) P(d \omega) = \\
&=& \int_{\Omega} \xi(\omega) P(d \omega) = \mathbb{E} \xi
\end{array}
$$
По аналогии с условным математическим ожиданием зададим условная плотность:
$$f_{\xi \mid \eta} (x, y) = \frac{f_{\xi \mid \eta}(x, y)}{f_{\eta}(y)}$$

### Свойства условного математического ожидания
>[!info] Свойства математического ожидания
>1. $\gamma \mathbb{E}(\alpha \mid \beta) + \sigma \mathbb{E}(\eta \mid \beta) = \mathbb{E}(\gamma \alpha + \sigma \eta | \beta)$ (*Линейность*)
>2. Если $\xi$ и $\eta$ независимы, то $\mathbb{E} (\xi \mid \eta) = \mathbb{E} \xi$ (*Независимость*)
>3. $P(\xi = const) = 1 \implies \mathbb{E}(\xi | \beta) = const$ (Как частный случай независимости)
>4. $\mathbb{E}(\mathbb{E}(\xi \mid \beta) \mid \beta) = \mathbb{E}(\xi \mid \beta)$ (*Law of total expectation*, доказательство выше)

Рассмотрим $\xi = \mathbb{I} [\omega \in A]$, $\beta = \{ \beta_{1} \dots \beta_{n}\}$. Тогда при условии, что $w \in \beta_{i}$:
$$\mathbb{E}(\mathbb{E}(I_{A} \mid \beta)) = P(A)$$
Таким образом мы поличили функцию полной вероятности для условного математического одидания.
