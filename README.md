# api_yatube
API социальной сети [YaTube](https://catiska.pythonanywhere.com/)
### Технологии
Python 3.9,
Django 3.2,
DRF
### Запуск проекта в dev-режиме
- Склонируйте к себе репозиторий:
```
git clone https://github.com/Catiska/api_yatube
``` 

- Переименуйте файл .env.example в .env и вставьте 
ваш секретный ключ:
```
# /.env
SECRET_KEY = 'put_your_secret_key_here'
```
- Установите и активируйте виртуальное окружение:
```
python3 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
```
- Установите зависимости из файла requirements.txt:
```
pip install -r requirements.txt
``` 

- Примените миграции в папке с файлом manage.py:
```
python3 manage.py migrate
```

- В папке с файлом manage.py выполните команду:
```
python3 manage.py runserver
```

## Примеры работы с API
### Для неавторизованных пользователей
Для неавторизованных пользователей доступен только режим чтения.
Получите токен по адресу ```/api/v1/jwt/create/``` через POST-запрос, 
предоставив свои данные:
```
{
    "username": "your_name",
    "password": "your_password"
}
```
Используйте его в заголовке хедера с приставкой Bearer.
- Получение списка публикаций, групп и просмотр комментариев:
```angular2html
GET api/v1/posts/ - получить список всех публикаций.
GET api/v1/posts/{post_id}/ - получение публикации по id

GET api/v1/groups/ - получение списка доступных групп
GET api/v1/groups/{group_id}/ - получение информации о группе по id

GET api/v1/{post_id}/comments/ - получение всех комментариев к публикации
GET api/v1/{post_id}/comments/{comment_id}/ - Получение комментария к публикации по id
```

### Для авторизованных пользователей
Для авторизованных пользователей доступны режимы чтения, публикации, 
изменения и удаления своих публикаций и комментариев.
#### Примеры использования API
- Получение списка публикаций, групп и просмотр комментариев такой же, как и 
для неавторизованных пользователей. Дополнительно авторизованным пользователям 
доступна страница подписок:
```angular2html
GET api/v1/follow/ - получить список подписок.
```
- Подписаться:
```angular2html
POST api/v1/follow/
```
Пример body: {"following": "string"}
- Для создания публикации:
```
POST api/v1/posts/ - создание публикации
POST api/v1/posts/{post_id}/comments/{comment_id}/ - создание комментария
```
Пример body:

  для создания публикации - { "text": "your_text", "image": "string", "group": {group_id} }. Поля 
"image" и "group" не являются обязательными.

  для создания комментария - { "text": "your_text"}
- Для обновления публикации:
```
PUT api/v1/posts/{post_id}/
```
Пример body:
{ "text": "your_text", "image": "string", "group": {group_id} }.
- Для частичного обновления публикации:
```
PATCH api/v1/posts/{post_id}/
```
Пример body для изменения группы:
{ "group": {group_id} } .

- Для удаления:
```
DELETE api/v1/posts/{post_id}/ - удаление публикации
DELETE api/v1/posts/{post_id}/comments/{comment_id}/ - удаление комментария к публикации
```
#### В проекте API доступна пагинация
Пример пагинации на 10 постов, начиная с первого:
```
GET /api/v1/posts/?limit=10&offset=0
```
### Создание и обновление групп возможно через админ-панель Django
Перейдите по адресу 
```/admin```, войдите в систему
и сделайте все необходимые изменения в разделе Groups.
