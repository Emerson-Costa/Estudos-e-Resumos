# HTTP

* Protocolo de comunicação entre um cliente e um servidor
* BitTorrent é um protocolo de comunicação utilizado para a troca de arquivos
* É Stateless pois não guarda requisições antigas



# HTTPS

* Modo seguro de comunicação entre um cliente e um servidor
* Gera uma certificação válida com uma determinada chave de criptografia SHA256 entre outros
* Chaves Publica e Privada
* Certificado Digital (Identidade)
* Navegador(Chave Pública)   <------>  Servidor(Chave Privada)
* Criptografia Simétrica   : Usa uma chave pública e uma chave privada, tendo duas chaves diferentes envolvidas, este tipo de comunicação costuma ser lenta.
* Criptografia Assimétrica: utiliza a mesma chave para decifrar os dados, é uma comunicação mais rápida porém não segura.

# Endereços

* protocolo**://**domínio(IP)**:**porta**/**recursos-e-informacoes
* Servidor DNS guarda todos os domínios informados no navegador e faz a tradução dos mesmos
* erro de timeout significa que o servidor não está respondendo por que a porta não está aberta
* Portas: http : 80 ,  https: 443
* URL e URI são usados como sinônimos



#  Requisição e Resposta

* Sessão
  * Autenticação, Cockies (Hash de conteúdos salvos com dados de usuários no navegador)
  * O HTTP é um protocolo de requisição e resposta



# Depurando o método HTTP

* abrir a ferramenta de desenvolvedor   
* abrir a aba de Network (Visualiza as requisições e os métodos requisitados)
* Status Code   200, 404, 301, 500 ... 
  * 2XX -  Sucesso
  * 3XX - Redirecionamento
  * 4XX - Erro do cliente
  * 5XX - Erro do servidor



# Parâmetros de Requisição GET e POST

* Quando enviamos parâmetros URL devemos iniciar primeiro ?, nome do parametro e um = , para receber o nome do parãmetro e do seu valor.

  ```
  ?nome_do_parametro=seu_valor
  ```

* Quando usamos mais do que, um parâmetro devemos usar **`&`** para separá-los:

  ```
  ?nome_do_parametro=seu_valor&nome_do_outro_param=valor
  ```



# HTTP 



* GET  : receber dados

  * PARAMS: 'https://www.youtube.copm/results?search_quewry=Ayrton+Serna&SP=CAM%253D'
    * & concatena os parametros por ?

* POST:  submeter dados

* DELETE: Remover 

* PUT: Atualizar 

  

  

# Request-Response

* HTML, XML ou JSON

* Cabeçalho 'Accept' : Através dele podemos usar algum formato específico como:

  ```
  Accept: text/html, Accept: text/css, Accept: application/xml, Accept: application/json, Accept:image/jpeg e Accept: image/*
  Accept: */*
  ```

  

# Rest

* URI + METHOD
  * http://dominio/api/servico     - GET
  * http://dominio/api/servico     - POST
  * http://dominio/api/servico/id - PUT/PATCH
  * http://dominio/api/servico/id - DELETE
  * http://dominio/api/servico/id/parametro - GET
* OPERAÇÔES(GET , POST , PUT , DELETE)       HTTP   REPRESENTAÇÂO(JSON , XML , HTML)



# Requisitando Dados pelo Prompt de Comandos

* curl flag : comandos do prompt
* 

# Dados Binários

* http2 comprime os dados nos formatos GZIP para diminuir o trafego dos dados na rede
* Requisição e Resposta trafega na rede no formato em binário
* BINARIO + HPACK, compressão dos dados em binário
* O HTTP2 por padrão já criptografa(TLS) os dados binários

# Representação de uma requisição

* HTTP1 Cabeçalhos: Host , User-Agent, Accept, Accept-Language, Accept-Encoding.
* HTTP2 cabeçalhos podem variar de acordo com a requisição (HEADERS STATEFUL).
* HTTP2 SERVER PUSH envia dados sem precisar requisitar o mesmo.

# KEEP-ALIVE e Multiplexação

* HTTP1 utiliza o Protocolo TCP, sempre tem que abrir e fechar uma conexão.

* HTTP! utiliza o Keep-Alive, conta a quantidade de abertura de conexões.

* HTTP2 utiliza o Protocolo TCP mas sempre mantem a conexão em paralelo de maneira assíncrona MULTIPLEXING.

  