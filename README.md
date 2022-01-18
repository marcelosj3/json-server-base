# Documentação API

Repositório para cadastro de pessoas, hobbies e estudos.

## Endpoints

As endpoints abaixo podem ser utilizadas na criação, exclusão e edição de usuários, hobbies e estudos.
Todas as endpoints precisam da baseURL especificada a seguir.

baseURL: https://json-server-5b11.herokuapp.com

---

---

## ** Endpoints sem a necessidade de autorização **

### CADASTRO

`BODY`

```
{
	"email": "seuemail@email.com",
	"password": "123456",
	"name": "Fulano",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 201`

```
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1hcmNlbG9Aa2VuemllLmNvbSIsImlhdCI6MTY0MjQ1ODc0MywiZXhwIjoxNjQyNDYyMzQzLCJzdWIiOiIyIn0.IrV6pp4wgQzgxKcpK_xSTnaOZq5H_mIEgxAXt7tw68M",
  "user": {
    "email": "seuemail@email.com",
    "name": "Fulano",
    "id": 1
  }
}
```

### POSSÍVEIS ERROS

\*\* Escrever um **_BODY_** sem e-mail ou senha.

`BODY`

```
{
  "password": "123456",
  "name": "Fulano",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 400`

```
"Email and password are required"
```

\*\* Escrever um **_BODY_** com a senha menor que 4 caracteres.

`BODY`

```
{
  "email": "marcelo23@kenzie.com",
  "password": "123",
  "name": "Fulano",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 400`

```
"Password is too short"
```

---

### LOGIN

`BODY`

```
{
  "email": "seuemail@mail.com",
  "password": "suasenha"
}
```

`POST /login - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1hcmNlbG9Aa2VuemllLmNvbSIsImlhdCI6MTY0MjQ1ODc0MywiZXhwIjoxNjQyNDYyMzQzLCJzdWIiOiIyIn0.IrV6pp4wgQzgxKcpK_xSTnaOZq5H_mIEgxAXt7tw68M",
  "user": {
    "email": "seuemail@email.com",
    "name": "Fulano",
    "id": 1
  }
}
```

### POSSÍVEIS ERROS

\*\* Escrever um **_BODY_** sem e-mail ou senha.

`BODY`

```
{
  "password": "123456",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 400`

```
"Email and password are required"
```

\*\* Escrever um **_BODY_** com um e-mail inexistente.

`BODY`

```
{
  "email": "unexistentuser@mail.com",
  "password": "123456",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 400`

```
"Cannot find user"
```

\*\* Escrever um **_BODY_** com a senha incorreta.

`BODY`

```
{
  "email": "unexistentuser@mail.com",
  "password": "123456",
}
```

`POST /users - FORMATO DA RESPOSTA - STATUS: 400`

```
"Incorrect password"
```

---

### ALL STUDIES

`GET /studies - FORMATO DA RESPOSTA - STATUS: 200`

```
[
  {
    "class": "FullStack Kenzie Academy",
    "institution": "Kenzie Academy",
    "how_many_months": 12,
    "actual_month": 5,
    "userId": 1,
    "id": 1
  },
  {
    "class": "FullStack Kenzie Academy",
    "institution": "Kenzie Academy",
    "how_many_months": 12,
    "actual_month": 5,
    "userId": 2,
    "id": 2
  }
]
```

---

### USER STUDIES

Substituir **_USERID_** pelo ID do usuário.

`GET /user/USERID/studies - FORMATO DA RESPOSTA - STATUS: 200`

```
[
  {
    "class": "FullStack Kenzie Academy",
    "institution": "Kenzie Academy",
    "how_many_months": 12,
    "actual_month": 5,
    "userId": 1,
    "id": 1
  }
]
```

**OBSERVAÇÃO**: Caso o **_USERID_** não exista ou não possua nenhum estudo.

`GET /user/USERID/studies - FORMATO DA RESPOSTA - STATUS: 200`

```
[]
```

---

---

## ** Endpoints com a necessidade de autorização **

Para a parte de autorização pode-se utilizar o header abaixo na hora das requisições.

`HEADER DE AUTORIZAÇÃO`

```
{
  "Content-Type': "application/json",
  "Authorization': `Bearer ${token}`
}
```

Caso não utilize o header:

`FORMATO DA RESPOSTA - STATUS 401`

```
"Missing authorization header"
```

### USER INFORMATION

Substituir **_USERID_** pelo ID do usuário.

`GET /users/USERID?\_embed=hobbies&\_embed=studies - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "email": "seuemail@email.com",
  "password": "$2a$10$qZKOZqXxYBLgkzLsYmrz5.krLXfkPCQTHM5Yk91uyuV5q3h2E4Q6u",
  "name": "Fulano",
  "age": 24,
  "id": 1,
  "hobbies": [
    {
      "name": "Andar de bicicleta",
      "difficulty": "Média",
      "userId": 1,
      "id": 1
    }
  ],
  "studies": [
    {
      "class": "FullStack Kenzie Academy",
      "institution": "Kenzie Academy",
      "how_many_months": 12,
      "actual_month": 5,
      "userId": 1,
      "id": 1
    },
  ]
}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_USERID_** sem informações

`GET /users/USERID?\_embed=hobbies&\_embed=studies - FORMATO DA RESPOSTA - STATUS: 404`

```
{}
```

---

### DELETE USER

Substituir **_USERID_** pelo ID do usuário.

`DELETE /users/USERID - FORMATO DA RESPOSTA - STATUS: 200`

```
{}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_USERID_** que não exista ou que seja diferente do usuário atual

`DELETE /users/USERID - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource access: entity must have a reference to the owner id"
```

---

### ALL HOBBIES

`GET /hobbies - FORMATO DA RESPOSTA - STATUS: 200`

```
[
  {
    "name": "Andar de bicicleta",
    "difficulty": "Média",
    "userId": 1,
    "id": 1
  },
  {
    "name": "Tocar guitarra",
    "difficulty": "Difícil",
    "userId": 2,
    "id": 2
  }
]
```

---

### USER HOBBIES

Substituir **_USERID_** pelo ID do usuário.

`GET /hobbies?userId=USERID - FORMATO DA RESPOSTA - STATUS: 200`

```
[
  {
    "name": "Andar de bicicleta",
    "difficulty": "Média",
    "userId": 1,
    "id": 1
  },
  {
    "name": "Tocar guitarra",
    "difficulty": "Difícil",
    "userId": 2,
    "id": 2
  }
]
```

**OBSERVAÇÃO**: Caso o **_USERID_** não exista ou não possua nenhum hobbie.

`GET /hobbies?userId=USERID - FORMATO DA RESPOSTA - STATUS: 200`

```
[]
```

---

### NEW HOBBIE

`BODY`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Média",
  "userId": 1
}
```

`POST /hobbies - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Média",
  "userId": 1,
  "id": 1
}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_userId_** que não exista, ou que seja diferente do usuário atual

`BODY`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Média",
  "userId": 2
}
```

`POST /hobbies - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource creation: request body must have a reference to the owner id"
```

---

### UPDATE HOBBIE

`BODY`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Fácil"
}
```

Substituir **_HOBBIEID_** pelo ID do hobbie.

`PATCH /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Fácil"
}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_HOBBIED_** que não exista.

`BODY`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Fácil"
}
```

Substituir **_HOBBIEID_** pelo ID do hobbie.

`PATCH /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 401`

```
"Cannot read property 'userId' of undefined"
```

\*\* Utilizar um **_HOBBIEID_** que não tenha relação com o usuário que o criou.

`BODY`

```
{
  "name": "Andar de bicicleta",
  "difficulty": "Fácil"
}
```

Substituir **_HOBBIEID_** pelo ID do hobbie.

`PATCH /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource access: entity must have a reference to the owner id"
```

---

### DELETE HOBBIE

Substituir **_HOBBIEID_** pelo ID do hobbie.

`DELETE /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 200`

```
{}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_HOBBIED_** que não exista.

Substituir **_HOBBIEID_** pelo ID do hobbie.

`POST /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 401`

```
"Cannot read property 'userId' of undefined"
```

\*\* Utilizar um **_HOBBIEID_** que não tenha relação com o usuário que o criou.

Substituir **_HOBBIEID_** pelo ID do hobbie.

`POST /hobbies/HOBBIEID - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource access: entity must have a reference to the owner id"
```

---

### NEW STUDY

`BODY`

```
{
  "class": "FullStack Kenzie Academy",
  "institution": "Kenzie Academy",
  "how_many_months": 12,
  "actual_month": 5,
  "userId": 1
}
```

`POST /studies - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "class": "FullStack Kenzie Academy",
  "institution": "Kenzie Academy",
  "how_many_months": 12,
  "actual_month": 5,
  "userId": 1,
  "id": 1
}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_userId_** que não exista ou que não seja o do criador do estudo.

`BODY`

```
{
  "class": "FullStack Kenzie Academy",
  "institution": "Kenzie Academy",
  "how_many_months": 12,
  "actual_month": 5,
  "userId": 2
}
```

`POST /studies - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource creation: request body must have a reference to the owner id"
```

### UPDATE STUDY

**OBSERVAÇÃO**: Você pode incluir o **_userId_** no body e substituir o valor dele por outro, mas fazendo isso, o usuário não consegue atualizar ou excluir o estudo atual, visto que apenas o usuário com o mesmo id do **_userId_** pode realizar essas tarefas.

`BODY`

```
{
  "actual_month": 8,
}
```

Substituir o **_STUDYID_** pelo id do estudo. Repare que você adicionar novas linhas e substituir o valor das chaves já passadas, com exceção do id.

`PATCH /studies/STUDYID - FORMATO DA RESPOSTA - STATUS: 200`

```
{
  "class": "FullStack Kenzie Academy",
  "institution": "Kenzie Academy",
  "how_many_months": 12,
  "actual_month": 8,
  "userId": 1,
  "id": 1
}
```

### POSSÍVEIS ERROS

\*\* Tentando atualizar um **_STUDYID_** que não possua o **_userId_** do usuário atual.

`BODY`

```
{
  "actual_month": 10,
}
```

`PATCH /studies - FORMATO DA RESPOSTA - STATUS: 401`

```
"Private resource access: entity must have a reference to the owner id"
```

\*\* Tentando atualizar um **_STUDYID_** que não exista.

`BODY`

```
{
  "actual_month": 8,
}
```

`PATCH /studies - FORMATO DA RESPOSTA - STATUS: 401`

```
"Cannot read property 'userId' of undefined"
```

---

### DELETE STUDY

Substituir **_STUDYID_** pelo ID do estudo.

`DELETE /hobbies/STUDYID - FORMATO DA RESPOSTA - STATUS: 200`

```
{}
```

### POSSÍVEIS ERROS

\*\* Utilizar um **_STUDYID_** que não exista.

Substituir **_STUDYID_** pelo ID do estudo.

`POST /hobbies/STUDYID - FORMATO DA RESPOSTA - STATUS: 401`

```
"Cannot read property 'userId' of undefined"
```

\*\* Utilizar um **_STUDYID_** que não tenha relação com o usuário que o criou.

Substituir **_STUDYID_** pelo ID do estudo.

`POST /hobbies/STUDYID - FORMATO DA RESPOSTA - STATUS: 403`

```
"Private resource access: entity must have a reference to the owner id"
```

---
