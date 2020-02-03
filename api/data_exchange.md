# REST-протокол обмена данным с клиентами iClinicApp

## Общие сведения
Клиент = клиент iClinicApp. Медицинская организация, которая пользуется услугами iClinicApp.
Сервер = сервер iClinicApp, с которым взаимодействует мобильное приложение.

Обмен данными с клиентами производится через HTTP REST API. 

Когда данные изменились на стороне клиента - клиент должен отправлять эти изменения на сервер.
Когда данные изменились на стороне сервера (например, юзер отменил запись к врачу) - сервер должен уведомить об этом клиента. 

# Протокол для обмена данными с сервером
Каждый запрос подписывается уникальным токеном, который выдается непосредственно клиенту при регистрации.


## Врачи
### Список всех врачей
* `GET`: `/api/v1/integration/doctos`
* Пример ответа:
```
{
    "count": 13,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 2,
            "full_name": "Иванов Иван Иванович",
            "picture": "link/to/picture",
            "description": "",
            "experience": "",
            "education": "",
            "speciality": "Терапевт",
            "services": [
                {
                    "id": 1,
                    "title": "Услуга 1"
                }
            ],
            "subsidiaries": [
                {
                    "id": 2,
                    "title": "Филиал 1"
                }
            ]
        }
    ]
```
### Отдельный врач
* `GET`: `/api/v1/integration/doctos/<id>` - получить инфу по врачу
* Пример ответа:
```
{
    "id": 2,
    "full_name": "Иванов Иван Иванович",
    "picture": "link/to/picture",
    "description": "",
    "experience": "",
    "education": "",
    "speciality": "Терапевт",
    "services": [
        {
            "id": 1,
            "title": "Услуга 1"
        }
    ],
    "subsidiaries": [
        {
            "id": 2,
            "title": "Филиал 1"
        }
}
```
------------------
* `PUT`/`PATCH`: `/api/v1/integration/doctos/<id>` - обновить инфу по врачу.
* Запрос:
```
curl -X PATCH -H "Content-Type: application/json" -d '{"education":"высшее","description":"Лучший доктор клиники"}' http://localhost:8080/api/v1/integration/doctos/<2>

```
* Пример ответа:
```
{
    "id": 2,
    "full_name": "Иванов Иван Иванович",
    "picture": "link/to/picture",
    "description": "Лучший доктор клиники",
    "experience": "",
    "education": "высшее",
    "speciality": "Терапевт",
    "services": [
        {
            "id": 1,
            "title": "Услуга 1"
        }
    ],
    "subsidiaries": [
        {
            "id": 2,
            "title": "Филиал 1"
        }
}
```


## Услуги

### Список всех услуг
* `GET`: `/api/v1/integration/services`
* Пример ответа:
```
{
  "count": 15,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "title": "Услуга 1",
      "description": "Описание услуги",
      "subsidiaries": [
        {
          "id": 1,
          "title": "Айболит №1, проспект Мира 13",
          "primary_image": null,
          "address": "проспект Мира 13",
          "short_address": "пр-т Мира 13"
        },
        {
          "id": 2,
          "title": "Айболит №2, ул. Пролетарская, 21-12",
          "primary_image": null,
          "address": "ул. Пролетарская, 79-87",
          "short_address": "Пролетарская 79"
        }
      ]
    }
  ]
}
```
### Отдельная услуга
* `GET`: `/api/v1/integration/services/<id>` - получить инфу по услуге
* Пример ответа:
```json5
{
  "id": 1,
  "title": "Услуга 1",
  "description": "Описание услуги",
  "subsidiaries": [
    {
      "id": 1,
      "title": "Айболит №1, проспект Мира 13",
      "primary_image": null,
      "address": "проспект Мира 13",
      "short_address": "пр-т Мира 13"
    },
    {
      "id": 2,
      "title": "Айболит №2, ул. Пролетарская, 21-12",
      "primary_image": null,
      "address": "ул. Пролетарская, 79-87",
      "short_address": "Пролетарская 79"
    }
  ]
}
```
------------------
* `PUT`/`PATCH`: `/api/v1/integration/doctos/<id>` - обновить инфу по врачу.
* Запрос:
```bash
curl -X PATCH -H "Content-Type: application/json" -d '{"description":"Самая крутая услуга по эту сторону Стены"}' http://localhost:8080/api/v1/integration/services/1
```
* Пример ответа:
```json5
{
  "id": 1,
  "title": "Услуга 1",
  "description": "Самая крутая услуга по эту сторону Стены",
  "subsidiaries": [
    {
      "id": 1,
      "title": "Айболит №1, проспект Мира 13",
      "primary_image": null,
      "address": "проспект Мира 13",
      "short_address": "пр-т Мира 13"
    },
    {
      "id": 2,
      "title": "Айболит №2, ул. Пролетарская, 21-12",
      "primary_image": null,
      "address": "ул. Пролетарская, 79-87",
      "short_address": "Пролетарская 79"
    }
  ]
}
```


## Пациенты
### Список
* `GET`: `/api/v1/integration/patients/` - получить список пациентов
* Пример ответа:
```json5
{
  "count": 1,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "profile": {
        "id": 0,
        "full_name": "string",
        "first_name": "string",
        "last_name": "string",
        "patronymic": "string",
        "picture": "string",
        "gender": "man",
        "birth_date": "2020-02-03",
        "is_active": true,
        "is_personal_data_filled": "string",
        "region_as_text": "string"
      },
      "contacts": "string",
      "integration_data": {},
      "phone": "string"
    }
  ]
}
```

### Отдельный пациент
* `GET`: `/api/v1/integration/patients/<id>` - получить инфу по пациенту
* Пример ответа
```json5
{
  "id": 0,
  "profile": {
    "id": 0,
    "full_name": "Иванова Наталья Петровна",
    "first_name": "Наталья",
    "last_name": "Иванова",
    "patronymic": "Петровна",
    "picture": "string",
    "gender": "woman",
    "birth_date": "2020-02-03",
    "is_active": true,
    "is_personal_data_filled": "string",
    "region_as_text": "string"
  },
  "contacts": "string",
  "integration_data": {},
  "phone": "string"
}
```

----------------------------
* `PUT`/`PATCH`: `/api/v1/integration/patients/<id>` - обновить инфу по пациенту.
* Запрос:
```bash
curl -X PATCH -H "Content-Type: application/json" -d '{"last_name":"Сидорова"}' http://localhost:8080/api/v1/integration/patient/1
```
* Ответ:
```json5
{
  "id": 0,
  "profile": {
    "id": 0,
    "full_name": "Сидорова Наталья Петровна",
    "first_name": "Наталья",
    "last_name": "Сидорова",
    "patronymic": "Петровна",
    "picture": "string",
    "gender": "woman",
    "birth_date": "2020-02-03",
    "is_active": true,
    "is_personal_data_filled": "string",
    "region_as_text": "string"
  },
  "contacts": "string",
  "integration_data": {},
  "phone": "string"
}
```

## Записи на прием
### Список
* `GET`: `/api/v1/integration/appointments/`
* фильтрация: `?patient_id=1234&doctor_id=5678`
* Пример ответа:
```json5
{
  "count": 1,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "created": "2020-02-03T14:34:43.773Z",
      "start": "2020-02-03T14:34:43.773Z",
      "end": "2020-02-03T14:34:43.773Z",
      "patient_id": "string",
      "doctor_id": "string",
      "service_id": "string",
      "subsidiary_id": "string"
    }
  ]
}
```

### Создать запись
* `POST`: `/api/v1/integration/appointments/`
* Пример ответа:
```json5
{
  "count": 1,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "created": "2020-02-03T14:34:43.773Z",
      "start": "2020-02-03T14:34:43.773Z",
      "end": "2020-02-03T14:34:43.773Z",
      "patient_id": "string",
      "doctor_id": "string",
      "service_id": "string",
      "subsidiary_id": "string"
    }
  ]
}
```gst