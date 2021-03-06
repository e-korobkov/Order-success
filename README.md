# Что программа делает
Ищет лучшие параметры random forest.
Сохраняет модель на диск.

# Описание программы.
Программа разделена на 2 части (по причине длительного времени выполнения за один запуск), part1 и part2 - оба файла одинаковые.
- Part1 – прекращает выполнение после блока «min_samples_leaf_grid»;
- Part2 – продолжает вычисления с блока «GridSearchCV на основе рекомендуемых параметров»
- 
### 1. Обработка данных (ETL):
Блок кода содержит «типовые» для таких задач проверки/преобразования:
- Удаление не влияющих на результат данных;
- Удаление данных, коррелирующих с ответом;
- Удаление данных с большим числом уникальных значений;
- Обработка пропусков;
- Выбор между коррелирующими переменной с наименьшим вкладом с последующим удалением.

### 2. Построение предварительных деревьев:
Цель раздела: выяснить окончательную форму набора данных:
- Обработанных LabelEncoder;
- Переработан в dummi переменные.
**Dummi набор показал лучший результат. Отличия не большие, но это повлечет сложности в обработке при прогнозировании новых данных.**

### 3. Поиск оптимальных параметров:
Запускается поиск параметров для трех показателей:
- Количества деревьев (trees_grid) – Наилучшее значение 85.99% и достигается при 3000 деревьях;
- Максимальная глубина (max_depth_grid) - Наилучшее значение 0.931 и достигается при максимальной глубине 160;
- Минимальное число листьев – Наилучшее значение 0.931 и достигается при минимальном количестве наблюдений в листе, равном 1

Следующим шагом проводиться перекрестная проверка GridSearchCV на основе рекомендованных параметров и лучший результат достигается при: 
- max_depth: 120;
- min_samples_leaf: 1;
- n_estimators: 1000/
**Два параметра отличаются от рекомендованных в части 3.1**

### 4. Построение окончательной модели
На основании данных GridSearchCV, строится финальная модель **с дополнительной настройкой warm_start = True**, получаем основные оценки.

### 5. Сохраняем модель на диск
Сохраняем на диск предсказательную модель и столбцы из набора с данными (чтобы не подать на вход модели не известные ей данные)/

### 6. Построение ROC-кривой и выбор оптимального порога
Для полноты картины выведу ROC кривую, тут нет правильного решения, каждая компания сама решает с какой ошибкой готова мериться при предсказании.

### 7. Для информации
Контрольно строится модель на наборе данных преобразованном LabelEncoder.
