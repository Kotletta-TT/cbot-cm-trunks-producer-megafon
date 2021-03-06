# cbot-cm-trunks-producer-megafon

Сервис-бот собирает статистику по бизнес SIM-картам провайдеров подключенных к 
ВирутальнойАТС (пока только Мегафон) и передает в очередь RabbitMQ

### Для запуска необходимо:


1. Заполнить файл `config.py`, на данный момент там приведен образец:

```
DATA_ACCESS = [
    {'address': 'vatsXXXXXX.megapbx.ru',
                'login': 'admin@vatsXXXXXX.megapbx.ru',
                'password': 'some_password',
                'name': "любое имя",
                'provider': 'Megafon'},]
```
В списке может быть любое количество контрактов в виде python-словарей

2. Docker:  
   При использовании Docker, в Dokerfile изменить переменные виртуального окружения:  
   - `RABBIT_HOST=адрес сервера RabbitMQ`  
   - `RABBIT_QUEUE=имя очереди`
   - `LOG_LEVEL=уровень логирования (INFO, DEBUG, WARNING...)` - по умолчанию WARNING
   - `TIMEOUT_REQUEST=таймаут между запросами к API провайдеров`  - по умолчанию 0
   Собрать `docker-build -t trunks-producer-build .`  
   Запустить `docker run -it --rm --name trunks-producer-bot trunks-producer-build`  
    
3. Локальный запуск:  
    Возможен, но не рекомендуется, действия для запуска:  
     - Создать виртуальное окружение Python `python3 -m venv venv`
     - Перейти в данное виртуальное окружение для Linux: `source venv/bin/activate`
     - Установить зависимости `pip install -r requirements.txt`  
     - Создать переменные в виртуальном окружении, как в пункте 2,  
       пример для Linux: `export RABBIT_HOST=localhost`
     - Запустить `python3 ./main.py`


### Применение:
- В основном пока для мониторинга транков, и оперативного реагирования через систему очередей

### В планах:

1. Ассинхронность/многопоточность запросов к API (Простите операторы)  
2. Универсальность (Возможность опрашивать разных операторов)

   