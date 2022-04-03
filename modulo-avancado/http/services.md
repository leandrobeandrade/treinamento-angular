## Serviços

Para implementação de services que fazem integração com as API's, utilizamos classes utilitárias implementadas na aplicação que auxiliam e abstraem boa parte da lógica 
necessária para a integração dos dados. Dentre estes arquivos os principais são:

- Serializable
- ApiServiceOld
- IResponseOld

### Serializable

Classe responsável por serializar e transformar os dados comunicados entre cliente e servidor para um formato em comum e que corresponda o esperado por ambos os lados. Também implementa propriedades comuns a praticamente todos os models, não sendo necessário declarar nos models estas propriedades já implementadas nesta classe como por exemplo *`id`*. 

**OBS:** Todos os models devem estender esta classe se for optado pela utilização do *ApiServiceOld*.

### ApiServiceOld

Classe responsável por realizar as chamadas aos métodos do pacote **http** do Angular, realizando assim a comunicação prática entre o cliente e o servidor. Implementa todos os verbos diponíveis para utilização de protocolos `HTTPRequest` JavaScript, além de configurações de proxy e tratamento dos loaders que demonstram visualmente em tela o progresso das requisições.

### IResponseOld

Classe reponsável pelo formato do retorno processado pelas requisições. Implementas propriedades tangíveis a cada tipo de requisição especificando o que cada uma possui no seu retorno, assim como propriedades que auxiliam por parte do cliente na utilização destas repostas, exemplo propriedade *`length`* que retorna a quantidade de registros existentes naquela requisição.

> Todos este métodos apesar de possuirem a marcação **deprecated**, funcionam perfeitamente, embora podem não serem utilizados se a opção da integração for a nova implementação do ApiService que pode ser vista [aqui](https://gitlab.com/ekaizen1/ekaizen-frontend-redo/-/wikis/api/Usando-novo-ApiService).

#### Descrições gerais

- Todos os serviços de integração com API's devem estar localizados na pasta **`api`** dentro da respectiva ferramenta
- Todos os serviços devem estender uma classe abstrata com métodos abstratos pré determinados
- Todos os serviços devem ser declarados dentro dos **`providers`** da classe utilitária que utilizar o serviço
- Todos os serviços devem ser tipados com sua respectiva classe modelo para prevenção de possíveis inconsistências de dados
