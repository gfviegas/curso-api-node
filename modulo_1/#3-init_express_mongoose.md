# Mãos a Obra!

Agora que temos nosso ambiente configurado os nossos packages essenciais instalados está na hora de começarmos nossa API. Vamos iniciar criando uma estrutura de pastas

## Inicializando o express, server e mongoose
Vamos começar com o entry point da nossa aplicação. Esse arquivo vai ser o responsável por inicializar o express e mongoose, inicializar nossas rotas e servidor.

Vamos começar criando ele na raiz de nosso projeto:

```javascript
require('dotenv').config()
require('./config/db')
const express = require('express')
const bodyParser = require('body-parser')
const logger = require('morgan')
const cors = require('cors')
const validator = require('express-validator')
const app = express()
const server = require('./config/server')
```
Antes de mais nada, damos um require no dotenv e carregamos nossas variáveis de ambiente. Para isso, temos que criar um arquivo .env na raiz do projeto:
```
DB_HOST = mongodb://localhost/curso-api
```
Esse arquivo define variáveis de ambiente e não devemos verionar ele, uma vez que ele vai abrigar os secrets da nossa aplicação, então não esqueça de o adicionar no .gitignore. Logo depois carregamos o nosso arquivo `./config/db.js` que inicializa a nossa conexão com o mongoose, utilizando o `process.env.DB_HOST` que se equivale ao valor definido a DB_HOST no nosso .env, massa né?

```javascript
const mongoose = require('mongoose')
mongoose.connect(process.env.DB_HOST)
```

Logo de cara já vimos uma novidade do ES6 que muitos não devem estar familiarizados. É o `const`. Ele, ao contrário do que muitos pensam, não cria uma variável imutável. Você pode declarar uma `const x = {}` e associar `x.foo = 'bar'` sem problemas. Com const você não pode é reatribuir um valor a uma variável, o que é ótimo na maioria dos cenários. Caso você precise reatribuir, use o `let`, que declara uma variavel somente no escopo do bloco. Não utilize `var`.

Continuando, importamos as nossas dependências e declaramos `app = express()` que cria uma aplicação express.

Agora que temos o nosso framework inicalizado, também [carregamos alguns middlewares](http://expressjs.com/en/api.html#app.use) ao express:

```javascript
app.use(logger('dev'))
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: false }))
app.use(cors())
app.use(validator())
app.use((req, res, next) => {
  res.setHeader('Content-Type', 'application/json')
  next()
})
```
Em seguida incializamos nosso server com `server.start(app)`. Este método vive no arquivo `config/server.js` e recebe a instância do nosso app express para inicializar o servidor. Bem clean.

a

a

a

a


a

a
