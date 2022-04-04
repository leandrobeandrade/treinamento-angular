## Serviços Angular

Conforme a Arquitetura do Angular a utilização de Serviços tem o propósito de organizar o projeto, isolando lógica de negócio e separando-a dos Controllers. Podemos afirmar que há dois tipos de classes do tipo service, uma que gerencia dados internos referentes a aplicação e outra que gerencia dados externos da aplicação por comunicação com API´s, o que gera a necessidade do entendimento de como uma classe comum utiliza o serviço.

### Descrição

Utiliza do decorador **`@Injectable()`** para declarar a classe como sendo do tipo service assim como para declarar que a mesma poderá ser injetada em outra(s) classe(s) através de `injeção de dependência (DI)`.

> @Injectable() 
 
Prove a classe como sendo um serviço. Possui seis metadados:

  - **provideIn (string)** - Determina quais injetores fornecerão o injetável. Fornece cinco opções de configurações:

    - `root (string)` - Declara o service como sendo do tipo **`Singleton`**.
    - `platform (string)` - Um injetor de plataforma singleton especial compartilhado por todos os aplicativos na página.
    - `any (string)` - Fornece uma instância única em cada módulo carregado lentamente, enquanto todos os módulos carregados lazy compartilham uma instância.
    - `null` - Equivalente a undefined. O injetável não é fornecido em nenhum escopo automaticamente e deve ser adicionado a um providers de um @NgModule , @Component ou @Directive .
    - `Type(any)` - Associa o injetável a um ou outro @NgModuleInjectorType.
  - **deps (any[])** - Recebe um array de classes do tipo service | ExampleService
  - **useClass (Type< any >)** - Recebe uma classe do tipo service | ExampleService
  - **useExisting (any)** - Configura o service para retornar um valor de outro através de um token.
  - **useFactory (function)** - Recebe uma função do service | (ServiceExample) => ServiceExample.getLocale()
  - **useValue (any)** - Recebe um valor para representar algum argumento que o service necessita para funcionar  | 'pt-BR'
    
> Injeção de dependência
  
No construtor de uma classe criasse a instância de outras classes que podem ou não depender de outras classes, interfaces, etc... criando assim a formação de cascata de dependências. Trata-se de um padrão de design no qual uma classe solicita dependências de fontes externas em vez de criá-las. A estrutura de DI do Angular fornece dependências para uma classe na instanciação

#### Instância sem injeção de dependência

    // Classe de Serviço
    export class ExampleService {

        constructor(http: HttpService) {}
  
        this.http.get()...
    }

    // Classe de Funcionalidade
    export class Example {
        exampleService: ExampleService;

        constructor() {
            this.exampleService = new ExampleService(...!!!...);      // Problema - ao instanciar ExampleService 
        }                                                             // teria que passar algo do tipo HttpService
    }
  
#### Instância com injeção de dependência

    // Classe de Serviço
    export class ExampleService {
  
        constructor(private http: HtppService) {}

    // Classe de Funcionalidade
    export class Example {                                    OU      export class Example {
        exampleService: ExampleService;                                 constructor(private: exampleService: ExampleService) {}
                                                                      }
        constructor() {
            this.exampleService = new ExampleService();
        }
    }   

### Comunicação Interna de Dados (Broadcast)

Podemos gerenciar dados existentes na aplicação e compartilhá-los em outros locais conforme faz necessário o seu uso. Diferentemente de services com dados `externos`, services com dados internos não são inseridos na propriedade **`providers: []`** dos componentes, sendo apenas injetados no construtor do componente para serem utilizados. Para utilização do service interno é necessário introduzi-lo no **providers** de módulo a utilizar o serviço ou no próprio serviço no metadado **provideIn** declarar como `root`.

> Exemplo de uso

#### Classe de Serviço

    import { Injectable } from '@angular/core';
    import { BehaviorSubject } from "rxjs";

    @Injectable({
      providedIn: 'root'
    })
    export class ModalStatusService {
      public readonly modalOpened: BehaviorSubject<boolean> = new BehaviorSubject<boolean>(false);
      public readonly modalOpened$ = this.modalOpened.asObservable();

      constructor() { }

      setModalStatus(opened: boolean) {
        this.modalOpened.next(opened);
      }
    }
    
#### Classe que seta o(s) dado(s) a serem compartilhado(s)
    ...
    constructor(private modalStatusService: ModalStatusService) {}
    
    ...
    
    this.modalStatusService.setModalStatus(true);

#### Classe que obtem o(s) dado(s) atualizado(s) e executa lógica sobre o valor desse(s) dado(s)
    ...
    constructor(private modalStatusService: ModalStatusService) {}
    
    loadData() {
      ...
    }
    
    ...
    
    refreshDados() {
      this.modalStatusService.modalOpened$.subscribe((status) => {
        if(status) this.loadData();
      })
    }
 
 ### Comunicação Externa de Dados
 
Prove comunicação `bidirecional` de dados entre a aplicação e um servidor. O método assíncrono envia uma solicitação HTTP e retorna um Observable que emite os dados solicitados quando a resposta é recebida. O tipo de retorno varia com base nos valores observe e responseType que você passa para a chamada.
 
> Exemplo de uso
  
#### Classe de Serviço  
 
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';

    @Injectable()
    export class ConfigService {
      configUrl = 'https://api-example';
  
      constructor(private http: HttpClient) {}

      getConfig(): Observable<any> {
        return this.http.get<any>(this.configUrl);
      }
    }
  
#### Classe de funcionalidade
  
    constructor(private configService: ConfigService) {}
 
    config: Config;

    showConfig() {
      this.configService.getConfig().subscribe((data: Config) => this.config = data);
    }  
    
| Referências|
| - |

- [services](https://angular.io/guide/architecture-services)    
- [padrão singleton](https://angular.io/guide/singleton-services)
- [injecção de dependência](https://angular.io/guide/dependency-injection)
- [injecção de dependência em ação](https://angular.io/guide/dependency-injection-in-action)
