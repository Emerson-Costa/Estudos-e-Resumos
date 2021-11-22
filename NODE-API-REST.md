# Node JS - API REST

==========================================================================================================================

## Estrutura organizacional da documentação do projeto

* PROJETO

  * config
    * "customExpress.js"
  * controllers
    * "atendimentos.js"
  * infraestrutura
    * "conexao.js"
    * "tabelas.js"
  * models (responsável por fazer a conexão como Banco de Dados)
    * "atendimentos.js"
  * "server.js"

  

==========================================================================================================================

Libs utilizadas no projeto

* consign       : faz o agrupamento das rotas dentro do app
* mySql          : conexão com o Banco de dados
* body-parser :  traduzir o tipo de requisição
*  moment      :  uma biblioteca que manipula datas de vários formatos

=========================================================================================================================



===================================  Alguns Passos importantes feitos durante o desenvolvimento ======================================

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

atendimentos.js (pasta controllers)

* Criando a requisição POST, que é um tipo de requisição feita através de um formulário.

*  Instalando a Lib body-parser para traduzir o tipo de requisição

customExpress.js (pasta config)

```javascript
const express = require('express')
const consign = require('consign')
const bodyParser = require('body-parser') /*Incluindo a lib body-parser*/

module.exports = () =>{
    const app = express()
	/*inserindo o body-parser para o express fazer o uso do mesmo*/
    app.use(bodyParser.urlencoded({extend: true})) 
    app.use(bodyParser.json())
    /*************************************************************/
	consign()
		.include('controllers')
		.into(app)
    return app
}
```



atendimentos.js (pasta controllers)

```javascript
module.exports = app => {
	app.get('/atendimentos', (req, res) => {
        res.send('Você está na rota atendimentos e esta realizando um GET')
    })
	app.POST('/atendimentos', (req, res) => {
        console.log(req.body)/*Body é o corpo da requisição que foi recebida, mas ainda este corpo não foi definido*/
        res.send('Você está na rota atendimentos e esta realizando um POST')
    })
}
```



6 - Criando uma base de dados com o MySQL

* instalar a lib do mysql
* Criar um diretório chamado 'infraestrutura' onde vai conter tudo que estiver relacionado ao banco de dados
* dentro da pasta de 'infraestrutura' criar um arquivo 'conexao.js' onde o mesmo vai servir para fazer a conexão com o banco de dados

conexao.js (pasta infraestrutura)

```javascript
const mysql = require('mysql')

/*Criando uma conexão com o Banco de Dados Mysql*/
const conexao = mysql.createConnection({
    host: 'localhost',
    port: 3307,
    user: 'root'
    password: 'admin'
    database: 'agenda-petshop'
})

module.exports = conexão
```

arquivo server.js (pasta raiz do projeto)

```javascript
const customExpress = require('./config/customExpress')

/*importando os arquivos e conectando com o banco de dados*/
const conexao = require('./infraestrutura/conexao')

conexao.connect( erro =>{
    if(erro){
         console.log(erro)
    } else {
         console.log('Conectado com Sucesso!')
         const app = customExpress()
		const PORT = 3000
		app.listen(PORT, () => console.log('Servidor Rodando na Porta 3000'))
    }
})
/*********************************************************/


```



7 - Criação de uma tabela para o banco

* criar um script 'tabelas.js' dentro da pasta infraestrutura

tabelas.js (pasta infraestrutura) 

```javascript
class Tabelas{
	init( conexao ) {
        this.conexao = conexao
        this.criarAtendimentos()
    }
    /*Criando a tabela de atendimentos*/
    criarAtendimento(){
        const sql = 'CREATE TABLE IF NOT EXISTS Atendimentos(
        ' id int NOT NULL AUTO_INCREMENT, cliente varchar(50) NOT NULL)
        ' pet varchar(20), servico varchar(20) NOT NULL, status varchar(20)
        ' observacoes text, PRIMARY KEY(id)'
        
        this.conexao.query(sql, (erro) => {
            if(){
                console.log(erro)
            } else {
                console.log('Tabela criada com sucesso!')
            }
        })
    }
}

module.exports = new Tabelas /*A palavra reservada new indica que vão haver várias instâncias diferenciadas de Tabela*/
```

arquivo server.js (pasta raiz do projeto)

```javascript
const customExpress = require('./config/customExpress')
const conexao = require('./infraestrutura/conexao')
/*importando o módulo da tabelas*/
const Tabelas = require('./infraestrutura/tabelas')

conexao.connect( erro =>{
    if(erro){
         console.log(erro)
    } else {
         console.log('Conectado com Sucesso!')
         /*iniciando a conexão com o banco em Tabelas*/
         Tabelas.init(conexao)
         const app = customExpress()
		const PORT = 3000
		app.listen(PORT, () => console.log('Servidor Rodando na Porta 3000'))
    }
})
```



8 - Enviando informações para a base de dados

* Criar uma pasta chamada 'models' e dentro desta pasta criar um arquivo chamado 'atendimentos.js'

atendimentos.js (pasta models)

```javascript

class Atendimento {
	adiciona(atendimento){
		const sql = 'INSERT INTO Atendimentos SET ?'
        
        conexao.query(sql,atendimento, (erro, resultados) => {
            if(erro){
                console.log(erro)
            }else {
                console.log(resultados)
            }
        })
	}
}

module.exports = new Atendimento
```

atendimentos.js (pasta controllers)

```javascript
const Atendimento = require('../models/atendimentos')

module.exports = app => {
	app.get('/atendimentos', (req, res) => {
        res.send('Você está na rota atendimentos e esta realizando um GET')
    })
	app.POST('/atendimentos', (req, res) => {
        console.log(req.body)/*Body é o corpo da requisição que foi recebida, mas ainda este corpo não foi definido*/
        res.send('Você está na rota atendimentos e esta realizando um POST')
    })
}

```



9 - Adicionando dados no atendimento

Instalar a biblioteca moment, uma biblioteca que manipula datas de vários formatos

tabelas.js (pasta infraestrutura) 

```javascript
class Tabelas{
	init( conexao ) {
        this.conexao = conexao
        this.criarAtendimentos()
    }
    /*Inserindo mais dois campos na Tabela de atendimento 'dataCriacao e data do tipo datetime'*/
    criarAtendimento(){
        const sql = 'CREATE TABLE IF NOT EXISTS Atendimentos(
        ' id int NOT NULL AUTO_INCREMENT, cliente varchar(50) NOT NULL)
        ' pet varchar(20), servico varchar(20) NOT NULL, data datetime NOT NULL,
        ' dataCriacao datetime NOT NULL, status varchar(20)
        ' observacoes text, PRIMARY KEY(id)'
        
        this.conexao.query(sql, (erro) => {
            if(){
                console.log(erro)
            } else {
                console.log('Tabela criada com sucesso!')
            }
        })
    }
}

module.exports = new Tabelas 
```

atendimentos.js (pasta models)

```javascript
const moment = require('moment')
const conexao = require('../infraestrutura/conexao')

class Atendimento {
    
	adiciona(atendimento){
         /* Criação de novos campos para inserção dos dados na Tablea*/
        const dataCriacao = moment().format('YYYY-MM-DD HH:MM:SS') /*Inserindo a instância do moment no local da data*/
        const data = moment(atendimento.data, 'DD/MM/YYYY').format('YYYY-MM-DD HH:MM:SS') /*Formatando a data*/
        
        /*Validando data e cliente*/
        const dataEhValida = moment(data).isSameOrAfter(dataCriacao)
        const clienteEhValido = atendimento.cliente.length >= 5
        
        /* array de objetos para validar os campos*/
        const validacoes = [
            {
                nome: 'data',
                valido: dataEhValida, 
                mensagem: 'Data deve ser maior ou igual a data atual'
            },
            {
                nome: 'cliente',
                valido: clienteEhValido,
                mensagem: 'Cliente deve ter pelo menos cinco caracteres'
            }
        ]
        
        /* Filtrado as validações, retorna false caso caso um campo não seja válido*/
        const erros = validacoes.filter(campo => !campo.valido)
        
        /*verifica se existe algum erro, se estiver vazio não existe erro*/
        const existemErros = erros.length
        
        if(existemErros){
			res.status(400).json(erros)
        } else {
            const atendimentoDatado = {...atendimento, dataCriacao, data} 
		   const sql = 'INSERT INTO Atendimentos SET ?'
            conexao.query(sql,atendimentoDatado, (erro, resultados) => {
            	if(erro){
                	res.status(400).json(erro) /*Inserindo o Http Status como resposta ao cliente*/
            	}else {
                	res.status(201).json(resultados)
            	}
        	})
        }
	}
    
    listarAtendimentos(res) {
        const sql = 'SELECT * FROM Atendimentos'
        
        conexao.query(sql, (erro, resultado) => {
            if(erro){
                res.status(400).json(erro)
            } else {
                res.status(200).json(resultados)
            }
        })
    }
}
module.exports = new Atendimento
```



atendimentos.js (pasta controllers)

```javascript
const Atendimento = require('../models/atendimentos')

module.exports = app => {
	app.get('/atendimentos', (req, res) => {
        Atendimento.lista(res)
    })
    
    app.get('/atendimentos/:id', (req, res) => {
       const id = parseInt(req.params.id)
       Atendimento.buscaPort(id, res)
    })
    
	app.post('/atendimentos', (req, res) => {
        console.log(req.body)/*Body é o corpo da requisição que foi recebida, mas ainda este corpo não foi definido*/
        res.send('Você está na rota atendimentos e esta realizando um POST')
    })
}

```

==========================================================================================================================





