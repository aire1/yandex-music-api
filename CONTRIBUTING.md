# Как внести свой вклад

Внесение своего вклада в этот проект ничем не отличается внесения в других 
open source проекты на GitHub'e, но есть несколько ключевых моментов, о которых
стоит знать и помнить.

Все необходимые пакеты для разработки есть в dev секции файла с зависимостями.
Установить их можно с помощью следующей команды:
```
pipenv install --dev
```

Основные типы вклада:
- добавление новых полей к существующим классам;
- добавление новых классов;
- установка опциональности какого-то поля;
- добавление нового метода в `Client` (новый запрос на API);
- добавление примера использование в папку [examples](examples).

Ваш вклад может быть более грандиозным. Так, например, можно написать 
интеграционные тесты для класса `Client` и всех методов-сокращений в моделях.
Написать тесты для класса запросов. Переписать модели с некрасивых конструкторов
на `Pydantic` или что-нибудь ещё. Написать генератор кода для быстрого добавления
новых полей.

## Pull Requests

PR'ы должны быть сделаны в `development` ветку. Определённого шаблона оформления
нет. Если это закрывает какое-то issue, то стоит сослаться на него с ключевым
словом "close". Например, "close #123". Обращайте внимание прошёл ли Ваш PR все
проверки GitHub Actions (тесты, проверка стиля кода).

## Тесты

Для тестирования используется `pytest`. На данный момент отсутствуют 
интеграционные тесты. Поэтому всё что сейчас 
требутеся — это юнит тесты классов-обёрток и их дополнительных методов 
(если имеются), которые не являются сокращениями для методов клиента.

Тесты модели должны покрывать 5 основных вещей: конструктор, десериализацию 
только обязательных полей, десериализацию всех полей, сравнение
объектов модели, десериализацию пустого словаря. По необходимости десериализацию
списка объектов.

Примеры доступны в папке [tests](tests).

## Документация

Для документации используется `Sphinx`. Документация ведется в [google style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html).
К каждому классу и методу должна быть написана документация на русском языке. 
Типы каждого значения. По возможности описано предназначение поля. Если 
невозможно понять — ставим `TODO`. Обязательно отмечаем какие поля являются
опциональными. Пишем заметки по известным значениям полей, чтобы из контекста
догадываться о предназначении.

Если добавлен новый класс, то не забудьте создать `.rst` файл в папке 
[docs/source](docs/source) и добавить его в дерево нужного пакета.

## Форматирование кода (стиль написания)

В проекте используется `black`. Не забывайте перед публикацией
отформатировать код и проверить его на работоспособность.

## Создание новых моделей

Исходите из логики при помещении модели в определённый пакет.
Добавьте свою модель для короткого импорта в одну из секций `__init__.py` файла
и пропишите название в `__all__`. Обязательно следите за тем, какие поля
присутствуют всегда, а какие нет. По возможности минимизируйте количество
опциональных полей. Не забывайте перечислить поля, по которым можно отличить
один объект от другого в `_id_attrs`. Экземпляр класса `Client` передаётся в
каждую модель для возможности написания методов-сокращений. 
