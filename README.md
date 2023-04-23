# manticore-test

Работать в консоли cmd или через консоль windows git.
Где-то тут: C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Git Git CMD

Создать / выбрать папку для проекта

например `cd projects/my-project`

В ней сделать

`git clone https://github.com/audetv/manticore-test`

Теперь запустить мантикроу можно так:

`cd manticore-test`

`docker compose up -d`

Остановить

`docker compose down`


### try geomatrix

Мне пришлось изменить наименование полей в source, а именно gh_e1, h_ne2 приставил цифры, иначе наименование полей не уникально и будет ошибка.

Полагаю, что перед запуском теста можно сразу изменить наименования этих полей, на необходимые, но уникальные

Это находится в файле: geomatrix/manticore.conf

В ту же папку geomatrix я положил файл csv, т.е. В папке лежит файл конфигурации мантикоры и csv для индекса. Эти файлы примонтированы в контейнер manticore в docker-compose.yml в секции volumes 


Обязательно останавливаем/удаляем работающий контейнер с мантикорой, можно через интерфейс, можно перед установкой запустить еще и эту команду:

`docker compose down -v --remove-orphans`

Это команда остановит контейнер manticore, удалит данные в volume.

Так же надо обязательно удалить все кроме файла .gitignore, что было ранее в папке manticore. У меня пока я это не сделал были ошибки с индексом.

В итоге внутри папки manticore перед первым запуском должны быть только файл .gitignore и папка data. Если их нет, то **обязательно создать папку data**.

Далее запускаем в консоли команду:

`docker compose run --rm manticore indexer geomatrix`

После этого csv файл должен быть успешно обработан.

Теперь можно запустить контейнер с мантикорой
`docker compose up -d`

И работать с таблицей в интерфейсе докера:

`mysql`

`show tables;`

`select * from geomatrix;`

И из postman:

`POST localhost:9308/search`

```
{
    "index": "geomatrix"
}
```

```
{
    "took": 0,
    "timed_out": false,
    "hits": {
        "total": 200,
        "total_relation": "gte",
        "hits": [
            {
                "_id": "1",
                "_score": 1,
                "_source": {
                    "lat": 49.063217,
                    "lon": 30.032660,
                    "height": 0.000000,
                    "name": "Чернобыль — Кучурган — М0",
                    "description": "description",
                    "geohash": "004hrr",
                    "uuid": "uuid",
                    "gh4": "004h",
                    "gh_n": "004j",
                    "gh_ne": "004m",
                    "gh_e": "004k",
                    "gh_se": "0047",
                    "gh_e1": "0045",
                    "gh_sw": "001g",
                    "gh_w": "001u",
                    "gh_ne2": "001v"
                }
            },
        ...
```
