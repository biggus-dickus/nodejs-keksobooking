# Личный проект «Кексобукинг» [![Build status][travis-image]][travis-url]

* Студент: [Макс Масленко](https://up.htmlacademy.ru/nodejs/2/user/107049).
* Наставник: [Вадим Шевяков](https://htmlacademy.ru/profile/id574589).

---

## Быстрый запуск приложения
1. Установить соединение с MongoDB в активной сессии терминала: `npm run db:start`.
2. Заполнить БД тестовыми данными: `npm run db:fill`.
3. Поднять локальный сервер: `npm start`.

## Корректные настройки для `.env` 

```
NODE_ENV=development

SERVER_PORT=3000
SERVER_HOST=localhost

DB_HOST=localhost:27017
DB_NAME=keksobooking
DB_USER=root
DB_PASS=root

SERVER_LOG_LEVEL=info
```

При отсутствии будут использованы значения по умолчанию.

## ТЗ проекта

### Общие положения
Необходимо разработать серверную часть приложения, которая будет состоять из API, предоставляющего доступ к сущностям приложения, статических ресурсов и утилитных команд.

### Статические ресурсы
Если программа запущена из каталога (Current Working Directory), в котором лежит папка `static`, то данные из этой папки должны предоставляться как статические данные в режиме сервера. Если папка отсутствует, то приложение должно стартовать без статических файлов.

Все статические файлы должны быть доступны из корня приложения (`/`) и далее в соответствии с файловой структурой.

### Интерфейс командной строки (CLI)
Приложение должно предоставлять данные о себе и своих возможностях при помощи интерфейса командной строки.

Если приложение запустили без команд, оно должно выводить информацию о том, что это за приложение, какие возможности у него есть, а также инструкцию о том, как его корректно запустить.

**Доступные команды:**

* `--help` — выводит список доступных команд
* `--author` — выводит информацию об авторе приложения
* `--version` — выводит информацию о текущей версии приложения
* `--server [PORT]` — запускает сервер на выбранном порту
* `--fill` — заполняет базу данных тестовыми данными

#### Дополнительно
Вместо текста приветствия предоставляется интерактивный ввод, где можно указать, что сделать с приложением.

### Сущности, описываемые приложением
Приложение описывает предложение на рынке недвижимости и хранит в себе следующую информацию:

```
{
  "author": {
    "name": "Имя автора",
    "avatar": "URL с аватаром пользователя"
  },
  "offer": {
    "title": "Заголовок объявления",
    "description": "Описание",
    "address": "Адрес",
    "price": "Цена предложения",
    "type": "Тип недвижимости",
    "rooms": "Кол-во комнат",
    "guests": "Максимальное кол-во гостей",
    "checkin": "Время заезда",
    "checkout": "Время выезда",
    "features": ["Список доступных удобств"],
    "photos": ["URL с фотографиями предложения"]
  },
  "location": {
    "x": "координата X на карте",
    "y": "координата Y на карте"
  },
  "date": "Дата размещения"
}
```

### Ресурсы
Все ресурсы должны быть доступны по относительному адресу `/api`. Ресурсы должны позволять обращаться к ним всем клиентам при помощи механизма `CORS`.

### Формат данных
Все ресурсы должны поддерживать формат `application/json` с кодировкой `utf-8`.

### Дополнительно
Ресурсы могут поддерживать формат данных `text/html`.

**`GET /api/offers:`**

Параметры:

<table>
  <thead>
    <tr>
      <th>Имя</th>
      <th>Тип</th>
      <th>Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>skip</code></td>
      <td>number</td>
      <td>Кол-во элементов которое нужно пропустить перед показом страницы. Значение по умолчанию — <code>0</code></td>
    </tr>
    <tr>
      <td><code>limit</code></td>
      <td>number</td>
      <td>Максимальное кол-во элементов на странице. Значение по умолчанию — <code>20</code></td>
    </tr>
  </tbody>
</table>


Коды ответов:

<table>
<thead>
  <tr>
    <th>Код ответа</th>
    <th>Описание</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>200 OK</td>
    <td>Всё хорошо</td>
  </tr>
  <tr>
    <td>400 Bad Request</td>
    <td>Неверный запрос. Не указаны обязательные параметры либо значение параметра неверно. (См. <code>некорректные запросы</code>)</td>
  </tr>
  <tr>
    <td>500 Internal Server Error</td>
    <td>Невозможно обработать запрос из-за серверной ошибки. (См. <code>некорректные запросы</code>)</td>
  </tr>
</tbody>
</table>


Пример:

**Request:**

GET /offers

**Response:**

200 OK
Content-Type: application/json; charset=utf-8
Content-Length: ...

```
{
  "data": [{
    "author": {
      "name": "Alex",
      "avatar": "api/offers/1519136255300/avatar"
    },
    "offer": {
      "title": "Уютное гнездышко для молодоженов",
      "address": "102-0082 Tōkyō-to, Chiyoda-ku, Ichibanchō, 14−3",
      "description": "Великолепный таун-хауз в центре Токио.",
      "price": 42000,
      "type": "house",
      "rooms": 3,
      "guests": 6,
      "checkin": "14:00",
      "checkout": "10:00",
      "features": ["wifi", "dishwasher", "parking", "washer"]
    },
    "location": {
      "x": 428,
      "y": 493
    },
    "date": 1519136255373
  },
  {
    "author": {
      "name": "Pavel",
      "avatar": "api/offers/1519136255300/avatar"
    },
    "offer": {
      "title": "Маленькая квартирка рядом с парком",
      "address": "102-0075 Tōkyō-to, Chiyoda-ku, Sanbanchō",
      "description": "Маленькая чистая квратира на краю парка. Без интернета, регистрации и СМС.",
      "price": 30000,
      "type": "flat",
      "rooms": 1,
      "guests": 1,
      "checkin": "9:00",
      "checkout": "7:00",
      "features": ["elevator", "conditioner"]
    },
    "location": {
      "x": 471,
      "y": 545
    },
    "date": 1519136255300
  }],
	"skip": 3,
	"limit": 20,
	"total": 5
}
```

**`GET api/offers/:date`**

Возвращает предложение, у которого `date` равен `:date`.

Параметры: Нет.

Код ответа:

<table>
<thead>
  <tr>
    <th>Код ответа</th>
    <th>Описание</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>200 OK</td>
    <td>Всё хорошо</td>
  </tr>
  <tr>
    <td>404 Not Found</td>
    <td>Предложение не найдено</td>
  </tr>
  <tr>
    <td>500 Internal Server Error</td>
    <td>Невозможно обработать запрос из-за серверной ошибки. (См. <code>некорректные запросы</code>)</td>
  </tr>
</tbody>
</table>

Пример:

**Request:**

GET /offers/1519136255300:

**Response:**

200 OK
Content-Type: application/json; charset=utf-8
Content-Length: ...

```
{
  "author": {
    "name": "Pavel",
    "avatar": "api/offers/1519136255300/avatar"
  },
  "offer": {
    "title": "Маленькая квартирка рядом с парком",
    "address": "471, 545",
    "description": "Маленькая чистая квартира на краю парка. Без интернета, регистрации и СМС.",
    "price": 30000,
    "type": "flat",
    "rooms": 1,
    "guests": 1,
    "checkin": "9:00",
    "checkout": "7:00",
    "features": ["elevator", "conditioner"]
  },
  "location": {
    "x": 471,
    "y": 545
  },
  "date": 1519136255300
}
```

**`POST api/offers`**

Создает новое предложение по переданным параметрам. Принимает на вход данные в формате `application/json, multipart/form-data`.

Если загрузили картинку, но валидация не прошла, картинка не должна показываться и не должна быть доступна.

Параметры: Нет.

<table>
<thead>
  <tr>
    <th>Код ответа</th>
    <th>Описание</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>200 OK</td>
    <td>Всё хорошо</td>
  </tr>
  <tr>
    <td>400 Bad Request</td>
    <td>Если произошла ошибка: данные не прошли валидацию, и т.д. (См. <code>некорректные запросы</code>)</td>
  </tr>
  <tr>
    <td>500 Internal Server Error</td>
    <td>Невозможно обработать запрос из-за серверной ошибки. (См. <code>некорректные запросы</code>)</td>
  </tr>
</tbody>
</table>

Пример: 

**Request:**

POST /offers
Content-Type: application/json
Content-Length: ...

```
{
  "name": "Pavel",
  "title": "Маленькая квартирка рядом с парком",
  "address": "570, 472",
  "description": "Маленькая чистая квратира на краю парка. Без интернета, регистрации и СМС.",
  "price": 30000,
  "type": "flat",
  "rooms": 1,
  "guests": 1,
  "checkin": "9:00",
  "checkout": "7:00",
  "features": ["elevator", "conditioner"]
}
```

**Response:**

200 OK
Content-Type: application/json; charset=utf-8
Content-Length: ...

```
{
  "name": "Pavel",
  "title": "Маленькая квартирка рядом с парком",
  "address": "570, 472",
  "description": "Маленькая чистая квратира на краю парка. Без интернета, регистрации и СМС.",
  "price": 30000,
  "type": "flat",
  "rooms": 1,
  "guests": 1,
  "checkin": "9:00",
  "checkout": "7:00",
  "features": ["elevator", "conditioner"],
  "location": {
    "x": 570,
    "y": 472
  }
}
```

**`GET api/offers/:date/avatar`**

Возвращает `author.avatar` для объявления, у которого `offer.date === :date`

Параметры: Нет.

<table>
<thead>
  <tr>
    <th>Код ответа</th>
    <th>Описание</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>200 OK</td>
    <td>Всё хорошо</td>
  </tr>
  <tr>
    <td>400 Bad Request</td>
    <td>Предложение отсутствует</td>
  </tr>
  <tr>
    <td>404 Not Found</td>
    <td>Предложение не имеет аватара</td>
  </tr>
  <tr>
    <td>500 Internal Server Error</td>
    <td>Невозможно обработать запрос из-за серверной ошибки</td>
  </tr>
</tbody>
</table>

Пример:

**Request:**

GET /offers/1519136255300/avatar

**Response:**

200 OK
Content-Type: image/jpeg
Content-Length: ...
image

### Валидация входных данных

Все сохраняемые данные должны быть проверены на корректность и целостность перед записью в базу данных.

* `title` — обязательное текстовое поле, размером от 30 до 140 символов
* `type` — обязательное текстовое поле, с одним из значений:
  * `flat`,
  * `house`,
  * `bungalow`,
  * `palace`
* `price` — обязательное числовое поле, в интервале от 1 до 100 000
* `address` — обязательное текстовое поле размером до 100 символов. Из поля адреса нужно создавать объект координат `{x: ...,y: ...}` с соотвествующими значениями из поля address
* `checkin` — обязательное текстовое поле в формате `HH:mm`
* `checkout` — обязательное текстовое поле в формате `HH:mm`
* `rooms` — обязательное числовое поле в интервале от 0 до 1000
* `features` — необязательный список доступных услуг, содержит неповторяющиеся значения из перечисленных:
  * `dishwasher`,
  * `elevator`,
  * `conditioner`,
  * `parking`,
  * `washer`,
  * `wifi`
* `avatar` — необязательная картинка профиля пользователя, содержит файл типа картинка (`image/jpg,  image/png...`)
* `preview` — необязательная картинка внешнего вида предложения, содержит файл типа картинка (`image/jpg, image/png...`)
* `name` — необязательное текстовое поле с именем пользователя; если не указано, то принимает случайным образом одно из следующих значений:
  * `Keks`,
  * `Pavel`,
  * `Nikolay`,
  * `Alex`,
  * `Ulyana`,
  * `Anastasyia`,
  * `Julia`

### Некорректные запросы

Все методы, которые не поддерживаются приложением, должны возвращать корректный статус `501`.

Все адреса, по которым нет данных, должны возвращать корректный код ошибки `404`.

Все ошибки со статусом `>400` должны предоставлять в своем теле развёрнутое описание причин возникновения проблемы.

Ошибки `>500` не должны раскрывать части исходного кода, стек вызовов, имена функций и т. д.

Формат ошибок:

```
[
	{
	  "error": "Internal Error",
		"errorMessage": "Server has fallen into unrecoverable problem."
	}
]
```

Пример ошибки валидации (400 Bad Request):

```
[
	{
	  "error": "Validation Error",
		"fieldName": "title",
		"errorMessage": "is required"
	},
	{
	  "error": "Validation Error",
		"fieldName": "type",
		"errorMessage": "is required"
	}
]
```

#### Дополнительно
Отдавать тело ошибки в корректном виде в зависимости от значения заголовка Accept — `text/html` или `application/json`.

### Предлагаемые системы хранения данных
Анализ данных показывает, что все данные в приложении являются атомарными и не имеют ссылок между собой. Состав данных состоит в основном из текстовой информации — документа. На этой основе было принято решение использовать документоориентированную базу данных MongoDB

___

<a href="https://htmlacademy.ru/intensive/nodejs"><img align="left" width="50" height="50" alt="HTML Academy" src="https://up.htmlacademy.ru/static/img/intensive/nodejs/logo-for-github-2.png"></a>

Репозиторий создан для обучения на интенсивном онлайн‑курсе «[Профессиональный Node.js, уровень 1](https://htmlacademy.ru/intensive/nodejs)» от [HTML Academy](https://htmlacademy.ru).

[travis-image]: https://travis-ci.com/htmlacademy-nodejs/107049-keksobooking.svg?branch=master
[travis-url]: https://travis-ci.com/htmlacademy-nodejs/107049-keksobooking
