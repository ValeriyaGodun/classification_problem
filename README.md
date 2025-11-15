### Краткое описание

Этот проект был реализован в рамках обучения на курсе проффесиональной переподготовки (НИЯУ МИФИ). 
- **Задача:** исследовать непараметрические и параметрические байесовские классификаторы, оценить влияние ширины/типа окна и сравнить с логистической регрессией (Task.pdf).
- **Данные** были сгенерированы из распределения:


<p align="center">
  <img width="495" height="79" alt="image" src="https://github.com/user-attachments/assets/0111f974-147a-4f4f-8923-0aaaed2f8220" />
</p>

### Структура ноутбука `solution.ipynb`

**Генерация выборки.** 

**Проведение разведочного анализа.** 

Гистограммы распределения признаков в каждом классе

<p align="center">
  <img width="1479" height="516" alt="image" src="https://github.com/user-attachments/assets/e3f1aa2e-7b80-4ade-bec6-13509deba37b" />
</p>

Диаграммы рассеяния

<p align="center">
  <img width="675" height="486" alt="image" src="https://github.com/user-attachments/assets/a7c39b4c-166f-4bc6-99bb-6d932fb09667" />
</p>

Box-and-whisker

<p align="center">
  <img width="1491" height="497" alt="image" src="https://github.com/user-attachments/assets/99c2733a-9eea-4825-96f8-3e3f508539a0" />
</p>

**Обучение непараметрических Байесовских классификаторов:**
  - Расчёт ширины окон по правилу Сильвермана.
  - Кросс-валидация (50 фолдов) для ядер `tophat`, `gaussian`, `epanechnikov`, `linear`.
  - Расчёт средней accuracy и std по train/test.
    
<p align="center">
  <img width="473" height="496" alt="image" src="https://github.com/user-attachments/assets/d8c68a32-dabe-422f-a808-94c540b105bb" />
</p>

  - Построение графиков зависимости accuracy и std от коэффициента λ (отношение ширины окна к ширине Сильвермана).
    
<p align="center">
  <img width="1220" height="594" alt="image" src="https://github.com/user-attachments/assets/33829125-6319-471e-9f01-a0a073d8b52c" />
</p>

<p align="center">
  <img width="1511" height="716" alt="image" src="https://github.com/user-attachments/assets/aeb28e51-1a19-4b12-8ed3-72d168a14749" />
</p>

  - Поиск λ с максимальной обобщающей способностью и минимальной дисперсией.
    
  - Построение диаграмм областей классов для каждого фолда и сводный график со всеми границами.

<p align="center">
  <img width="799" height="704" alt="image" src="https://github.com/user-attachments/assets/5c05e3be-90b6-4313-9e29-a2497b893884" />
</p>

**Метрики качества:** ROC/PR кривые (micro/macro) с AUC для train/test.

<p align="center">
  <img width="669" height="515" alt="image" src="https://github.com/user-attachments/assets/ce001fd9-4437-42a2-b25e-f6c26eb6adbe" />
</p>

<p align="center">
  <img width="675" height="512" alt="image" src="https://github.com/user-attachments/assets/56fdce3d-7acf-4f8e-9e1f-4053bd3e307d" />
</p>

<p align="center">
  <img width="606" height="522" alt="image" src="https://github.com/user-attachments/assets/1cb1fbbe-72fb-406d-86b6-4d1620a23d69" />
</p>


**Обучение параметрического Байеса:** GaussianNB (диагональные ковариационные матрицы), усреднённые accuracy и std.

<p align="center">
  <img width="392" height="97" alt="image" src="https://github.com/user-attachments/assets/f5bb837c-7811-4e41-95e2-52c36f0ed890" />
</p>

**Обучение логистической регрессии:** multinomial LogisticRegression, усреднённые accuracy и std.

<p align="center">
  <img width="388" height="82" alt="image" src="https://github.com/user-attachments/assets/8c7da5d3-360a-4b29-9a1d-75ae1506bd08" />
</p>


**Дополнительные исследования:** 

- Оценка априорных вероятностей.
- Сравнение границ.

## Основные результаты

- Лучшее ядро на средней точности по всем λ на тестовой выборке получилось gaussian.

- Максимальная точность для каждого ядра при его оптимальном λ:

<p align="center">
  <img width="715" height="110" alt="image" src="https://github.com/user-attachments/assets/1fa11198-6648-4aa6-957d-2460a318390f" />
</p>

Все четыре ядра дают похожую максимальную точность и сопоставимые стандартные отклонения, значит метод относительно устойчив к выбору типа ядра. Оптимальная ширина окна же варьируется по ядрам - все ядра достигают максимума при разных λ. Это указывает, что для данной задачи выбор ядра менее критичен, чем правильная ширина окна.

- Смещённые априорные вероятности заметно изменяют границы и accuracy. Для данной выборки оптимальнее оставлять априорные вероятности равномерными, т.к. любые искусственные смещения ухудшают обобщающую способность KDE-классификатора.

- GaussianNB уступает непараметрическому классификатору, но обеспечивает плавные границы, логистическая регрессия показывает промежуточный результат.

<p align="center">
  <img width="895" height="135" alt="image" src="https://github.com/user-attachments/assets/27ebd93a-7a10-439f-ae1a-0f477e33d6d4" />
</p>

<img width="1647" height="485" alt="image" src="https://github.com/user-attachments/assets/ba694060-3596-4bdb-8fe1-b83e4f6d6f8f" />


