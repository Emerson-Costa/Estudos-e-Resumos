## Javascript-DOM

* DOM - Document Object Model
* O DOM é a representação do árvore HTML representando os nós pais e filhos
* Com o javascript é possíves trabalhar dinamicamente com o DOM, pemitindo criar páginas dinâmicas e interativas

* (Tutorial JS-DOM)[https://www.w3schools.com/js/js_htmldom.asp]

Código   | Função
-------- | ------
`document.querySelector('p')` | possibilita percorrer a árvore do DOM e capturar a tag `p`
`document.getElementById('id')` | R$ 8
`document.getElementByClassName('className')` | Captura p elemento pelo nome da classe
`document.getElementsByTagName(name)` | Captura o elemento pelo nome da TAG
`document.createElement(element)` | Cria elemento HTML
`document.removeChild(element)` | Remove elemento HTML
`document.appendChild(element)` | Adiciona elemento HTML
`document.addEventListener(click,function)` | Adiciona evento de click a um elemento html com sua respectiva função
`document.getElementById("demo").innerHTML=elemento html` | Adiciona elemento html dentro de outro
`document.querySelectorAll("p")` | Selectiona todos os elementos com a tag p
`document.querySelector('p').parentElement` | Selectiona o elemento pai do nó
`document.querySelector('p').ClassList.add` | Adiciona uma classe em uma tag HTML



* Componentes
    * Componentes são usados para criar funcionalidades isoladas no código de modo que um mesmo componente pode ser usado diversas vezes
    * Ao exportar um módulo, ele pode ser compartilhado em outras partes do código `export default Component`
    * Qualquer parte do código pode importar um componente. `import Componente from ./Component`
    ```
    const BotaoConclui = () => { 
    const botaoConclui = document.createElement('button')  
    
    botaoConclui.classList.add('check-button')
    botaoConclui.innerText = 'concluir'

    botaoConclui.addEventListener('click', concluirTarefa)

    return botaoConclui

    }

    export default BotaoConclui
    ```

## IIFE

* Immediately Invoked Function Expression ou “Função de Invocação Imediata”. 
 ```
    (function helloWorld(){
        alert('Hello, world!');
    }
    )();
```
* A Pricipal vantagem de uma IIFE é a possibilidade de usar seu escopo para criar um fechamento e impedir que dados sejam acessados dentro desse escopo fechado

