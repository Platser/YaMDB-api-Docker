# YaMDB API (Docker)

## Описание
YaMDB - система сбора отзывов пользователей о фильмах, книгах, музыке и других произведениях.
В данном проекте реализован API для YaMDB.

### Возможности
API предоставляет следующие возможности:
* Регистрация пользователя (с подтверждение по e-mail)
* Аутентификация по JWT-токену
* Добавление/изменение произведений, категорий, жанров (только администраторы)
* Выставление оценок произведениям, написание отзывов произведениям, написание комментариев к отзывам.
* Модерирование отзывов и коментариев (пользователи с ролью модератор)

##№ Технологии
* Python 3.7
* Django 2.2.16
* DRF 3.12.4
* Docker
* Nginx
* PostgreSQL

## Как запустить проект:

Клонировать репозиторий и перейти в корневую директорию проекта:

```
git clone https://github.com/Platser/YaMDB-api-Docker.git
cd YaMDB-api-Docker
```

Создать файл api_yamdb/.env со следующим содержанием:
```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

Собрать и запустить котейнеры Docker:
```
docker-compose -f infra/docker-compose.yaml up -d --build
```

Выполнить миграции баз данных:
```
docker-compose -f infra/docker-compose.yaml exec web python manage.py migrate
```

Создать суперпользователя:
```
docker-compose -f infra/docker-compose.yaml exec web python manage.py createsuperuser
```

Выполнить сбор статических данных:
```
docker-compose -f infra/docker-compose.yaml exec web python manage.py collectstatic --no-input
```

### Решение проблем
На некоторых ПК при работе с GitBash для Windows команда runserver зависает после вывода "Watching for file changes with StatReloader". В этом случае необходимо определить следующую переменную окружения:
```
export PYTHONUNBUFFERED=1
```
Выполнение некоторых команд manage.py при работе с GitBash для Windows приводит к возникновению ошибки, например такой:
```
Superuser creation skipped due to not running in a TTY.
```
В этом случае команду следует выполнять, добавив winpty:
```
winpty python manage.py createsuperuser
```


## Использование

### Подключение и авторизация
Чтобы создать пользователя следует отправить имя пользователя ("username") 
и его адрес электронной почты "email" по адресу /api/v1/auth/signup/.

На электронную почту придет подтверждающий код ("confirmation_code"), который
вместе с именем пользователя для получения токена нужно отправить по адресу 
/api/v1/auth/token/.

Изменение данных в системе доступно только авторизованным пользователям.

### Загрузка первоначальных данных

После запуска сервера можно загрузить первоначальные данные командой:
```
docker-compose -f infra/docker-compose.yaml exec web python manage.py loaddata
```

### Важные эндпоинты
REST API YaMDB позволяет работать: 
* **с произведениями** по адресу /api/v1/titles/;
* **с категориями** по адресу /api/v1/categories/;
* **с жанрами** по адресу /api/v1/genres/;
* **с отзывами** по адресу /api/v1/reviews/;
* **с комментариями к отзывам** по адресу /api/v1/comments/;
* **с web-пфнелью администратора** по адресу /admin;
  
### Примеры API-запросов
Проект сопровождается документацией, которая будет доступна после запуска сервера по адресу: `http://127.0.0.1:8000/redoc/`

## Авторы
Яндекс.Практикум и учебная группа: Денис Малашевич, Герман Милякин и Даниил Яковлев.
Проект YaMDB был создан в качестве группового совместного проекта в рамках учебного курса Python-разработчик Яндекс.Практикум.

## Лицензия 
[Лицензия MIT](https://opensource.org/licenses/MIT)
