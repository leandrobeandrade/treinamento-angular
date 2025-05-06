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

> Todos este métodos apesar de possuirem a marcação **deprecated**, funcionam perfeitamente, embora podem não serem utilizados se a opção da integração for a nova implementação do ApiService que pode ser vista.

#### Descrições gerais

- Todos os serviços de integração com API's devem estar localizados na pasta **`api`** dentro da respectiva ferramenta
- Todos os serviços devem estender uma classe abstrata com métodos abstratos pré determinados
- Todos os serviços devem ser tipados com sua respectiva classe modelo para prevenção de possíveis inconsistências de dados
- A url base deve estar em consenso com a versão definida pela API naquele momento
- Todos os serviços devem ser declarados dentro dos **`providers`** da classe utilitária que utilizar o serviço

### Integração de dados localmente e via IBM

Existe duas possibilidades para a execução de requisições para a API's, rodar as API's localmente e realizar as chamadas ou apenas realizar as chamadas com as API's já publicadas na IBM.

> Rodar localmente

Nos serviços que forem rodar API's localmente faz-se necessário inserir o valor **`true`** como parâmetro dos métodos que fazem a conexão e seguir os passos a seguir

- Baixar e configurar a API na máquina com todas as entradas e variáveis de ambiente
- Posuir VPN instalada e configurada na máquina
- Rodar VPN e consequentemente a API

Exemplo de serviço consumindo API localmente

    addUser(categories: IUserModel): Observable<IResponseOld<IUserModel>> {
      return this.apiService
        .crudeAdd({
          url: this.url,
          payload: categories.toJSON(),
        }, true)                                                 // true - Automaticamente conectara com as API's rodando na máquina
        .pipe(
          map((res) => {
            return new IResponseOld<IUserModel>(res, IUserModel);
          })
        );
    }

> Via IBM

Para se integrar dados cuja API's já foram publicadas basta apenas declarar os endpoints com os métodos que implementam os verbos *`HTPP`* correspondentes, sem passar o valor **true**

