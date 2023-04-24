# Задача
На основе предоставленной навигационной схемой приложения для работы с корпоративными картами разработать API.
Система не должна быть интегрирована ни с какими внешними продуктами, разработка ведется целиком с нуля, внешние данные импортируются сразу в БД.

## Навигационная схема:
https://www.dropbox.com/s/22ps1w1j5uou6rp/sber_corp_proto_v1.1.pdf?dl=0

Считаю, что решение данной задачи должно быть более комплексным, поэтому дополнительно выполнил разработку диаграмм архитектуры и самой БД. Для каждого пункта оставил описание, почему было принято то или иное решение.

## Диаграмма архитектуры:
![arch](https://github.com/ClausStolz/Architecture-Test/blob/main/img/architecture.png)
Предлагаю использовать в качестве архитектуры распределенный монолит, так как по условиям задачи данные приходят в БД извне и для избегания связности между базами данных для каждого микросервиса придется делать отдельный сервис корректно импортирующий данные, либо производить изменения в логике получения данных извне. Для сбора оценок использовать сервис статиски, который хранит данные в ClickHouse, так как он ориентирован на чтение данных, что ускорит процесс сбора аналитики и для дальнейшего масштабирования функционала. 

В связи с нехваткой данных об инфраструктуры компании не прибегал к использованию докернизацию, но при необходимости возможна доработка архитектуры с использованием Cubernetes и выносом некоторых сервисов в отдельные контенеры с последующей балансировкой нагрузки.

Для отображения статистики предлагаю использовать Graffana, для логирования использовать Elastichsearch. В рамках задачи сказано, что система не должна быть интегрирована с внешними продуктами, а данные сервисы можно развернуть непосредственно вместе с системой избегая использования их как Saas.

## Диаграмма БД:
Основная БД
![db](https://github.com/ClausStolz/Architecture-Test/blob/main/img/database.png)

БД для статистики
![statistic]()

 
## Документация API:

API возвращает JSON ответ в формате:
```
{
  "status_code" int,
  response: {
    ...
  }
}
```
В поле `response` будет сущность либо данные об ошибке.

- [Account service](###Account)
- [Card service](docs/card_service.md)
- [Operation service](docs/operation_service.md)
- [Appeal service](docs/appeal_service.md)
- [Statistic service](docs/statistic_service.md)


### Account
#### POST LOGIN
##### URL: 
```
https://{host}/api/{api_version}/users/login
```

##### Описание:
Авторизация пользователя по логину и паролю. Если авторизация прошла успешно возвращает ***access_token*** пользователя.

##### Параметры:
```
{
  login: string,
  password: string
}
```

##### Пример использования:
```
curl -X 'POST' \
  'https://{host}/api/{api_version}/users/login' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'access_token: {user_access_token} \
  -d '{
    "login": "{user_login}",
    "password": "{user_password}"
  }'
```
##### Реузльтат
Возвращает access_token пользователя.
```
{
  "status_code": int,
  "response": 
  {
    "access_token": string
  }
}
```

#### GET LOGOUT
##### URL: 
```
https://{host}/api/{api_version}/users/{user_id}/logout
```

##### Описание:
Производит выход пользователя из системы.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/users/{user_id}/logout' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```
  
##### Реузльтат
Возвращает сообщения о выполнении операции.
```
{
  "status_code": int,
  "response": {
    "message": string
  }
}
```

#### GET REFRESH TOKEN
##### URL: 
```
https://{host}/api/{api_version}/users/refresh
```

##### Описание:
Обновляет время жизни `access_token`.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/users/refresh' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```
  
##### Реузльтат
Возвращает обновленный `access_token`.
```
{
  "status_code": int,
  "response": {
    "access_token": string
  }
}
```

#### GET USER
##### URL: 
```
https://{host}/api/{api_version}/account/{account_id}
```

##### Описание:
Возвращает данные пользователя.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/account/{account_id}' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```
  
##### Реузльтат
Возвращает данныe пользователя.
```
{
  "status_code": int,
  "response":
  {
    "id": int,
    "name": string
}
```

### Cards
