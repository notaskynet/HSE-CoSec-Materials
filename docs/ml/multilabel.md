---
next:
  text: 'К содержанию'
  link: '/ml/ml_index'
prev: false
---

# Классификация с пересекающимися классами (MultiLabel Classification)

$$a: \mathbb{X} \to \{0, 1\}^{K}$$
*Основной подход*: Binary Relevance

*Идея*: Обучаем для каждого класса совй классификаторов, в конце собиарем вектор из получившихся предсказаний.

*Проблема:* Предсказания могут быть скорелированными, а классификаторы ничего друг про друга не знают.