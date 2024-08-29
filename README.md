# Oiee
Seja bem vindo a mais um tuturial!!!
Hoje vamos fazer essas coisas ai:

## Autenticação de Usuários (single server)

Autenticação é o processo de verificar se alguém é quem diz ser. 

### Autenticação VS Autorização

Autenticação: "Você é quem você diz que é?"

Autorização: "O que você pode fazer?"


# Iniciando um Repositório  no CodeSpace
Crie um Repositório  com o nome "2024-IA24-Autenticacao".

Lembrando: O Repositório  deve ser publico e com o arquivo README incluso.
![.](https://raw.githubusercontent.com/Edson-Edu/2024-IA22-2TRI/main/img/criar_repositorio.PNG)
Agora nessa tela, voce ira clicar no botao `code` > `codespace on main` e depois no ``Simbolo + ``
![.](https://raw.githubusercontent.com/Edson-Edu/2024-IA22-2TRI/main/img/codespace.PNG)

Espere a paginar carregar para continuar os proximos passos. Sua tela devera estar assim:
 ![.](https://raw.githubusercontent.com/Edson-Edu/2024-IA22-2TRI/main/img/tela_inicial.PNG)


# Comece no terminal digitando os seguintes comandos de cada vez:

1)
````javascript
npm init -y
````

2)
````javascript
npm install express cors sqlite3 sqlite
````

3)
````javascript
npm install --save-dev typescript nodemon ts-node @types/express @types/cors
````

4)
````javascript
npx tsc --init
````

5)
````javascript
mkdir src
````

5)
````javascript
touch src/index.ts
````

# Agora no arquivo ``index.ts`` adicione:

````javascript
import express, { application } from 'express'
const app = express()

app.use(express.json())
app.use(express.static(__dirname + '/../public'))

const registredUsers = [ // Lista de Usuarios Registrados
  {
    username: "daniel",
    password: "123123"
  },
  {
    username: "admin",
    password: "admin"
  }
]

const logedUsers: any = [ // Lista de Usuarios Logados

]


app.get("/check/:token", (req, res) => {
  const { token } = req.params
  if (!logedUsers[token])
    return res.status(401).json({ error: "Token inválido" })
  return res.json({ message: "Usuário logado" })
})

app.post("/login", (req, res) => {
  const { username, password } = req.body
  const user = registredUsers.find(user => user.username === username && user.password === password)
  if (!user)
    return res.status(401).json({ error: "Usuário não encontrado" })
  logedUsers.push(user)
  const token = logedUsers.length - 1
  return res.json({ token })
})

app.post("/logout/:token", (req, res) => {
  const { token } = req.params
  if (!logedUsers[token])
    return res.status(401).json({ error: "Token inválido" })
  delete logedUsers[token]
  return res.json({ message: "Usuário deslogado" })
})

app.listen(3000, () => {
  console.log("Server is running on port 3000")
})
````
# Agora crie uma pasta chamada public, e dentro crie um arquivo chamado ``index.html`` e ``main.js``, adicione:

## index.html
````html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>identificação</title>
  <script defer src="main.js"></script>
</head>
<body>
  <form>
    <div>
      <label for="username">username:</label>
      <input type="text" id="username" name="username">
    </div>
    <div>
      <label for="password">password:</label>
      <input type="password" id="password" name="password">
    </div>
    <button type="submit">entrar</button>
  </form>
  <button id="btLogout">sair</button>
</body>
</html>
````

## Main.js
````javascript
const form = document.querySelector('form')
const btLogout = document.querySelector('#btLogout')

console.log(form)

form.addEventListener('submit', async (e) => {
  e.preventDefault()
  const response = await fetch('/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      username: form.username.value,
      password: form.password.value
    })
  })
  const result = await response.json()
  if (response.status === 401) {
    alert('Usuário ou senha inválidos')
    return
  }
  localStorage.setItem('token', result.token)
  console.log(result)
})

btLogout.addEventListener('click', async () => {
  const response = await fetch(`/logout/${localStorage.getItem('token')}`, { method: 'POST' })
  if (response.status == 401)
      return alert("Usuário já não estava logado")
  const result = await response.json()
  console.log(result)
  alert("Usuaário deslogado com sucesso")
})
````

# Agora vamos alterar os arquivos ``package.json`` e ``package-lock.json``, Substitue:

## package.json

````json
{
  "name": "2024-tri2-ia22-autentica-o",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts",
    "build": "tsc -p ."
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.19.2",
    "sqlite": "^5.1.1",
    "sqlite3": "^5.1.7"
  },
  "devDependencies": {
    "@types/cors": "^2.8.17",
    "@types/express": "^4.17.21",
    "nodemon": "^3.1.4",
    "ts-node": "^10.9.2",
    "typescript": "^5.5.4"
  }
}

````

````json

````