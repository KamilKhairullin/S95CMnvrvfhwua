# Тестовое задание. Контур, Data Science 2021.
Камиль Хайруллин (Kamil Khairullin, k.hayrullin@innopolis.university) <br/>
Ссылка на Github с проектом: https://github.com/KamilKhairullin/S95CMnvrvfhwua

## Описание проблемы

Требуется обучить модель, которая сможет приводить символы в наименовании к корректному регистру, так чтобы полученное название наиболее вероятно совпадало с прописанным в уставе ЕГРЮЛ.

## Стратегия решения проблемы

После изучения проблемы я понял, что перед нами стоит задача по определению правильного написания заглавных букв в словах, где такая информация недоступна. Это одна из проблем в NLP, которая называется **Truecasing**. 

Я приступил к изучению данного вопроса, поиску решений и исследований на эту тему. Тогда я наткнулся на исследование [Lucian Vlad Lita, Abe Ittycheriah, Salim Roukos, Nanda Kambhatla (2003). tRuEcasIng](https://www.cs.cmu.edu/~llita/papers/lita.truecasing-acl2003.pdf), которое и взял за основу своего решения. 

В основе решения лежит четыре частотных распределения, в исслледовании называемых:
- **unigram** - для каждого уникального слова, встреченного в training data, хранит количество появлений.
- **forward bigram** - для каждой уникальной пары слов (текущее слово, следующее слово), встреченной в training data, хранит количество появлений.
- **backward bigram** - для каждой уникальной пары слов (предыдущее слово, текущее слово), встреченной в training data, хранит количество появлений.
- **trigram** - для каждой уникальной тройки слов (предыдущее слово, текущее слово, следующее слово), встреченной в training data, хранит количество появлений.  <br/>
 <br/>
А так же жадный алгоритм, который высчитывает вероятность написания слова на основе четырёх вышеупомянутых структур.

![q2eq](https://user-images.githubusercontent.com/54369751/116721183-b5584d00-a9e5-11eb-989d-4bfede9d5beb.png)
После чего выдаётся самый вероятный casing для данного слова. 

## Структура проекта (Имплементация)

Весь функционал представлен в классе Truecaser (models/truecaser.py)

### Класс Truecaser

Публичные методы: <br/>
**fit(path : String)** - на вход подаётся путь к training data. <br/>
Здесь происходит препоцессинг данных: для каждого предложения разделенного символом "конец строки" убирается символ "конец строки" и происходит разделение предложения на отдельные токены. <br/>

**train()**  - не требует ничего на вход. Здесь происходит создание unigram, forward bigram, backward bigram, trigram на основе данных, запроцессенных методом fit. <br/>
Так же каждое слово в предложении добавляется в сет всех встреченных слов (слова с разным регистром считаются как разные)  <br/>

**predict(sentence : [String])** - на вход поступает предложение в форме массива строк, где каждая строка - одно предложение. <br/> 
Для каждого слова в предложении выбирается наиболее вероятный регистр написания на основе вышеописанного алгоритма. <br/>

**save_model(name: String)** - после обучения модель можно сохранить для дальнейшего использования с помощью этой функции. <br/>

**load_model(path : String)** - и загрузить с помощью этой функции <br/>

## Результаты и тестирование
Я написал класс Tester (tests/tester.py), который расчитывал F1-score для моей модели с помощью метода evaluate_F1. <br/>
В ходе нескольких тестов F1-score составил 90-93% <br/>
<br/>
Так же, чтобы убедится, что алгоритм не добавил лишний пробел, я написал функцию, которая проверяет char-by-char соответствие test и predicted файлов с помощью метода evaulate_consistency.

## Как запустить + требования

Требования: <br/>
click==7.1.2
joblib==1.0.1
nltk==3.6.2
regex==2021.4.4
tqdm==4.60.0

## Источники
1. [Lucian Vlad Lita, Abe Ittycheriah, Salim Roukos, Nanda Kambhatla (2003). tRuEcasIng](https://www.cs.cmu.edu/~llita/papers/lita.truecasing-acl2003.pdf)
2. [Nils Reimers (2016). truecaser](https://github.com/nreimers/truecaser)

