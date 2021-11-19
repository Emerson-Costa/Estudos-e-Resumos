# -----------------------------------> JAVASCRIPT <-------------------------------

### O JAVASCRIPT é uma linguagem baseada em protótipo em que tudo nela é considerado objeto



* ## Tipos de Variáveis

  * Três formas de escrever variáveis
    * var : primeira que surgiu onde funciona em qualquer parte do código.
      * pode usar antes de declarar.
    * let : criada após o surgimento da var, onde existe regras de uso desta palavra reservada
      * só funciona dentro de blocos de  códigos como métodos e condições conforme o caso
    * const : um tipo constante que mantém o valor fixo
  * Hoje em dia a var não é utilizada sendo substituída pela 'let'
  * typeof (<variavel> ) : informa o tipo de dado que estamos trabalhando

  

  

* ## Condições ternárias

  * <condição>?<if-return> : <else-return>



* ## Arrays, Listas e Prototype

  * const lista = new Array(  'elemento1',  'elemento2');

  * lista.push('elemento') : adiciona um novo elemento no final da lista

  * lista.pop(): retira um elemento do topo da lista

  * lista.splice(indice, qutd_item ) : remove um elemento

  * concat(<array>) : junta dois arrays

  * filter( <funcao-callback>) : filtra cada elemento através de uma função de call-back com uma determinada condição 

  * find( <funcao-callback>): busca por apenas um elemento definido pela condição da função de callback

  * findIndex(<funcao-callback>): semelhante ao find() mas retorna o índice do elemento

  * lastIndexOf(<funcao-callback>): semelhante ao findIndex(), porém começa do ultimo elemento e não do primeiro

  * array.slice(<indice-corte>,<final-corte>): divide um array

  * array.slice(<indice-inicial>,<indice-new-elemento>,<elemento>): atualiza um elemento de um  array

  * array1[array2] : modo de obter um array dentro do outro com 2 dimensões

  * array.includes(<elemento>): verifica se há algum elemento dentro de um determinado array

  * DOCUMENTAÇÃO (ARRAY.Proptotype) =  'https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/@@unscopables'

    ```
    arr.__proto__ : guarda todas as definições de funçoes de um array
    ```
  
    
  
    
  
    

## Métodos  CallBack

* ## ForEach

  ```javascript
  array.forEach(elemento => {
  	console.log(elemento)
  })
  
  saída(elemento)
  ```

* ## Map

  ```javascript
  array = [1,2,3,4]
  array.map(elemento => elemento + 1)
  
  saída(2,3,4,5)
  ```

* ## Reduce

  ```javascript
  array = [1,2,3,4,5]
  array.reduce( (acumulador, valor-atual) => valor-atual + acumulador, 0)
  
  saida(14)
  ```
  
  

* ## Funções e Objetos

  Iniciaremos com a criação de um novo arquivo chamado ” clientePrototipo.js” e adicionando o código abaixo a ele, para definir construtores de objetos.

  ```javascript
  function Cliente (nome, cpf, email, saldo) {
   this.nome = nome
   this.cpf = cpf
   this.email = email
   this.saldo = saldo
    this.depositar = function(valor){
     this.saldo += valor
   }
  }
  const andre = new Cliente("Andre", "12312312312", "andre@email.com", 100)
  ```

	   Agora vamos definir um construtor que utiliza o `Cliente` para gerar um novo tipo de cliente.

```javascript
function ClientePoupanca(nome, cpf, email, saldo, saldoPoup){
 Cliente.call(this, nome, cpf, email, saldo)
 this.saldoPoup = saldoPoup
}
const ju = new ClientePoupanca("Ju", "12312312312", "ju@email.com", 100, 200)
```

​		Agora vamos manipular o comportamento do protótipo do objeto `ClientePoupanca`, adicionando o código abaixo:

```javascript
ClientePoupanca.prototype.depositarPoup = function(valor){
 this.saldoPoup += valor
```

​		Então podemos testar algumas propriedades e validar a ideia de cadeia de protótipos. Digite o seguinte:

```javascript
console.log(andre.hasOwnProperty("saldoPoup"))
console.log(ju.hasOwnProperty("saldoPoup"))
console.log(andre instanceof Cliente)
console.log(typeof andre)
console.log(typeof ju)
console.log(ju instanceof ClientePoupanca)
console.log(Function.prototype.isPrototypeOf(Cliente))
console.log(Cliente.constructor === Function)
```

​		Na saída do console veremos que usando `.hasOwnProperty()`, a função “saldoPoup” só existe no objeto do tipo `ClientePoupanca`.

​		Os objetos `andre` e `ju` são do tipo `object`, ou seja, ambos herdam do protótipo de object, que está associado a todo objeto criado no JavaScript.





# Orientação a objetos



```javascript
class pessoa {
    constructor(nome, idade, sexo){
        this.nome = nome
        this.idade = idade
        this.sexo = sexo
    }
    
    exibirNome(){
        console.log(nome)
    }
}

/* Conceito de Herança*/
class Gerente extends Pessoa{
    constructor(nome, idade, sexo, nome_loja){
        super(nome, idade, sexo)
        this.nome_loja = nome_loja
    }
}
```

​     

