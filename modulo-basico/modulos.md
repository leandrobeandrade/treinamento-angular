## Módulo Angular

Arquivo responsável por gerenciar toda a estrutura dos componentes (classe, serviços, diretivas) internos, assim como bibliotecas externas.

### Descrição @NgModule()

> Define um módulo que contém componentes, diretivas, pipes e providers

    import { NgModule } from '@angular/core';

    @NgModule ({
        declarations: [ ... ], 
        imports: [ ... ], 
        exports: [ ... ], 
        providers: [ ... ],
        entryComponents: [ ... ],
        bootstrap: [ ... ],
        schemas: [ ... ]
        id: '',
        jit: true
    })

    class MyModule {}

- **declarations?:** Lista de componentes, diretivas e pipes que pertencem a este módulo.

      [AppComponent, HelloComponent, MyDatePipe]

- **imports?:** Lista de módulos a serem importados para este módulo. Tudo desde módulos importados estão disponíveis para as declarações deste módulo.

      [CommonModule, BrowserModule, NgModule]

- **exports?:** Lista de componentes, diretivas e pipes visíveis para os módulos que importam este módulo.

      [AppComponent, MyDatePipe]

- **providers?:** Lista de provedores de injeção de dependência visíveis para o conteúdo deste módulo e para importadores deste módulo.

      [MyService, {provide: ...}]

- **entryComponents?:** Lista de componentes não referenciados em nenhum modelo alcançável, por exemplo, criado dinamicamente a partir do código.

      [ModalComponent, FormComponent]

- **bootstrap?:** Lista de componentes a serem inicializados quando este módulo for inicializado.

      [AppComponent]
       
- **schemas?:** O conjunto de esquemas que declara elementos permitidos no NgModule. Elementos e propriedades que não são componentes angulares nem diretivas devem ser declarados em um esquema. Os valores permitidos são **NO_ERRORS_SCHEMA** e **CUSTOM_ELEMENTS_SCHEMA**.

      schemas?: Array<SchemaMetadata | any[]>

- **id?:** Um nome ou caminho que identifica exclusivamente este NgModule em getModuleFactory. Se deixado indefinido, o NgModule não será registrado com getModuleFactory.

      id?: string

- **jit?:** Quando presente, este módulo é ignorado pelo compilador **AOT**. Ele permanece no código distribuído e o compilador JIT tenta compilá-lo em tempo de execução, no navegador. Para garantir o comportamento correto, o aplicativo deve importar **@angular/compiler**.

      jit?: true
      
### Sharing Module

A criação de módulos compartilhados permite organizar e otimizar o código. Você pode colocar diretivas, pipes e componentes comumente usados em um módulo e, em seguida, importar apenas esse módulo sempre que precisar em outras partes de seu aplicativo.

    import { CommonModule } from '@angular/common';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    import { CustomerComponent } from './customer.component';
    import { NewItemDirective } from './new-item.directive';
    import { OrdersPipe } from './orders.pipe';

    @NgModule({
     imports:      [ CommonModule ],
     declarations: [ CustomerComponent, NewItemDirective, OrdersPipe ],
     exports:      [ CustomerComponent, NewItemDirective, OrdersPipe, CommonModule, FormsModule ]
    })
    export class SharedModule { }
    
Ao reexportar `CommonModule` e `FormsModule`, qualquer outro módulo que importe **SharedModule**, obtém acesso a diretivas como *NgIf* e *NgFor* do CommonModule e pode vincular a propriedades do componente com, uma diretiva no arquivo.

#### Exemplo:

- 1 - Declarar e exportar o componente em um módulo
    
      ...
      declarations: [ ..., AlgumComponent ],
      exporst: [ ..., AlgumComponent ]
      
      export class ExemploModule

- 2 - Importar no outro módulo o módulo que foi declarado o componente a ser compartilhado
      
      ...
      imports: [ ..., ExemploModule ]
      
      export class OutroModule
      
|Referências|
|-|

- [ngModule](https://angular.io/api/core/NgModule)
- [sharing modules](https://angular.io/guide/sharing-ngmodules)

