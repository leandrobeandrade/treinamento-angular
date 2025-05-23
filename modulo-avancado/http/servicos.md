## Serviços

Para implementação de services que fazem integração com as API's, utilizamos classes utilitárias implementadas na aplicação que auxiliam e abstraem boa parte da lógica 
necessária para a integração dos dados. Dentre estes arquivos os principais são:

- Serializable ??
- ApiServiceOld
- IResponseOld

### Serializable (~~não mais utilizada~~)

Classe responsável por serializar e transformar os dados comunicados entre cliente e servidor para um formato em comum e que corresponda o esperado por ambos os lados. Também implementa propriedades comuns a praticamente todos os models, não sendo necessário declarar nos models estas propriedades já implementadas nesta classe como por exemplo *`id`*.

### ApiService

Classe responsável por realizar as chamadas aos métodos do pacote **http** do Angular, realizando assim a comunicação prática entre o cliente e o servidor. Implementa todos os verbos diponíveis para utilização de protocolos `HTTPRequest` JavaScript, além de configurações de proxy e tratamento dos loaders que demonstram visualmente em tela o progresso das requisições.

### IResponse

Classe reponsável pelo formato do retorno processado pelas requisições. Implementas propriedades tangíveis a cada tipo de requisição especificando o que cada uma possui no seu retorno, assim como propriedades que auxiliam por parte do cliente na utilização destas repostas, exemplo propriedade *`length`* que retorna a quantidade de registros existentes naquela requisição.

#### Descrições gerais

- Todos os serviços de integração com API's devem estar localizados na pasta **`api`**
- Todos os serviços devem estender uma classe abstrata com métodos abstratos pré determinados
- Todos os serviços devem ser tipados com sua respectiva classe modelo para prevenção de possíveis inconsistências de dados
- A url base deve estar em consenso com a versão definida pela API naquele momento
- Todos os serviços devem ser declarados dentro dos **`providers`** da classe utilitária que utilizar o serviço
