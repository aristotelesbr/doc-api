# API DOC

Para mais detalhes sobre o processo de autenticação segue o link da documentação oficial:
https://github.com/lynndylanhurley/devise_token_auth

API BASE
http://api.hemopimanager.com

# Cadastro
Enviando uma requisição do tipo POST para a URL de "/auth"

# Hearders
Content-Type 	-> application/json
Accept		    -> application/vnd.hemopimanager.v1


# Pré cadastro

POST http://api.hemopi-manager/auth
```
{
    "email": "johndoe@example.org",
    "password": "123456",
    "password_confirmation": "123456",
    "first_name" : "John"
    "last_name" : "Doe"
    "code" : "123456"
    "login" : "0425547885474" CPF
}
```
# Pós Cadastro
```
"login": "025888745996", # This is CPF
"rg": "2585222 SSP PI",
"city_id": "1",
"state_id": "1",
"gender": "M" or "F",
"phone": "3220-1606",
"aditional_phone": "3220-1606",
"street": "street",
"district": "Saci",
"zip_code": "64000000",
"street_number": "11",
"birth_date" : "29-02-1988",
"contact" "{ 
	phone: '86 3333-2544', 
	phone: '86 99988-7744', 
	....
}",
"image" : "loading..."
```
Um novo token é retornado a cada requisição, salvo em casos de requisições em lote, a mesma, tem um tempo de 0.5s
Exemplo dos HEADRS:
```
[
	{
		"key":"client","value":"uddjuoggIQfdbm_xIZuJuw"
	},{
		"key":"uid","value":"johndoe@example.org"
	},{
		"key":"access-token","value":"kIIqKlZPXEgOWpT-nWKA4w"
	}
]
```

# Iniciando uma sessão
Enviando uma requisição POST para a URL de "/auth/sign_in" com uma chave user passando email e senha.
Ex:
```
  {
    "email": "test@example.org",
    "password": "123456"
  }
```
# Encerrando uma sessão:

Enviando uma requisição DELETE para "/sign_out" passanado o uid, client, e o access-token no headers.

Atualizando dados do usuário
PUT http://BASE_API/auth

- No body a senha atual deve ser passada ao atributo "current_password" com o password do usuario da "sessão" atual e os novos atributos de atualização.


# Agendamento

## Create

O usuário deverá enviar uma requisição do tipo POST para '/schedules' com um objeto "schedule" contendo os seguintes atributos:

  -schedule_date (do tipo Date)
  -shcedule_hour (do tipo String)
  -turn          (do tipo Int)
  -user_id       (do tipo Int)


## Update

Requisição dos tipo PUT para '/schedules/:id' com um objeto "schedule" com o "id" e os atributos a serem atualizados.

## Destroy

Requisição dos tipo DELETE para '/schedules/:id' com um objeto "schedule" com o "id" do objeto a ser excluido.

## Show

Requisição dos tipo GET para '/schedules/:id' com um objeto "schedule" contendo o "id" do objeto a ser localizado.

# Consultar disponibilidade
Requisição do tipo GET para '/schedules' passando uma 'query string' com os parametros 'schedule_date' e 'turn'

Ex:
  http://api.IP/schedules?schedule_date=09-10-2017&turn=1

"turn" recebe um inteiro onde 1 compreende o período de 7:00h às 11:59h e 2 de 12:00h às 17:00h
respectivamente "Manhã" e "Tarde" 
A resposta será uma hash com um atributo "available" com valor "true" quando houver disponibilidade
e "false" quando não.


# Estadados
GET para http://api.hemopi/states

	Será renderizado um json com uma listagem de todos os estados.


# Cidades
GET http://api.hemopi-manager/states/:id

Para ver cada cidade corespondente a cada estado basta passar o estate_id na proxima requisição.
Sera retornado um chave "Included contendo os dados."

Ex:
```
{
    "data": {
        ....
    "included": [
        {
            "id": "22",
            "type": "cities",
            "attributes": {
                "name": "Xapuri",
                "state-id": 1
            },
            "relationships": {
                "state": {
                    "data": {
                        "id": "1",
                        "type": "states"
                    }
                }
            }
        },
        ....
    }
}]
```
