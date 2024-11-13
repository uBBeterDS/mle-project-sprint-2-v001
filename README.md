# mle-template-case-sprint2

Цель проекта - улучшение базовой модели предсказания стоимости квартир при помощи MLflow.

git clone https://github.com/uBBeterDS/mle-project-sprint-2-v001.git
cd mle-project-sprint-2-v001
pip install -r requirements.txt

Этап 1: Разворачивание MLflow и регистрация модели

    sh mlflow_server/run_mlflow_server.sh
    experiment id: 5

Этап 2: Проведение EDA

    Шаги:
    1. Статистическое описание: Анализ центральных тенденций и разброса числовых переменных, что позволяет выявить ключевые характеристики.

    2. Анализ пропусков: Выявление количества пропущенных значений, чтобы определить, требуется ли их обработка.

    3. Корреляционный анализ: Исследование взаимосвязей между переменными с помощью корреляционной матрицы для понимания, какие факторы влияют друг на друга.

    4. Визуализация распределений: Гистограммы для переменных, таких как площадь и цена, помогают понять их распределение и наличие аномалий.

    5. Анализ категориальных переменных: Использование коробчатых графиков для оценки влияния категориальных признаков на целевую переменную (цену).

    6. Географический анализ: Визуализация цен по геолокации позволяет увидеть пространственные паттерны и их влияние на стоимость.

Этап 3: Генерация признаков и обучение модели

    В процессе преобразования данных с помощью класса CustomFeaturesTransformer были сгенерированы следующие признаки:

    distance_to_center: Расстояние до центра (вычислено по координатам долготы и широты).
    space_per_room: Площадь на комнату (общая площадь делится на количество комнат).
    kitchen_total_area: Доля кухни от общей площади (площадь кухни делится на общую площадь).
    living_total_area: Доля жилой площади от общей площади (жилую площадь делят на общую площадь).
    high_floor: Признак, указывающий, находится ли квартира на высоком этаже (более половины от общего числа этажей).
    age: Возраст здания (текущий год минус год постройки).
    flats_density: Плотность квартир на этаже (общее количество квартир делится на количество этажей).

MLflow run name: feature_generation

Этап 4: Отбор признаков и обучение новой версии модели

    Метод 1: Важность признаков из модели

    После обучения модели CatBoostRegressor была извлечена важность признаков через feature_importances_.
    Рассчитана медианная важность, и выбраны признаки, у которых важность выше этой медианы. Это позволяет отобрать наиболее значимые признаки, которые способствуют предсказанию целевой переменной.

    Метод 2: Permutation Importance

    Модель была переобучена, и использован метод "permutation importance" для оценки значимости каждого признака.
    Важность признаков была отсортирована, и отобраны признаки по заданным порогам (например, 0.01, 0.02 и т.д.) для дальнейшего анализа и улучшения модели.

    MLflow run name: feature_selection

Этап 5: Подбор гиперпараметров и обучение новой версии модели

    Метод 1: Randomized Search CV

    Использован RandomizedSearchCV для подбора гиперпараметров модели CatBoostRegressor.

    Метод 2: Optuna

    Использована библиотека Optuna для более продвинутого подбора гиперпараметров.

    MLflow run name: parameter_tuning
