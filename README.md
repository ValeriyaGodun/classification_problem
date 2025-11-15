## Краткое описание

Этот проект был реализован в рамках обучения на курсе проффесиональной переподготовки (НИЯУ МИФИ). 
- **Задача:** исследовать непараметрические и параметрические байесовские классификаторы, оценить влияние ширины/типа окна и сравнить с логистической регрессией (Task.pdf).
- **Данные** были сгенерированы из распределения:

!картинка формулы

## Структура ноутбука `название.ipynb`

**Генерация выборки.** 
**Проведение разведочного анализа.** 

Гистограммы распределения признаков в каждом классе

<img width="1278" height="451" alt="image" src="https://github.com/user-attachments/assets/dca79ab6-5a14-4d8e-97da-e18a4dc2f952" />

Диаграммы рассеяния

<img width="578" height="432" alt="image" src="https://github.com/user-attachments/assets/ca6bf887-10ed-4c28-9b84-848c5c6a4f26" />

Box-and-whisker

<img width="1303" height="451" alt="image" src="https://github.com/user-attachments/assets/cea273c3-1ad7-4d1d-888d-68ed91f8066e" />

**Обучение непараметрических Байесовских классификаторов:**
  - Расчёт ширины окон по правилу Сильвермана.
  - Кросс-валидация (50 фолдов) для ядер `tophat`, `gaussian`, `epanechnikov`, `linear`.
    
<img width="383" height="498" alt="image" src="https://github.com/user-attachments/assets/8764f837-2bef-4085-b849-4a0c0a627710" />
    
  - Расчёт средней accuracy и std по train/test.
  - Построение графиков зависимости accuracy и std от коэффициента λ (отношение ширины окна к ширине Сильвермана).
  - 
<img width="1068" height="1067" alt="image" src="https://github.com/user-attachments/assets/f3599710-7b71-445d-be44-fd01b13990ad" />

<img width="1317" height="1298" alt="image" src="https://github.com/user-attachments/assets/1f922600-98cb-4dda-8e1e-012834fb969d" />

  - Поиск λ с максимальной обобщающей способностью и минимальной дисперсией.
    
  - Построение диаграмм областей классов для каждого фолда и сводный график со всеми границами.

<img width="702" height="624" alt="image" src="https://github.com/user-attachments/assets/27862cf8-7fe2-4025-91e4-84a06f5a22ab" />

**Метрики качества:** ROC/PR кривые (micro/macro) с AUC для train/test.

<img width="578" height="455" alt="image" src="https://github.com/user-attachments/assets/48591df3-ac3f-4249-aa93-eb8ffd6bbb2a" />

<img width="578" height="455" alt="image" src="https://github.com/user-attachments/assets/bc458b6a-e757-485a-b288-1215bace88e5" />

<img width="597" height="528" alt="image" src="https://github.com/user-attachments/assets/28f54ba5-3fb7-41ed-ae38-b2b6233f01b6" />


**Обучение параметрического Байеса:** GaussianNB (диагональные ковариационные матрицы), усреднённые accuracy и std.

<img width="407" height="89" alt="image" src="https://github.com/user-attachments/assets/6780c211-51bc-4981-a864-83771f563fc3" />

**Обучение логистической регрессии:** multinomial LogisticRegression, усреднённые accuracy и std.



**Дополнительные исследования:** 

- Оценка априорных вероятностей.

- Сравнение границ KDE(лучшее ядро) и GaussianNB.

<img width="1389" height="590" alt="image" src="https://github.com/user-attachments/assets/bff41c12-a0eb-42d0-9398-d5b53ee9a432" />



## Основные результаты

- Лучшее ядро на средней точности по всем λ на тестовой выборке получилось 000.

- Максимальная точность для каждого ядра при его оптимальном λ:
!!!РЕЗУЛЬТАТЫ

Все четыре ядра дают практически одинаковую максимальную точность (≈0.938–0.940) и сопоставимые стандартные отклонения (~0.07), значит метод относительно устойчив к выбору типа ядра. Таким образом, выбор ядра почти не влияет на максимум точности.

- Смещённые априорные вероятности заметно изменяют границы и accuracy. Для данной выборки оптимальнее оставлять априорные вероятности равномерными, т.к. любые искусственные смещения ухудшают обобщающую способность KDE-классификатора.

- GaussianNB уступает непараметрическому классификатору, но обеспечивает плавные границы, логистическая регрессия показывает промежуточный результат.

!!! числа
!!!сравнение моделей график

## Как воспроизвести
1. Открыть `.ipynb` в Jupyter/Colab.
2. Выполнить ячейки по порядку (кросс-валидация и построение графиков может потребовать времени из-за 50 фолдов).
3. Итоговые таблицы и графики автоматически сохраняются в выводах ноутбука.

## Используемые библиотеки
- `numpy`, `pandas`, `matplotlib`
- `scikit-learn` (KDE, GaussianNB, LogisticRegression, метрики)
- `pingouin` (multivariate_normality)
- `itertools`, `collections`, `scipy.special`

