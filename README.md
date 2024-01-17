## Цель работы
На платформе kaggle была представлена [кредитная история](https://www.kaggle.com/datasets/ranadeep/credit-risk-dataset/data) клиентов финансового учреждения. 
<br/>Цель состоит в том, чтобы заранее спрогнозировать возможных неплательщиков по кредитам и помочь финансовым учреждениям предпринять соответствующие шаги.

Источник предоставил подробное описание данных, оно представлено в файле "LoanStats tab in LCDataDictionary.xlsx"

## План работы
1. Сбор и предобработка данных:
- Выбор параметров
- Заполнение пропусков
- Кодирование категориальных переменных
2. Корреляционный анализ.
3. Графический анализ.
4. Проверка гипотез
5. Построение пайплайна модели:
- Разделение данных на подвыборки и выделение целевой переменной.
- Кодирование переменных.
- Обучение моделей.
6. Проверка эффективности лучшей модели на тестовой выборке.
7. Анализ значимости переменных для лучшей модели.
8. Результат работы

# Результат работы

1. Был дан датасет на 887379 строк и 74 столбца. Перед анализом признаков, данные были предобработаны
-  удалены столбцы, которые могли привести к утечке целевого признака либо же не несшие в себе какого-либо практического смысла
- были обработаны пропущенные значения в двух признаках (длительность трудоустройства заполнено в соответствии с логикой нулями, пропуски в доходах клиентов были заполнены смежным показателем, оставшиеся после этого пропуски также были заполнены нулевыми значениями)
- были обработаны категориальные признаки: переведены в целочисленные значения
- был выделен целевой признак: за наличие риска принимается все кредиты со статусом уаазывающим на его неполное погашение
2. [**Корреляционный анализ:**](#eda)
- наблюдается высокая корреляция между признаком grade и int_rate, installment и loan_amnt. В дальнейшем рассмотрим их взаимосвязь более подробно.
3. **Анализ данных (EDA):** 
- Матрица корреляции показала, что есть сильная [зависимость](#1_graph) между величиной кредита и величиной платежа по нему. При увеличении величины кредита, платеж по нему также увеличивается. существует несколько возможных причин для наблюдаемой зависимости между величиной кредита и величиной платежа по нему, однако, для более точного анализа и понимания причин данной зависимости необходимо провести дополнительные исследования и учитывать другие факторы, такие как доход, срок кредита, процентная ставка и т.д.
- Если рассматривать цели кредитование клиентов данной финансовой организации, то можно увидетть, что наибольшая доля заемщиков берет кредит с целью консолидации долга, другими словами рефинансирования долга. Информация, полученная из [данного графика](#2_graph) может быть важна для банков и кредиторов при формировании стратегий кредитования. Например, банк может решить ужесточить условия для кредитования на определенные цели или, наоборот, предложить более выгодные условия для стимулирования спроса на кредиты в желаемых сферах.
- Если же [рассматривать](#3_graph) суммы кредтов, выданных на различные цели, то тут мы можем проселдить логику, что наибольшие крдеиты выдаются на цели связанные с жильем (house, home_iprovement), на финансовые цели (dept_consolidation, credit_card) и на построение собственного бизнеса (small business). 
- Оценка зависимости между стажем работы и процентными ставками: [График](#4_graph) позволяет визуально выявить, существует ли корреляция между стажем работы заемщика и уровнем предлагаемых процентных ставок. Это важно для понимания, как банк или кредитор учитывает опыт работы при формировании ставок. Вопреки моему предположению, визуально стаж работы практически никак не влияет на процентную ставку, данный график демонтрирует, что средняя процентная ставка по кредитам у заемщиков с различным стажем работы практически равна.
- В описании данных источник не предоставил информации о том, чем же все-таки конкретно является признак Grade. При помощи корреляционного анализа было выявлено, что данный признак имеет болшую корреляцию с процентной ставкой. Так при помощи [двух графиков](#5_graph) установил, что признак грейд является внутренней градацией клиентов банка именно по этому признаку. Предположение о том, что доход также значим для параметра грейд не потвердилось.
- Рассмотрим распределение дохода клиента банка в зависимости от налчиия риска невыплаты кредита. По [гистограмме](#6_graph) мы видим, что распределение доходов между двумя группами незначительно разница.Однако по  графику трудно сделать однозный вывод, подобная визуализация плохо отображает распределение среднего дохода. Оставим проверку данной гипотезы статистическому тесту. 
- То же самое можно сказать и про [распределение](#7_graph) среднего по сумме кредита в разбивке на наличие риска невыплаты. Также проведем статистческий тест для подтверждения или опровержения гипотезы о равенстве средних.
4. [**Проверка гипотез:**](#gipotezy)
<br/>В процессе рассмотрения и анализа данных были сформированы следующие гипотезы:
- Средний доходы клиентов в разбивке на наличие риска равны
- Средний размеры кредитов в разбивке на наличие риска равны
<br/>Ни одна из этих гипотез не подтвердилась. P-value были меньше установленного уровня статистической значимости. Что означает, вероятность получить такие или более экстремальные различия между средними значениями двух групп (с риском и без) случайно при условии, что нулевая гипотеза верна (т.е. средние значения равны), крайне низкая. Таким образом, мы отвергаем нулевые гипотезы о равенстве средних значений двух групп. 
<br/> 5. [**Обучение моделей**:](#modely)
Всего было обучено 3 модели:
- RandomForestClassifier
- KNeighborsClassifier
- CatBoostClassifier
При обучении каждой модели использовался pipline состоящий из кодирования категориальных/числовых признаков и подбора гипперпараметров модели про помощи Gridsearch.
<br/>Наилучший показатель accuracy продемонстрировала модель случаного леса с гипперрпараметрами {'classifier__criterion': 'gini', 'classifier__max_depth': 10, 'classifier__n_estimators': 50}. Лучшая оценка: 0.923281486798603.
<br/> 6. [**Оценка лучшей модели:**](#ocenka)
- Точность на тестовой выборке: 0.9233
- ROC-AUC лучшей модели: 0.7293
<br/>Самыми важными для предсказания признаками оказались int_rate (процентная ставка), plan (наличие плана выплат) и home_ownership	(тип владения жельем)
