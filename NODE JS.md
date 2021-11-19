# Node JS - Bibliotecas

* Node é um comando Runtime para a execução de códigos Javascript

* ## 1- criando um arquivo package Json

  * npm init -y
  * yarn init -y

* ## 2- Instalando uma Biblioteca (Lib)

  * npm install <nome-pacote>
  * yarn add <nome-pacote>

* ## Flags

  * -g : instalação de pacotes de maneira global

* ## node_modules

  * É um pacote onde contém todos os arquivos das dependências baixadas pelo node.

    * é recomendável criar um arquivo .gitignore e informar neste arquivo o nome do pacote node_modules

  * ### executando uma biblioteca

    * const nome-dep = require('nome-dep')
    
      

==========================================================================================================================

# Lendo arquivos

```javascript
fs = require('fs')
fs = readFile('path', 'utf-8', <callback>)
```

```javascript
/*Função básica para o tratamento de erro*/
function trataErro(erro){
    throw new Error(chalk.red(erro.code, 'Não há arquivo no caminho')) // chalk é uma biblioteca do Node que serve para mudar as cores do texto no console
}

/*Função para ler um arquivo*/
function pegaArquivo(caminho_do_arquivo){
    const encoding = 'utf-8'
    fs.readfile(caminho_do_arquivo, encoding, (erro,texto)=>{
        if(erro){
            trataErro(erro)
        }
        console.log(texto)
    })
}

pergarArquivo('./arquivos/texto1.md')
```

==========================================================================================================================

# Promessas

* Código Sícrono   :  Acontece em tempo real por exemplo comunicação por telefone.
* Código Assícrono: Não acontece em tempo real, enquanto um procedimento não acontecer ele parte para outro procedimento

```javascript
/*Função para ler um arquivo*/
function pegaArquivo(caminho_do_arquivo){
    const encoding = 'utf-8'
    fs.promises.readfile(caminho_do_arquivo, encoding)
    .then( (texto) => console.log(texto) ) /* Método callback que é executado após a promessa ser processada*/
    .catch((erro) => trataErro(erro)) /*Método para pegar um erro causa ele ocorra*/
}
```

## Utilizando o async / await

```javascript
async function pegaArquivo(caminho_do_arquivo){
    const encoding = 'utf-8'
    try{
        const texto = await fs.promises.readfile(caminho_do_arquivo, encoding)
    	console.log(texto)
    } catch (erro){
        trataErro(erro)
    }
    
}
```

## Promisse.all(  ) :

* recebe um conjunto de promessas por parãmetro e retorna um array com todas as promessas 

documentação: https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Promise/all



==========================================================================================================================

# Expressões Regulares Regex

* Site para teste em expressões regulares: https://regex101.com/
* Documentação: https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Regular_Expressions

## Classe das expressões regulares são representadas por [ ]

* [ abc ] : aqui vai listar todas as ocorrências de a, b e c em um t=determinado texto.
* [ ^abc ] : aqui vai retornar tudo que não for a, b ou c minúsculo.
* [a-z] : pega todas as letras de a-z minúsculas ignorando as que tiverem acento e caracteres especiais.
* [A-z] : pega todas as letras em maíusculo e minúsculo de A-z ignorando as que tiver acento e caracteres especiais.
* \ w : pega qualquer estrutura que não for palavra.
* \ S  : pega o espaço.
* \ [  ou \ ] : captura o caractere ' [ ' ou ' ] '  dizendo que não é uma sintaxe do regex.
* \ [ [ \w\S ] * \ ] : pega todos os textos com espaços que estiverem entre [ ], o caractere * simboliza que quer pegar todos.
* \ [ [ ^ \ ] ] * \ ] : pega tudo que não for um ' ] ', vai entrar na expressão e capturar tudo que estiver dentro  dos [ ].
* ab? : ' ab ' antes de ? , diz que pode ocorrer no texto 0 ou mais vezes

## Utilizando o Regex no Javascript

```javascript

function extraiCaracter(texto){ /* Esta função vai extrair os caracteres a de um texto*/
    const regex: /[a]/
    const letrasExtraidos = texto.match(regex)
}
/*Dependendo da expressão o retorno será um array com as letras ou textos encontrados*/
```

## Montando a expressão

Começamos observando os padrões na sequência que queremos capturar: todos os links começam da mesma forma, a partir de `http`, e terminam na primeira `/`. Então podemos começar a expressão dessa forma:

`https?`: a sequência exata de letras `http` e `s` ocorrendo de 0 até 1 vez.

Em seguida temos `://` que sempre ocorre da mesma forma:

`https?:\/\/`: como `/` é um *meta-char*, temos que usar a barra invertida para “escapar” este char, ou seja, para que seja considerado literalmente e não como *meta-char*.

A primeira classe, ou seja, o primeiro conjunto de caracteres antes do `.`, permanece o mesmo que usamos na aula:

`https?:\/\/[^\s$.?#].`: todos os caracteres exceto `$`, `.`, `?`, `#` e sem espaços em branco `\s`.

Agora a expressão deve parar que capturar os caracteres logo após a primeira `/`, então a segunda classe de caracteres vai ficar um pouco diferente:

`https?:\/\/[^\s$.?#].[^\/\s]`: após o ponto, todos os caracteres com exceção de `\s` (espaços em branco) e agora também `\/`, barra. Lembrando que `/` deve ser “escapada” para ser considerada como um caractere e não um *meta-char*.

Agora podemos finalizar, fazendo a expressão parar na primeira barra:

`https?:\/\/[^\s$.?#].[^\/\s]*\/`: acrescentamos `*` ao final, para que a expressão localize 0 ou mais ocorrências consecutivas entre `.` e `/`. Ou seja, para que a expressão localize tanto `https://dominio.com/` quanto `https://www.dominio.com.br/`.



==========================================================================================================================

# Montando Comandos

* Criando uma porta de entrada para uma aplicação NodeJS

```javascript
const caminho = process.argv /* retorna um array[2] com 2 endereços: [1] binários do node, [2] o caminho (raíz) absoluto*/

```



Um **caminho** é onde se localiza um arquivo ou diretório (que também chamamos de *pasta*) no sistema de arquivos de um sistema operacional. é importante diferenciar **caminho relativo** de **caminho absoluto**, além de como acessá-los.

## Caminho absoluto

Chamamos de caminho absoluto quando a localização de um arquivo ou pasta é especificado a partir do diretório-raiz do sistema operacional. Por exemplo:

```javascript
#caminho para um diretório (a última / é opcional)
/home/juliana/Documents/alura/projeto-js

#caminho para um arquivo dentro do diretório
/home/juliana/Documents/alura/projeto-js/index.js
```

## Caminho relativo

Um caminho relativo para um diretório ou arquivo é definido a partir de sua relação com o `pwd`, ou seja, o **present working directory** (diretório de trabalho atual). Na linha de comando, `pwd` também é o comando **print working directory** (imprimir o diretório de trabalho), que usamos justamente para saber onde na estrutura de diretórios do sistema operacional se encontra o diretório em que estamos.

Veja no exemplo abaixo uma representação em árvore de um diretório como o do curso em que estamos trabalhando (o diretório `node_modules` foi excluído para facilitar a leitura, pois é muito extenso):

```javascript
/home/juliana/Desktop/curso-js
.
├── curso-js
│   ├── arquivos
│   │   └── texto1.md
│   ├── package.json
│   ├── package-lock.json
│   └── src
│       ├── cli.js
│       ├── http-validate.js
│       └── index.js
```

Na representação acima, consideramos como `pwd` o diretório `curso-js`. Então, o caminho relativo do arquivo `texto1.md`, por exemplo, seria `./arquivos/texto1.md`, e o caminho absoluto seria `/home/juliana/Desktop/curso-js/arquivos/texto1.md`.

Na estrutura de diretórios, o `.` representa “aqui”. Quando queremos sair do diretório atual e “voltar” um nível, utilizamos `..`. Por exemplo:

```javascript
/home/juliana/Desktop/curso-js
.
├── curso-js
│   ├── arquivos
│   │   └── texto1.md
│   ├── package.json
│   ├── package-lock.json
│   ├── src
│   │   ├── cli.js
│   │   ├── http-validate.js
│   │   └── index.js
│   └── test
│       └── index.test.js
```

Se quisermos referenciar um módulo no arquivo `./src/index.js` a partir do arquivo `./test/index.test.js`, a importação seria feita da seguinte forma:

```javascript
// arquivo ./test/index.test.js

const moduloImportado = require(‘../src/index.js’)
```

Usamos o `..` para “subir um nível” na hierarquia de diretórios para depois “entrar” no diretório que queremos. Dessa forma, não precisamos referenciar o caminho absoluto de todos os arquivos quando fizermos importações de módulos; o que também funcionaria, mas não é tão prático.

Outra diferença importante entre caminho absoluto e relativo é com relação à execução de programas a partir da linha de comando. Por exemplo, usando a árvore de diretórios acima, o comando `node index.js` só funcionaria caso o diretório atual (pwd) no terminal já fosse `/home/juliana/Desktop/curso-js/src`. Por outro lado, o comando `node /home/juliana/Desktop/curso-js/src/index.js` funcionaria independentemente do diretório atual no terminal, pois o NodeJS vai acessar o arquivo `index.js` a partir de seu caminho absoluto.

O terminal é uma ferramenta poderosa. Além de executar comandos e rodar programas, com ele podemos fazer tudo que fazemos com as janelas e ícones do sistema operacional como navegar entre diretórios (ou pastas), criar arquivos, mudá-los de lugar e renomeá-los, dentre outras tarefas. Pratique os comandos para ir ganhando agilidade e para ficar confortável com o sistema de arquivos e diretórios.



==========================================================================================================================

# Status Code

Documentação: https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status



==========================================================================================================================

# Fetch API

Documentação: https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API

* Só funciona na parte front-end do navegador
* Existe uma biblioteca onde pode fazer a requisição lado servidor ($ npm i node-fetch)

Usando do node-fetch

```javascript
const fetch = require('node-fetch')
async function checaStatus(url){
    resp = await fetch(url) 
    return resp.status /*retorna um status code da url*/
}
```



==========================================================================================================================

# Exportando Módulos

A função que o NodeJS utiliza para importar módulos em um arquivo é `require()`. Os módulos podem importar e exportar todas as funções declaradas no arquivo ou apenas algumas, de acordo com o necessário. **Este formato de exportação e importação de módulos é conhecido como CommonJS ou CJS**.

Por exemplo, para exportar apenas uma função em um arquivo:

```javascript
module.exports = function soma(num1, num2) {
 return num1 + num2;
};
```

ou

```javascript
function soma(num1, num2) {
 return num1 + num2;
}

module.exports = soma;
```

É possível exportar apenas a função que precisa ser executada a partir de outra parte do código, enquanto outras funções do mesmo arquivo permanecem inacessíveis ao restante. Por exemplo:

```javascript
function soma(num1, num2) {
 return num1 + num2;
}

function multiplica(num1, num2) {
 return soma(num1, num2) * 2;
}

module.exports = multiplica;
```

No exemplo acima, declaramos as funções `soma()` e `multiplica()`. A função `multiplica()` executa `soma()` dentro de seu escopo e retorna um resultado. Podemos exportar somente a função `multiplica()` para ser utilizada em outras partes do código, sem a necessidade de exportar tudo.

Por outro lado, caso seja necessário exportar e importar mais de uma função, podemos fazer isso exportando um único objeto no final do arquivo/módulo:

```javascript
function soma(num1, num2) {
 return num1 + num2;
}

function multiplica(num1, num2) {
 return soma(num1, num2) * 2;
}

module.exports = { multiplica, soma };
```

Essas funções podem ser importadas desestruturando o mesmo objeto:

```javascript
const { multiplica, soma } = require('./caminho/arquivo');

const resultadoMult = multiplica(2, 2);
const resultadoSoma = soma(2, 2);
```

O mesmo princípio se aplica para módulos externos que instalamos em nosso projeto a partir de um gerenciador de pacotes como o NPM (com `npm install <nome da lib>`); nesse caso não precisamos passar o caminho, pois o JavaScript vai acessar os métodos necessários a partir da pasta `node_modules`:

```javascript
const chalk = require('chalk');
```

A mesma coisa para módulos que já são *built-in* no NodeJS, ou seja, são módulos que integram o sistema, mas mesmo assim precisam ser importados para dentro do projeto, como o módulo `fs` (file system, ou sistema de arquivos):

```javascript
const fs = require('fs');
```

Já vimos como o `chalk` funciona e veremos o `fs` mais para frente no curso.



## EcmaScript Modules, ou ESM

Você pode já ter visto (ou verá no futuro) trechos de código em JavaScript em que o mesmo processo de exportação de módulos é feito com a sintaxe `export` ou `export default` e a importação com a sintaxe `import <nomeModulo> from ‘./caminho/arquivo’`.

Esta outra forma de lidar com a importação e exportação de módulos veio com o famoso ES6 ou JS2015 e foi aos poucos sendo implementada para funcionar nativamente nos navegadores com a ajuda de *bundlers* como o WebPack, que fazem a “tradução” de métodos do JavaScript mais moderno para garantir retrocompatibilidade. Porém, o ESM não estava disponível para uso com NodeJS.

Hoje o ESM já funciona nativamente em todos os navegadores e passou a ter suporte para Node a partir da versão 13 (atualmente, o NodeJS está na versão 14.17.4). Mesmo assim, grande parte das aplicações desenvolvidas com NodeJS utiliza o formato CJS de importação e exportação de módulos.

A sintaxe do ESM segue os exemplos abaixo. Para exportar funções, por exemplo:

```javascript
export function soma(num1, num2) {
 return num1 + num2;
}

export function multiplica(num1, num2) {
 return soma(num1, num2) * 2;
}
```

Ou, alternativamente, para exportar somente a função `multiplica()` como *default* (padrão):

```javascript
function soma(num1, num2) {
 return num1 + num2;
}

function multiplica(num1, num2) {
 return soma(num1, num2) * 2;
}

export default multiplica;
```

Ou outra forma de exportar mais de uma função:

```javascript
function soma(num1, num2) {
 return num1 + num2;
}

function multiplica(num1, num2) {
 return soma(num1, num2) * 2;
}

export { multiplica, soma };
```

Para fazer a importação, seguem os exemplos:

```javascript
import soma from './caminho/arquivo';
```

Ou, no caso de mais funções exportadas a partir do mesmo módulo:

```javascript
import { soma, multiplica } from './caminho/arquivo';
```

É possível também importar de uma só vez todo o objeto exportado:

```javascript
import * as operacoes from './caminho/arquivo';
```

E importar da seguinte forma:

```javascript
const soma = operacoes.soma(1, 2);
const multiplica = operacoes.multiplica(1, 2);
```

Existe uma convenção no uso de ESM em projetos NodejS, que é utilizar a extensão `.mjs` para distinguir quais arquivos são módulos, continuando com a extensão `.js` para os arquivos que não exportam módulos.

==========================================================================================================================

# Testes

* Jest é um framework de teste do Javascript
  * documentação: https://jestjs.io/pt-BR/docs/getting-started

1- Criar na raíz do projeto uma pasta teste

2- Criar um arquivo para teste por exemplo "index.test.js"

```javascript
function sum(a, b) {
  return a + b;
}

test('Função de Soma', ()=>{ /* É passado por parãmetro o noem do teste */
	expect( sum(1,2).toBe(3)) /* Executa a função de soma com o parãmetro 1 e 2 esperando como saída o valor 3*/
})
```

testando várias entradas

```javascript
describe('Função de Soma sendo testada'){/*Descrição do que vai ser testado*/
    it('Função de Soma', () =>{ // mesmo comportamento de expect
        expect( sum(1,2).tobe(3))
    })
    
    it('Função de Soma', () =>{ // mesmo comportamento de expect
        expect( sum(2,2).tobe(4))
    })         
}


```

Matches

* toBe ( ): compara tipos primitivos de dados

* toEqual(): compara objeto

  

==========================================================================================================================

