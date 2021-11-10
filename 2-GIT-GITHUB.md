# Git-GitHub

* Sistema de Controle de versões
  * CVS, SVN, MERCURIAL e GIT

* Logar no repositório

  ```
  git config --local user.name "Seu nome aqui"
  git config --local user.email "seu@email.aqui"
  ```

* comandos

  * git init 
  * git status
    * **`HEAD`:** Estado atual do nosso código, ou seja, onde o Git os colocou
    * **`Working tree`:** Local onde os arquivos realmente estão sendo armazenados e editados
    * **`index`:** Local onde o Git armazena o que será *commitado*, ou seja, o local entre a *working tree* e o repositório Git em si.
  * git add <nomeArquivo>
  * git log
  * git log --oneline     : Mostra os commits em um única linha
  * git log -p                : Mostra os commits com as alterações 
  * git log --pretty="parametros de formatação"
  * git log --graph
    * https://devhints.io/git-log
  * git config <user.x> : Altera as configurações do repositório local ou global
    * git config --local user.name "Emerson Costa"

* arquivo gitignore

  * 1- Crie um arquivo '.gitignore ' na pasta do projeto
  * 2- Para ignorar um arquivo, basta digitar o nome do arquivo ou pasta  ignorado no arquivo '.gitignore'.

  * 3-  adicionar o arquivo .gitignore
    * git add .gitignore
    * git commit "Adicionando o arquivo .gitignore"

  

* Criando um repositório remoto

  * mkdir <nomeDiretório>

  * cd <nomeDiretório>

  * git init --bare

    * um link com o caminho do diretório será criado copie este link

  * No repositório do projeto cole o link no seguinte comando:

    * git remote add <nomeServidor> <link>

      

* Branches

* https://git-school.github.io/visualizing-git/

  * git branch <nomeBranch>

  * git checkout <nomeBranch>

  * git checkout -b <nomeBranch>

    

* Merge

  * git merge <nomeBranch>

  * git rebase <nomeBranch>

    

* Desfazendo Alterações utilizando git

  * git checkout --<nomeArquivo> : sem ter feito o add

  * git reset HEAD <nomeArquivo> : ter feito add , após ter feito comando faça novamente o checkout

  * git revert <hashCommit> : quando foi feito um commit

    

* Salvar todas as alterações em um local temporário

  * git stash

  * git stash list : lista tudo que está salvo

    

* Pegar a alterações que se encontram no local temporário

  * git stash apply <numberList>

    

* Apagar as alterações armazenadas no local temporário

  * git stash pop : apaga a ultima alteração feita

    

* Viajando pelo tempo entre os commits

  * git checkout <HashCode>

  * Criar uma branch para o commit não se perder na timeline

    

* Vendo a diferença entre dois commits, listas todas as 'Alterações' de diversos commits

  * git diff <hashCodeInicial> <hashCodeFinal>

    

* Tags e releases

  * Tag marca um ponto na linha do tempo que não poderá ser modificada
  * git tag -a <nomeTag>  -m "mensagem" | (exemplo de nome para versão da tag 'v0.1.0') |
  * git push origin v0.1.0

