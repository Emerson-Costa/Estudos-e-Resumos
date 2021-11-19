# Node JS - API REST

1 - Instalar o express, um framework para criar servidores, rotas etc...



2 - Criar um arquivo dentro do projeto 'server.js'

```javascript
const express = require('express')

const app = express()

const PORT = 3000

app.listen(PORT, () => console.log('Servidor Rodando na Porta 3000'))

/* Criando Rotas */

app.get('/', (req, res) => res.send('Servidor rodando, tudo ok')) /*Rota Raíz do projeto*/

app.get('/atendimentos', (req, res) => res.send('Você está na rota atendimentos'))


```



3 - Criar uma pasta controllers para fazer o gerenciamento dos serviços da API, e como exemplo criar um arquivo chamado 'atendimentos.js'

Instalar uma lib chamada 'consign', que faz o agrupamento das rotas dentro do app

arquivo server.js (pasta raiz do projeto)

```javascript

const express = require('express')
const consign = require('consign')

const app = express()
/* Todo controller que for adicionado dentro do diretório controllers, vão ser invocados pelo app*/
consign()
	.include('controllers')
	.into(app)

const PORT = 3000
app.listen(PORT, () => console.log('Servidor Rodando na Porta 3000'))
```



atendimentos.js (pasta controllers)

```javascript

module.exports = app => {
	app.get('/atendimentos', (req, res) => res.send('Você está na rota atendimentos'))
}

```



4 - Criar uma pasta config e um arquivo chamado 'customExpress.js' onde serão armazenados todos os arquivos de configuração do express, assim separando

cada arquivo para conter uma responsabilidade por exemplo:

* server.js
  * responsabilidade de subir o servidor no sistema.
* customExpress.js
  * responsabilidade de configurar todas as modificações que serão feitas no express
* atendimento.js
  * um dos arquivos de controle de rotas que estarão sendo criados ao longo do projeto

arquivo server.js (pasta raiz do projeto)

```javascript
const customExpress = require('./config/customExpress')

const app = customExpress()
const PORT = 3000
app.listen(PORT, () => console.log('Servidor Rodando na Porta 3000'))

```

customExpress.js (pasta config)

```javascript
const express = require('express')
const consign = require('consign')

module.exports = () =>{
    const app = express()
	/* Todo controller que for adicionado dentro do diretório controllers, vão ser invocados pelo app*/
	consign()
		.include('controllers')
		.into(app)
    return app
}

```



5 - Rota para criar um atendimento