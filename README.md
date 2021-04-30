# Тестовое задание. Контур, Data Science 2021.
Камиль Хайруллин (Kamil Khairullin, k.hayrullin@innopolis.university)

## Описание проблемы

Требуется обучить модель, которая сможет приводить символы в наименовании к корректному регистру, так чтобы полученное название наиболее вероятно совпадало с прописанным в уставе ЕГРЮЛ.

## Стратегия решения проблемы

После изучения проблемы я понял, что перед нами стоит задача по определению правильного написания заглавных букв в словах, где такая информация недоступна. Это одна из проблем в NLP, которая называется **Truecasing**. 

Я приступил к изучению данного вопроса, поиску решений и исследований на эту тему. Тогда я наткнулся на исследование [Lucian Vlad Lita, Abe Ittycheriah, Salim Roukos, Nanda Kambhatla (2003). tRuEcasIng](https://www.cs.cmu.edu/~llita/papers/lita.truecasing-acl2003.pdf), которое и взял за основу своего решения. 

В основе решения лежит четыре частотных распределения, в исслледовании называемых:
- **unigram** - для каждого уникального слова, встреченного в training data, хранит количество появлений.
- **forward bigram** - для каждой уникальной пары слов (текущее слово, следующее слово), встреченной в training data, хранит количество появлений.
- **backward bigram** - для каждой уникальной пары слов (предыдущее слово, текущее слово), встреченной в training data, хранит количество появлений.
- **trigram** - для каждой уникальной тройки слов (предыдущее слово, текущее слово, следующее слово), встреченной в training data, хранит количество появлений. \
\
А так же жадный алгоритм, который высчитывает вероятность написания слова на основе четырёх вышеупомянутых структур.

![q2eq](https://user-images.githubusercontent.com/54369751/116721183-b5584d00-a9e5-11eb-989d-4bfede9d5beb.png)
После чего выдаётся самый вероятный casing для данного слова. 

