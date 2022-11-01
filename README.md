# api_yamdb

## Описание
Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории, список категорий может быть расширен администратором.

## Технологии
* Python 3.7
* Django 2.2.16
* DRF 3.12.4
* Docker
* Nginx
* PostgreSQL

## Как запустить проект:

Склонировать из репозитория, затем перейти в каталог:

```
cd infra_sp2
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

## Использование

Детальная информация по работе с системой описана в документации.

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

### Примеры работы 
REST API YaMDB позволяет работать: 
* **с произведениями** по адресу /api/v1/titles/;
* **с категориями** по адресу /api/v1/categories/;
* **с жанрами** по адресу /api/v1/genres/;
* **с отзывами** по адресу /api/v1/reviews/;
* **с комментариями к отзывам** по адресу /api/v1/comments/;
  
Подробности описаны в документации по адресу /redoc/, которая будет доступна
после запуска сервера.


## Авторы
Яндекс.Практикум и учебная группа: Денис Малашевич, Герман Милякин и Даниил Яковлев.

## Лицензия 
[Лицензия MIT](https://opensource.org/licenses/MIT)
