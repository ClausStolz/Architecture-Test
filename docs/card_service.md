<details><summary>Get cards</summary>

##### URL: 
```
https://{host}/api/{api_version}/cards
```

##### Описание:
Получение списка карт пользователя.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/cards' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```

##### Реузльтат
Возвращает JSON список карт пользователя.
```
[
 Card: {
  id: integer
  card_number: integer
  card_name: string
  amount: double
  withdrawal_limit: double
  purchase_limit: double
  currency: string
  status: string
 },
 
 ...
 
 Card: {
  id: integer
  card_number: integer
  card_name: string
  amount: double
  withdrawal_limit: double
  purchase_limit: double
  currency: string
  status: string
 }
]
```
  
</details>

<details><summary>Get card</summary>

##### URL: 
```
https://{host}/api/{api_version}/cards/{id}
```

##### Описание:
Возвращает данные карты пользователя.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/cards/{id}' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```

##### Реузльтат
Возвращает JSON список карт пользователя.
```
{
  id: integer
  card_number: integer
  card_name: string
  amount: double
  withdrawal_limit: double
  purchase_limit: double
  currency: string
  status: string
}
```
  
</details>

<details><summary>POST card block</summary>

##### URL: 
```
https://{host}/api/{api_version}/cards/{id}
```

##### Описание:
Регистрирует запрос на блокировку карты.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 
В `Body` нужно передать действие совершаемое с картой, на данный момент доступно действие на блокировку карты.
```
{
  action: string
}
```

##### Пример использования:
```
curl -X 'POST' \
  'https://{host}/api/{api_version}/cards/{id}' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'access_token: {user_access_token} \
  -d '{
    "action": "block"
  }'
```

##### Реузльтат
Возвращает JSON список карт пользователя.
```
{
  id: integer
  card_number: integer
  card_name: string
  amount: double
  withdrawal_limit: double
  purchase_limit: double
  currency: string
  status: string
}
```

</details>
