# yamdb_final

![example workflow](https://github.com/wiky-avis/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

# REST API Yamdb – база отзывов пользователей о произведениях.

## Стек: 
Python 3, Django 3, Django REST Framework, Docker, PostgreSQL, Simple-JWT, GIT.

## Описание:
Реализован пользовательский функционал дающий возможность пользоваться приложением не посещая сайт:
1.	Пользовательские роли:
   * Аноним — может просматривать описания произведений, читать отзывы и комментарии.
   * Аутентифицированный пользователь (user)— может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.
   * Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.
   * Администратор (admin) — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
   * Администратор Django — те же права, что и у роли Администратор.
2.	Система регистрации пользователей:
   * Пользователь отправляет POST-запрос с параметром email на /api/v1/auth/email/.
   * YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
   * Пользователь отправляет POST-запрос с параметрами email и confirmation_code на /api/v1/auth/token/, в ответе на запрос ему приходит token(JWT-токен).
   * После регистрации и получения токена пользователь может отправить PATCH-запрос на /api/v1/users/me/ и заполнить поля в своём профайле.
3.	Ресурсы API YaMDb:
   * Ресурс AUTH: аутентификация.
   * Ресурс USERS: пользователи.
   * Ресурс TITLES: произведения, к которым можно написать отзыв.
   * Ресурс CATEGORIES: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
   * Ресурс GENRES: жанры произведений.
   * Ресурс REVIEWS: отзывы на произведения.
   * Ресурс COMMENTS: комментарии к отзывам.

Документация к API доступна по адресу http://localhost:8000/redoc/

## Установка:
Для работы приложения требуется установка на ваш компьютер [Python](https://www.python.org/downloads/), [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows), [PostgreSQL](https://postgrespro.ru/windows).

Склонируйте репозиторий на локальную машину:

  `git clone https://github.com/wiky-avis/yamdb_final.git`

Запустите docker-compose:

  `docker-compose up -d`

Применените миграции базы данных:

  `docker-compose exec web python manage.py migrate --noinput`

Сбор статических файлов, если вдруг статика не подгрузилась:

  `docker-compose exec web python manage.py collectstatic --no-input`
  
Проект запущен и доступен по адресу [http://localhost/](http://localhost/).

Создаем суперпользователя:

  `docker-compose exec web python manage.py createsuperuser`

Заполнения базы начальными данными:

  `docker-compose exec web python manage.py loaddata fixtures.json`

Остановить все запущенные контейнеры:

  `docker-compose down`

API доступен по адресу [http://localhost/api/v1/](http://localhost/api/v1/).

Скачать образ YaMDb из репозитория на DockerHub:

  `docker pull 79452165/yamdb_final:v1`

Также настроен workflow для автоматического разворачивания на сервере.
