# Определение стоимости автомобилей

## Цель проекта
---
__Описание исследования__

Сервис по продаже автомобилей с пробегом «Не бит, не крашен» разрабатывает приложение для привлечения новых клиентов. В нём можно быстро узнать рыночную стоимость своего автомобиля. В вашем распоряжении исторические данные: технические характеристики, комплектации и цены автомобилей.

__Цель исследования__

Нужно построить модель для определения стоимости автомобиля на основе имеющихся данных.
Заказчику важны:

- качество предсказания;
- скорость предсказания;
- время обучения.

В качестве метрики моделей выбрана RMSE, она не должна быть больше 2500.

__Задачи исследования__

Необходимо загрузить данные, выполнить их предобработку: анализ типов данных, заполнение или устранение пропусков, поиск дубликатов. Затем провести исследовательский анализ с целью выявления закономерностей в данных, а также выявления аномалий и выбросов. Провести корреляционный анализ. Затем, осуществить обучение нескольких моделей регрессии, одна из которых — LightGBM, как минимум одна — не бустинг. Для каждой модели попробовать подобрать разные гиперпараметры.
Проанализировать время обучения, время предсказания и качество моделей. Опираясь на критерии заказчика, выбрать лучшую модель, проверить её качество на тестовой выборке.


__Исходные данные__

В нашем датасете присутсвуют следующие признаки:

- DateCrawled — дата скачивания анкеты из базы
- VehicleType — тип автомобильного кузова
- RegistrationYear — год регистрации автомобиля
- Gearbox — тип коробки передач
- Power — мощность (л. с.)
- Model — модель автомобиля
- Kilometer — пробег (км)
- RegistrationMonth — месяц регистрации автомобиля
- FuelType — тип топлива
- Brand — марка автомобиля
- Repaired — была машина в ремонте или нет
- DateCreated — дата создания анкеты
- NumberOfPictures — количество фотографий автомобиля
- PostalCode — почтовый индекс владельца анкеты (пользователя)
- LastSeen — дата последней активности пользователя

Целевой признак:
- Price — цена (евро)
## Результат
---
После реализации всех этапов подготоки данных, мы обучили несколько моделей для реализации прогнозо и получили следующие результаты:
1. Метрика лучшей из моделей, которой оказалось дерево решений с глубиной дерева равной 13-и, 
   равна -1945.589.
2. Время обучения модели - 0.7845 с.
3. Время реализации прогноза - 0.2532 с.

Далее, провели обучение моделей градиентного бустинга. Первым делом разрабатывали модель __LightGBM__, произвели кодирование категориальных столбцов с помощью LabelEncoder, выбрали гиперпараметры: скорость обучения, глубину дерева и количество деревьев в ансамбле. С помощью перебора по сетке выбрали лучшую из моделей и также замерили времена и рассчитали метрику RMSE.
После обучения моделей зафиксируем результаты:
1. Метрика лучшей из моделей LightGBM с параметрами: learning_rate=0.2, max_depth=12, n_estimators=150 
   равна -1637.161.
2. Время обучения модели - 0.6941 с.
3. Время реализации прогноза - 0.366 с.

Заключительным этапом было обучение модели __CatBoost__, выбор лучшей модели производился также с помощью GridSearch и несколькими гиперпараметрами.

1. Метрика лучшей из моделей CatBoost с параметрами: depth=12, iterations=150, learning_rate=0.3,
   равна -1637.698.
2. Время обучения модели - 19.6916 с.
3. Время реализации прогноза - 0.3929 с.

Исходя из данных полученных на валидационных выборках, можно сказать, что лучше всех с задачей справляется модель CatBoost, ее метрика RMSE оказалась наименьшей, а время обучения и предсказания расположилось между моделями дерева решений и LightGBM. При этом, хоть дерево решений и показало самую низкую метрику RMSE, но в качестве плюсов можно отметить скорости обучения и предсказания, которые в несколько раз лучше, чем у моделей градиентного бустинга.

Для заказчика важны следующие критерии работы модели:
- качество предсказания;
- время обучения модели;
- время предсказания модели.

Исходя из этого, рекомендуется использование модели __LightGBM__, которая показала себя наиболее точной по результам оценки метрик, а также первой по быстроте в обучении и второй по времени предсказания.

Мы рекомендуем заказчику обратить внимание на модель __LightGBM__, которая продемонстрировала наибольшую точность и показала хорошее время работы. 
Последним этапом нашей работы стало использование лучшей модели LightGBM на тестовых данных. Метрика RMSE на тестовых данных достигла __1636.44613__, что говорит нам о том, что стоит поработать над подбором гиперпараметров для модели, а также, если точность все же важнее скорости обучения, то выбрать модель CatBoost.