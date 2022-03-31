# IPFS4Students
Проект по курсу Децентрализованные Системы в НИУ ВШЭ
Project withing the HSE Сourse "Decentralized Systems"

## Краткое описание инфраструктуры
- Хранилище - файлы загружаются в ipfs, он выдает хеши на загруженные в него объекты. 
- Высокодоступная отказоустойчивая база метаданных отвечает за хранение метаданных по загруженным в ipfs объектам.
Эта база хранит хеши и вспомогательную инфу типа формат объекта (видео, аудио, пдф и т.д.), время загрузки, кто загрузил, размер объекта, ключевые слова, описание и т.д. 
- Локальное SQLite хранилище + IPFS PubSub, которые дают возможность узнавать участникам о новых метаданных в IPFS, даже в случае если удаленная база метаданных недоступна.
- Клиент - программа, которая позволяет пользователю загрузить или найти какой-либо объект, загруженный в ipfs.  

## Реализованные компоненты:
1. Компонент, который кладет в ipfs объект и записывает в базу данных метаинфу. У компонента должен быть интерфейс типа /put или соответствующий публичный метод на питоне.
2. Компонент - демон, который работает в фоновом режиме, опрашивает pubsub на наличие новых метаданных, и добавляет их в локальную SQLite, если такие имеются. 
3. База метаданных с репликацией. У базы (PostgreSQL) SQL интерфейс типа SELECT/INSERT INTO запрос.
4. Часть CLI клиента, которая отвечает за запрос на загрузку в ipfs объекта, то есть обрабатывает инпут от клиента и формирует соответствующий payload в 1ую компоненту.
5. Часть CLI клиента, отвечающая за перевод поискового запроса пользователя в запрос в базу метаданных и выдачу ему результатов (ссылок, по которым, он может из ipfs это все скачать)

## Использование:
```python
# background daemon
python ./updater.py &
# upload
python ./main.py upload file_examples/sample.mp4 --format "notes" --tags "tag1,tag2,tag3"
# search
python ./main.py search --search "project" --type "notes"
```
