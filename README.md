
Lab1
Lab1_
Разведочный анализ данных. Исследование и визуализация данных.
В качестве набора данных для анализа и визуализации был выбран набор данных. Он содержит результаты цифровой обработки изображений тонкоигольной аспирационной биопсии (FNA) молочной железы. Для каждого ядра клетки рассчитываются 10 вещественных признаков: радиус (среднее расстояние от центра до точек периметра), текстура (стандартное отклонение значений градаций серого), периметр, площадь, гладкость (локальное изменение радиуса), компактность (периметр²/площадь - 1.0), вогнутость (степень вогнутых частей контура), вогнутые точки (количество вогнутых частей контура), симметричность и фрактальная размерность.

Загрузка библиотек и данных

[ ]
Collecting ucimlrepo
  Downloading ucimlrepo-0.0.7-py3-none-any.whl.metadata (5.5 kB)
Requirement already satisfied: pandas>=1.0.0 in /usr/local/lib/python3.12/dist-packages (from ucimlrepo) (2.2.2)
Requirement already satisfied: certifi>=2020.12.5 in /usr/local/lib/python3.12/dist-packages (from ucimlrepo) (2026.4.22)
Requirement already satisfied: numpy>=1.26.0 in /usr/local/lib/python3.12/dist-packages (from pandas>=1.0.0->ucimlrepo) (2.0.2)
Requirement already satisfied: python-dateutil>=2.8.2 in /usr/local/lib/python3.12/dist-packages (from pandas>=1.0.0->ucimlrepo) (2.9.0.post0)
Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.12/dist-packages (from pandas>=1.0.0->ucimlrepo) (2025.2)
Requirement already satisfied: tzdata>=2022.7 in /usr/local/lib/python3.12/dist-packages (from pandas>=1.0.0->ucimlrepo) (2026.1)
Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.12/dist-packages (from python-dateutil>=2.8.2->pandas>=1.0.0->ucimlrepo) (1.17.0)
Downloading ucimlrepo-0.0.7-py3-none-any.whl (8.0 kB)
Installing collected packages: ucimlrepo
Successfully installed ucimlrepo-0.0.7

[ ]
Данные были загружены с помощью кода со Scikit-learn и объеденины в единый набор данных.


[ ]

Основные характеристики датасета

[ ]
# Размер датасета - 569 строк, 31 колонка
data.shape
(569, 31)

[ ]
total_count = data.shape[0]
print('Всего строк: {}'.format(total_count))
Всего строк: 569

[ ]
# Список колонок
data.columns
Index(['radius1', 'texture1', 'perimeter1', 'area1', 'smoothness1',
       'compactness1', 'concavity1', 'concave_points1', 'symmetry1',
       'fractal_dimension1', 'radius2', 'texture2', 'perimeter2', 'area2',
       'smoothness2', 'compactness2', 'concavity2', 'concave_points2',
       'symmetry2', 'fractal_dimension2', 'radius3', 'texture3', 'perimeter3',
       'area3', 'smoothness3', 'compactness3', 'concavity3', 'concave_points3',
       'symmetry3', 'fractal_dimension3', 'diagnosis'],
      dtype='object')

[ ]
# Список колонок с типами данных
data.dtypes


[ ]
# Проверка на наличие пустных значений
for col in data.columns:
    # Количество пустых значений - все значения заполнены
    temp_null_count = data[data[col].isnull()].shape[0]
    print('{} - {}'.format(col, temp_null_count))
radius1 - 0
texture1 - 0
perimeter1 - 0
area1 - 0
smoothness1 - 0
compactness1 - 0
concavity1 - 0
concave_points1 - 0
symmetry1 - 0
fractal_dimension1 - 0
radius2 - 0
texture2 - 0
perimeter2 - 0
area2 - 0
smoothness2 - 0
compactness2 - 0
concavity2 - 0
concave_points2 - 0
symmetry2 - 0
fractal_dimension2 - 0
radius3 - 0
texture3 - 0
perimeter3 - 0
area3 - 0
smoothness3 - 0
compactness3 - 0
concavity3 - 0
concave_points3 - 0
symmetry3 - 0
fractal_dimension3 - 0
diagnosis - 0

[ ]
# Основные статистические характеристки набора данных
data.describe()


[ ]
# Определим уникальные значения для признака Gender
data['diagnosis'].unique()
array(['M', 'B'], dtype=object)
Визуальное исследование

[ ]
#построение диаграммы рассеяния 
fig, ax = plt.subplots(figsize=(10,10)) 
sns.scatterplot(ax=ax, x='radius1', y='texture1', data=data)

Видно, что нет явной зависимости тексторы от радиуса. Посмотрим насколько влияет целевой признак.


[ ]
fig, ax = plt.subplots(figsize=(10,10)) 
sns.scatterplot(ax=ax, x='radius1', y='texture1', data=data, hue='diagnosis')

Видно, что радиус большинства клеток злокачественных клеток (оранжевые) находится в диапозоне меньше 15 и ни одна не выходит за пределы 20.


[ ]
fig, ax = plt.subplots(figsize=(10,10)) 
sns.histplot(data['radius1'], kde=True, ax=ax)


[ ]
sns.jointplot(x='radius1', y='texture1', data=data)


[ ]
sns.jointplot(x='radius1', y='texture1', data=data, kind="hex")


[ ]
sns.jointplot(x='radius1', y='texture1', data=data, kind="kde")


[ ]
sns.pairplot(data, vars=['radius1', 'texture1', 'perimeter1', 'area1'])


[ ]
sns.pairplot(data, hue='diagnosis', vars=['radius1', 'texture1', 'perimeter1', 'area1'])

Ящик с усами
Отображает одномерное распределение вероятности.


[ ]
sns.boxplot(x=data['radius1'])


[ ]
sns.boxplot(y=data['radius1'])


[ ]
sns.boxplot(x='diagnosis', y='radius1', data=data)


[ ]
sns.violinplot(x=data['radius1'])


[ ]
fig, ax = plt.subplots(2, 1, figsize=(10,10))
sns.violinplot(ax=ax[0], x=data['radius1'])
sns.histplot(data['radius1'], kde=True, ax=ax[1])


[ ]
sns.violinplot(x='diagnosis', y='radius1', data=data)


[ ]
sns.catplot(y='radius1', x='diagnosis', data=data, kind='violin', split=True)

Информация о корреляции признаков

[ ]
numeric_data = data.select_dtypes(include=['float64', 'int64'])
numeric_data.corr()


[ ]
data.select_dtypes(include=['float64', 'int64']).corr(method='pearson')


[ ]
data.select_dtypes(include=['float64', 'int64']).corr(method='kendall')

Построим тепловую карту корреляций


[ ]
sns.heatmap(data.select_dtypes(include=['float64', 'int64']).corr())


[ ]
plt.figure(figsize=(20, 18))
sns.heatmap(data.select_dtypes(include=['float64', 'int64']).corr(), 
            annot=True, 
            fmt='.2f',  # уменьшил точность до 2 знаков
            cmap='coolwarm',
            center=0,
            square=True)


[ ]
plt.figure(figsize=(20, 18))
sns.heatmap(data.select_dtypes(include=['float64', 'int64']).corr(), 
            cmap='YlGnBu', 
            annot=True, 
            fmt='.2f',
            square=True)


[ ]
import numpy as np

# Берем только числовые столбцы
corr_matrix = data.select_dtypes(include=['float64', 'int64']).corr()

# Создаем маску для верхней части (оставляем нижнюю)
mask = np.zeros_like(corr_matrix, dtype=bool)
mask[np.triu_indices_from(mask)] = True

# Рисуем тепловую карту
plt.figure(figsize=(20, 18))
sns.heatmap(corr_matrix, 
            mask=mask, 
            annot=True, 
            fmt='.2f', 
            cmap='YlGnBu',
            square=True)


[ ]
# Берем числовые данные один раз
numeric_data = data.select_dtypes(include=['float64', 'int64'])

fig, ax = plt.subplots(1, 3, figsize=(18, 6))  # sharex='col', sharey='row' убрал, с ними были проблемы
sns.heatmap(numeric_data.corr(method='pearson'), ax=ax[0], annot=False, fmt='.2f', cmap='coolwarm', center=0)
sns.heatmap(numeric_data.corr(method='kendall'), ax=ax[1], annot=False, fmt='.2f', cmap='coolwarm', center=0)
sns.heatmap(numeric_data.corr(method='spearman'), ax=ax[2], annot=False, fmt='.2f', cmap='coolwarm', center=0)
fig.suptitle('Корреляционные матрицы, построенные различными методами', fontsize=14)
ax[0].title.set_text('Pearson')
ax[1].title.set_text('Kendall')
ax[2].title.set_text('Spearman')
plt.tight_layout()
plt.show()


[ ]
fig, ax = plt.subplots(1, 1, figsize=(25, 22))
fig.suptitle('Корреляционная матрица', fontsize=16)
sns.heatmap(data.select_dtypes(include=['float64', 'int64']).corr(), 
            ax=ax, 
            annot=True, 
            fmt='.1f',  # всего 1 знак после запятой вместо 3
            cmap='coolwarm',
            center=0,
            square=True,
            annot_kws={'size': 14})  # мелкий шрифт для чисел

Платные продукты Colab - Отменить подписку
Не удается связаться с сервисом reCAPTCHA. Проверьте подключение к Интернету и перезагрузите страницу.
