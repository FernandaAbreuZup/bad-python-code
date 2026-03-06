Especificação Técnica — API de Login em Python
Objetivo
Desenvolver uma API RESTful para autenticação de usuários (login), utilizando Python e o framework Flask.

Requisitos Funcionais
Endpoint de Login

Rota: POST /api/login
Request Body:



{
  "username": "string",
  "password": "string"
}
Validações:
Ambos os campos são obrigatórios.
O usuário deve existir no banco de dados.
A senha deve ser validada de forma segura (hash).
Resposta de Sucesso:
HTTP 200
Retornar um token JWT válido e informações básicas do usuário.



{
  "token": "jwt_token",
  "user": {
    "id": 1,
    "username": "usuario"
  }
}
Resposta de Erro:
HTTP 401 para credenciais inválidas.
HTTP 400 para campos obrigatórios ausentes.
Persistência

Usuários devem estar cadastrados em um banco de dados (pode ser SQLite para desenvolvimento).
Segurança

As senhas devem ser armazenadas com hash seguro (ex: bcrypt).
O token JWT deve ser assinado com segredo seguro.
Requisitos Não Funcionais
Código em Python 3.8+.
Utilizar Flask e Flask-JWT-Extended.
Documentação da API via Swagger/OpenAPI.
Testes unitários para o endpoint de login.
Critérios de Aceite
 O endpoint /api/login deve autenticar usuários corretamente.
 Deve retornar token JWT válido em caso de sucesso.
 Deve retornar mensagens de erro apropriadas para falhas.
 Senhas nunca devem ser retornadas ou expostas.
 Cobertura mínima de testes: 80%.
Exemplo de Implementação (trecho)



from flask import Flask, request, jsonify
from flask_jwt_extended import JWTManager, create_access_token

app = Flask(__name__)
app.config['JWT_SECRET_KEY'] = 'sua_chave_secreta'
jwt = JWTManager(app)

@app.route('/api/login', methods=['POST'])
def login():
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    # Validar usuário e senha (hash)
    # Se válido:
    access_token = create_access_token(identity=username)
    return jsonify(token=access_token, user={"username": username}), 200
    # Se inválido:
    # return jsonify({"msg": "Credenciais inválidas"}), 401
