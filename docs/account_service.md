<details><summary>POST authorize</summary>

##### URL: 
```
https://{host}/api/{api_version}/account/authorize
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
  'https://{host}/api/{api_version}/account/authorize' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'access_token: {user_access_token} \
  -d '{
    "login": "user login",
    "password": "user password"
  }'
```
##### Реузльтат
Возвращает JSON с access_token и данные пользователя.
```
{
  access_token: string
  User: {
    name: string
  }
}
```
  
</details>

<details><summary>GET user</summary>

##### URL: 
```
https://{host}/api/{api_version}/account
```

##### Описание:
Возвращает данные пользователя.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 

##### Пример использования:
```
curl -X 'GET' \
  'https://{host}/api/{api_version}/account' \
  -H 'accept: application/json' \
  -H 'access_token: {user_access_token} 
```
  
##### Реузльтат
Возвращает JSON с данными пользователя.
```
{
  name: string
}
```

</details>

