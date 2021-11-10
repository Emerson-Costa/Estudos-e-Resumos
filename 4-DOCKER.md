# Docker



* Virtualização

  * Resolve o problema da ociosidade (muito recurso para poucas requisições), pois divide em várias maquinas virtuais o hardware oscioso

  * Custos como RAM, capacidade de disco HD, SO etc....

    

* Containers

  * Não precisa ter um SO para rodar uma aplicação

  * É mais leve que uma máquina virtual

  * Mais rápido para subir(upload)

    

* Docker

  * Docker (Empresa) 

    * Anteriormente era chamada dotCloud
    * Faz 'Plataform as a Service' como:   Heroku, Azure etc..

  * Docker (Tecnologias)

    * Faz o intermédio entre o container e o SO

    * Docker Composer

      * um jeito mais fácil de trabalhar com múltiplos containers

    * Docker Swarm

      * colocar múltiplos Dockers Host's para trabalharem junto ao cluster

    * Docker Hub

      * Repositório de imagens

    * Docker Machine

      * Permite instalar e configurar hosts virtuais

        

# Imagens e Containers

* O Docker busca automaticamente a imagem caso tu informe o nome correto da mesma e faz o download dessa imagem para o seu computador.
* Toda imagem possui mais de uma camada, leitura e escrita são bloqueadas para essas camadas
* Containers são voláteis, mas possui volumes

# Comandos Docker

* $ docker <comando> --help : ajuda para executar o comando informado
* $ docker  : lista todos os comando do docker
* $ docker version : listar versão do docker
* $ docker run <nome_imagem> ou  : executando  uma imagem
* $ docker run <nome_imagem> <comando_app_imagem> : executa uma determinada funcionalidade a qual a imagem oferece
* $ docker run -it <nome_imagem> : acessar o terminal referente a imagem escolhida
  * CTRL + D : encerra a execução da imagem 
* $ docker run <imagem_Oficial> / <nome_criador> : buscando uma imagem criada por alguém a partir da oficial
* $ docker run  --name 'nome_Container' <nome_imagem> 
* $ docker run -p 123:80 <nome_imagem> : roda uma determinada imagem em uma porta específica indicada pelo usuário
* $ docker run -e AUTOR="nome" <nome_imagem> : adiciona o nome do autor no servidor
* $ docker ps : lista os containers ativos até o momento
* $ docker ps -a : lista todos os containers que foram criados
* $ docker start
* $ docker start <id_container> : ativar um container específico
* $ docker stop <id_container> : encerra as atividades do container
* $ docker stop $(docker ps -q) : vai encerrar os dois containers
* $ docker start -a -i  <id_container> : inicia a imagem direto no terminal da mesma
* $ docker rm <id_imagem> : remove um determinado container
* $ docker container prune : remove todos os containers
* $ docker images : mostra todas as imagens contidas no docker
* $ docker rmi <nome_imagem> : remove uma determinada imagem
* $ docker port <id_aplicação> : lista a porta que o container está utilizando



# Principais estados do Container

* Running 
* Stopped



# Criando um volume no container

* docker run -d -v "<path>:/var/<nome-pasta>" <nome-image> : criando um diretório de dados referente a imagem no computador
* O volume fica salvo no DockerHost
* -d : flag que evita travar os serviços no terminal



# Rodando um código em um container utilizando volumes

* docker run -d -p 8080:3000 -v "<path-volume>/:/var/<nome-pasta>"  -w "/var/<nome-pasta>" node npm start : usando a flag `-v` seguindo pelo `CAMINHO_HOST:CAMINHO_CONTAINER`



# Criando um Dockerfile com o Node

* nomear um arquivo com o nome "Dockerfile" ou salvar com a extensão <arquivo.dockerfile>
  * FROM <nome-tecnologia> : <versao>
  * MAINTAINER  <nome-imagem>
  * ENV NODE_ENV=production :criando uma variável de ambiente para produção
  * ENV PORT=3000
  * COPY ./var/<nome-pasta>  : copia todo o conteúdo da pasta para este diretório
  * WORKDIR /var/<nome-pasta>
  * RUN npm install : instalar as dependências do node
  * ENTRYPOINT  npm start  ou ["npm","start"]  :comando que vai ser executado assim assim que o container for carregado
  * EXPOSE $PORT : iniciando o projeto na porta 3000 que foi atribuído a variável de ambiente PORT
* Executar o seguinte comando
  * docker build -f <nome-arquivo-dockerfile>  -t <nome>/<nome-imagem> .
    * Caso o arquivo for apenas Dockerfile não precisa usar a flag -f
    * flag -t -e a targ da imagem

# Colocando a imagem no DockerHub

* docker login : realizar o login na conta do DockerHub
* docker push <nome-imagem>
* docker pull <nome-imagem>

# Redes no Docker

* Trabalhando com múltiplos containers no Docker
* Aplicações conversam entre si
* Docker cria uma rede virtual para cada container 
* docker inspect <id-container> : inspecionando um container
* comunicação entre os containers na rede Default do Docker só acontece será através do IP
* Criando uma rede no Docker
  * docker network create --driver bridge 
  * docker network ls : lista todos os nomes da redes no docker
  * docker run -it --name <meu-container> --network <nome-rede>



# Docker Composer

* <nome-documento>.yml    ou <nome-documento>.yaml

* ```
  version: '3'
  services: 
  	nginx:
  		build:
  			dockerfile: <path>
  			context: .
  		image: <nome>/nginx
  		container_name: nginx
  		ports:
  			- "80:80"
  		 networks: 
  		 	- production-network
  		 depends_on:
  			- "node1"
  			- "node2"
  		 	
  	mongodb:
  		image: mongo
  		networks:
  			- production-network
  			
  	node1:
  		build:
  			dockerfile: <path>
  			context: .
  		image: <nome>/aluraBooks
  		container_name: alura-books1
  		ports:
  			- "3000"
  		networks:
  		- production-network
  		depends_on:
  			- "mongodb"
  		
  	node2:
  		build:
  			dockerfile: <path>
  			context: .
  		image: <nome>/aluraBooks
  		container_name: alura-books2
  		ports:
  			- "3000"
  		networks:
  		- production-network
  		depends_on:
  			- "mongodb"
  		
  networks:
  	- production-network:
  		driver: bridge
  ```

  * $ docker-compose build : constrói os serviços
  * $ docker-compose up : inicia os serviços
  * $ docker-compose down : finaliza os serviços
  * $ docker-compose ps : lista os serviços que estão sendo executados



## Sobre Microsserviços

Já ouviu falar de Microsserviços? Se já ouviu, pode pular a introdução abaixo e ir diretamente para a parte "Docker e Microsserviços", senão continue comigo.

Uma forma de desenvolver uma aplicação é colocar todas as funcionalidades em um único "lugar". Ou seja, a aplicação roda em uma única instância (ou servidor) que possui todas as funcionalidades. Isso talvez seja a forma mais simples de criar uma aplicação (também a mais natural), mas quando a base de código cresce, alguns problemas podem aparecer.

Por exemplo, qualquer atualização ou *bug fix* necessita parar todo o sistema, buildar o sistema todo e subir novamente. Isso pode ficar demorado e lento. Em geral, quanto maior a base de código mais difícil será manter ela mesmo com uma boa cobertura de testes e as desvantagens não param por ai. Outro problema é se alguma funcionalidade possuir um gargalo no desempenho o sistema todo será afetado. Não é raro de ver sistemas onde relatórios só devem ser gerados à noite para não afetar o desempenho de outras funcionalidades. Outro problema comum é com os ciclos de testes e *build* demorados (falta de agilidade no desenvolvimento), problemas no monitoramento da aplicação ou falta de escalabilidade. Enfim, o sistema se torna um legado pesado, onde nenhum desenvolvedor gostaria de colocar a mão no fogo.

## Arquitetura de Microsserviços

Então a ideia é fugir desse tipo de arquitetura monolítica monstruosa e dividir ela em pequenos pedaços. Cada pedaço possui uma funcionalidade bem definida e roda como se fosse um "mini sistema" isolado. Ou seja, em vez de termos uma única aplicação enorme, teremos várias instâncias menores que dividem e coordenam o trabalho. Essas instâncias são chamadas de Microsserviços.

Agora fica mais fácil monitorar cada serviço específico, atualizá-lo ou escalá-lo pois a base de código é muito menor, e assim o *deploy* e o teste serão mais rápidos. Podemos agora achar soluções específicas para esse serviço sem precisar alterar os demais. Outra vantagem é que um desenvolvedor novo não precisa conhecer o sistema todo para alterar uma funcionalidade, basta ele focar na funcionalidade desse microsserviço.

Importante também é que um microsserviço seja acessível remotamente, normalmente usando o protocolo HTTP trocando mensagens JSON ou XML, mas nada impede que outro protocolo seja usado. Um microsserviço pode usar outros serviços para coordenar o trabalho.

Repare que isso é uma outra abordagem arquitetural bem diferente do monolítico e por isso também é chamado de *arquitetura de microsserviços*.

Por fim, uma arquitetura de Microsserviços tem um grau de complexidade muito alta se comparada com uma arquitetura monolítica. Aliás, há aqueles profissionais que indicam partir para uma [arquitetura monolítica primeiro e mudar para uma baseada em microsserviços depois](http://sdtimes.com/martin-fowler-monolithic-apps-first-microservices-later/), quando estritamente necessário.

## Docker e Microsserviços

Trabalhar com uma arquitetura de microsserviços gera a necessidade de publicar o serviço de maneira rápida, leve, isolada e vimos que o **Docker** possui exatamente essas características! Com **Docker** e **Docker Compose** podemos criar um ambiente ideal para a publicação destes serviços.

O **Docker** é uma ótima opção para rodar os microsserviços pelo fato de isolar os *containers*. Essa utilização de *containers* para serviços individuais faz com que seja muito simples gerenciar e atualizar esses serviços, de maneira automatizada e rápida.

## Docker Swarm

Ok, tudo bem até aqui. Agora vou ter vários serviços rodando usando o **Docker**. E para facilitar a criação desses containers já aprendemos usar o **Docker Compose** que sabe subir vários *containers*. O **Docker Compose** é a ferramenta ideal para coordenar a criação dos *containers*, no entanto para melhorar a escalabilidade e desempenho pode ser necessário criar muito mais *containers* para um serviço específico. Em outras palavras, agora gostaríamos de criar *muitos containers* aproveitando *várias* máquinas (virtuais ou físicas)! Ou seja, pode ser que um microsserviço fique rodando em 20 *containers* usando três máquinas físicas diferentes. Como podemos facilmente subir e parar esses *containers*? Repare que o **Docker Compose** não é para isso e por isso existe uma outra ferramenta que se chama **Docker Swarm** (que não faz parte do escopo desse curso).

**Docker Swarm** facilita a criação e administração de um *cluster* de *containers*.





Segue a lista com os principais comandos utilizados durante o curso:

- Comandos relacionados às informações
  - `docker version` **-** exibe a versão do docker que está instalada.
  - `docker inspect ID_CONTAINER` **-** retorna diversas informações sobre o container.
  - `docker ps` **-** exibe todos os containers em execução no momento.
  - `docker ps -a` **-** exibe todos os containers, independentemente de estarem em execução ou não.

- Comandos relacionados à execução
  - `docker run NOME_DA_IMAGEM` **-** cria um container com a respectiva imagem passada como parâmetro.
  - `docker run -it NOME_DA_IMAGEM` **-** conecta o terminal que estamos utilizando com o do container.
  - `docker run -d -P --name NOME dockersamples/static-site` **-** ao executar, dá um nome ao container e define uma porta aleatória.
  - `docker run -d -p 12345:80 dockersamples/static-site` **-** define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345.
  - `docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM` **-** cria um volume no respectivo caminho do container.
  - `docker run -it --name NOME_CONTAINER --network NOME_DA_REDE NOME_IMAGEM` **-** cria um container especificando seu nome e qual rede deverá ser usada.

- Comandos relacionados à inicialização/interrupção
  - `docker start ID_CONTAINER` **-** inicia o container com id em questão.
  - `docker start -a -i ID_CONTAINER` **-** inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos.
  - `docker stop ID_CONTAINER` **-** interrompe o container com id em questão.

- Comandos relacionados à remoção
  - `docker rm ID_CONTAINER` **-** remove o container com id em questão.
  - `docker container prune` **-** remove todos os containers que estão parados.
  - `docker rmi NOME_DA_IMAGEM` **-** remove a imagem passada como parâmetro.

- Comandos relacionados à construção de Dockerfile
  - `docker build -f Dockerfile` **-** cria uma imagem a partir de um Dockerfile.
  - `docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM` **-** constrói e nomeia uma imagem não-oficial.
  - `docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM CAMINHO_DOCKERFILE` **-** constrói e nomeia uma imagem não-oficial informando o caminho para o Dockerfile.

- Comandos relacionados ao Docker Hub
  - `docker login` **-** inicia o processo de login no Docker Hub.
  - `docker push NOME_USUARIO/NOME_IMAGEM` **-** envia a imagem criada para o Docker Hub.
  - `docker pull NOME_USUARIO/NOME_IMAGEM` **-** baixa a imagem desejada do Docker Hub.

- Comandos relacionados à rede

  - `hostname -i` **-** mostra o ip atribuído ao container pelo docker (funciona apenas dentro do container).
  - `docker network create --driver bridge NOME_DA_REDE` **-** cria uma rede especificando o driver desejado.

- Comandos relacionados ao docker-compose

  - `docker-compose build` **-** Realiza o build dos serviços relacionados ao arquivo docker-compose.yml, assim como verifica a sua sintaxe.

  - `docker-compose up` **-** Sobe todos os containers relacionados ao docker-compose, desde que o build já tenha sido executado.

  - `docker-compose down` **-** Para todos os serviços em execução que estejam relacionados ao arquivo docker-compose.yml.

    

