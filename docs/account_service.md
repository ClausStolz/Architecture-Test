<details><summary>POST authorize</summary>

##### URL: 
```
https://{host}/api/account/authorize
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

##### Коды ошибок:

1. 404 - Пользователя с такими данными не существует;
2. 401 - Неправильный пароль для пользователя.
</details>

<details><summary>GET user</summary>

##### URL: 
```
https://{host}/api/account/
```

##### Описание:
Возвращает данные пользователя.

##### Параметры:
**В `Header` запроса нужно добавить `access_token` пользователя** 
##### Реузльтат
Возвращает JSON с данными пользователя.
```
{
  name: string
}
```

##### Коды ошибок:

1. 401 - `access_token` не является валидным.
</details>

