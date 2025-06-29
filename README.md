# Weather_prediction

## Описание проекта
Микросервис для предсказания погоды, основанный на модели градиентного бустинга. Сервис оценивает вероятность дождя на следующий день по текущим погодным условиям.
Модель обёрнута в веб-приложение, к которому можно обращаться по сети.
Также в коде имеются тесты для оценки работоспособности сервиса и модели.


## Как работать с проектом?

### 1. Скачивание и установка зависимостей
1. Установка библиотек:
~~~
pip install -r requirements.txt
pip install -e .
~~~
Бывает, что pip пропускает модуль **marshmallow_dataclass**. Можно установить его вручную:
~~~
pip install marshmallow_dataclass
~~~

### 2. Использование модели машинного обучения
Можно обучить модель заново или получить предсказание.

**Обучение модели:**
~~~
python ml_project/train_test_pipeline.py  --mode=fit --config=config/config_lgbm.yaml
~~~

**Предсказание погоды:**
~~~
python ml_project/train_test_pipeline.py  --mode=predict --config=config/config_predict.yaml
~~~

**Запуск юнит-тестов для пайплайна обучения модели:**
~~~
python ml_project/tests/test_full_train_fit_pipeline.py
python ml_project/tests/data/test_get_dataset.py
python ml_project/tests/features/test_transformer.py
python ml_project/tests/models/test_train_model.py
~~~


### 3. Запуск веб-приложения

**Запуск веб-сервера:**
~~~
uvicorn --app-dir=online_inference app:app --host 127.0.0.1 --port 8000
~~~

**Ручная проверка post-запросов:**
~~~
python -m online_inference.script
~~~

**Тесты для веб-приложения:**
~~~
python -m online_inference.tests
~~~



## Архитектура проекта
==============================

    ├── LICENSE         
    ├── README.md          
    ├── data               <- Папка c датасетом и предиктом 
    │
    ├── notebooks          <- Ноутбук с разведочным анализом данных
    │
    ├── configs            <- Конфиги с параметрами моделей (для обучения: список фич, параметры модели, пути для сохранения; для предикта: путь к модели и результатам)
    │
    ├── requirements.txt   <- Файл с зависимостями
    │
    ├── ml_project         <- Исходный код ML-микросервиса
    │   ├── __init__.py    
    │   │
    │   ├── data           <- Скрипт для загрузки и разделения данных на обучающую и тестовую выборку
    │   │   └── get_data.py
    │   │
    │   ├── features       <- Трансформер для обработки датасета
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Скрипт для работы с моделью
    │   │   └── model_fit_predict.py
    │   │
    │   ├── enities        <- Параметры модели  датасета в формате DataClass (чёрная магия для парсинга конфигов)
    │   │   ├── features_params.py
    │   │   ├── model_params.py
    │   │   └── split_params.py
    │   │
    │   └── tests          <- Юнит-тесты отдельных модулей и тест пайплайна для обучения модели
    │
    |
    ├── online_inference   <- Код веб-приложения для онлайн-использования модели
    │   ├── app.py         <- Точка входа: обработчики запросов и валидация
    │   │
    │   ├── script.py      <- Скрипт для создания post-запросов 
    │   |
    │   ├── tests.py       <- Юнит-тесты для проверки работы приложения
    │   │
    │   └── data_utils    
    │       └── data_utils.py  <- Утилиты для работы с моделью (удобная загрузка модели и датасета, валидация данных из post-запроса)
    │
    └── setup.py            

-----------------
