## Serviços Angular

Conforme a Arquitetura do Angular a utilização de Serviços tem o propósito de organizar o projeto, isolando lógica de negócio e separando-a dos Controllers. Podemos afirmar que há dois tipos de classes do tipo service, uma que gerencia dados internos referentes a aplicação e outra que gerencia dados externos da aplicação por comunicação com API´s, o que gera a necessidade do entendimento de como uma classe comum utiliza o serviço.

### Descrição

Utiliza do decorador **`@Injectable()`** para declarar a classe como sendo do tipo service assim como para declarar que a mesma poderá ser injetada em outra(s) classe(s) através de `injeção de dependência (DI)`

> @Injectable() 
 
Prove a clase como sendo um serviço. Possui seis metadados:

  - **provideIn (string)** - Determina quais injetores fornecerão o injetável. Fornece seis opções de configurações:

    - root (string) - Declara o service como sendo do tipo **`Singleton`**
  - **deps (any[])** - 
  - **useClass (Type<any>)** -
  - **useExisting (any)** -
  - **useFactory (function)** - 
  - **useValue (any)** -  
    
> Injeção de dependência
  
No construtor de uma classe criasse a instância de outras classes que podem ou não depender de outras classes, interfaces, etc... criando assim a formação de cascata de dependências. Trata-se de um padrão de design no qual uma classe solicita dependências de fontes externas em vez de criá-las. A estrutura de DI do Angular fornece dependências para uma classe na instanciação

#### Instância sem injeção de dependência

    // Classe de Serviço
    export class ExampleService {
        prop: PropEx;
        http: HttpService;

        constructor(http: HttpService) {
           this.http = new HttpService();
        }
    }

    // Classe Funcionalidade
    export class Example {
        exampleService: ExampleService;

        constructor() {
            this.exampleService = new ExampleService(...!!!...);  // Problema teria que instanciar ExampleService 
        }                                                         // passando algo do tipo HttpService
    }
  
#### Instância com injeção de dependência

    // Classe de Serviço
    export class ExampleService {
        prop: PropEx;

        constructor(private http: HtppService) {}

    export class Example {                                    OU      export class Example {
        exampleService: ExampleService;                                 constructor(private: exampleService: ExampleService) {}
                                                                      }
        constructor() {
            this.exampleService = new ExampleService();
        }
    }   

### Comunicação Interna de Dados

Podemos gerenciar dados existentes na aplicação e compartilhá-los em outros locais conforme faz necessário o seu uso. Diferentemente de services com dados `externos`, services com dados internos não são inseridos na propriedade **`providers: []`** dos componentes, sendo apenas injetados no construtor do componente para serem utilizados. 

> Exemplo de uso

#### Service

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
    
- [services](https://angular.io/guide/architecture-services)    
- [padrão singleton](https://angular.io/guide/singleton-services)
- [injecção de dependência](https://angular.io/guide/dependency-injection)  
  
  
    
    
    
    
    
    
    
    
    
    
