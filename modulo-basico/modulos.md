# @NgModule()

> Define um módulo que contém componentes, diretivas, pipes e providers.

    import { NgModule } from '@angular/core';

    @NgModule ({
        declarações: [ ... ], 
        importações: [ ... ], 
        exportações: [ ... ], 
        provedores: [ ... ],
        entryComponents: [ ... ],
        bootstrap: [ ... ],
        schemas: [ ... ]
        id: '',
        jit: true
    })

    class MyModule {}

- **declarations? :** Lista de componentes, diretivas e pipes que pertencem a este módulo.

      [AppComponent, HelloComponent, MyDatePipe]

- **imports? :** Lista de módulos a serem importados para este módulo. Tudo desde módulos importados estão disponíveis para as declarações deste módulo.

      [CommonModule, BrowserModule, NgModule]

- **exports? :** Lista de componentes, diretivas e pipes visíveis para os módulos que importam este módulo.

      [AppComponent, MyDatePipe]

- **providers? :** Lista de provedores de injeção de dependência visíveis para o conteúdo deste módulo e para importadores deste módulo.

      [MyService, {provide: ...}]

- **entryComponents? :** Lista de componentes não referenciados em nenhum modelo alcançável, por exemplo, criado dinamicamente a partir do código.

      [ModalComponent, FormComponent]

- **bootstrap? :** Lista de componentes a serem inicializados quando este módulo for inicializado.

      [AppComponent]
       
- **schemas? :** O conjunto de esquemas que declara elementos permitidos no NgModule. Elementos e propriedades que não são componentes angulares nem diretivas devem ser declarados em um esquema. Os valores permitidos são **NO_ERRORS_SCHEMA** e **CUSTOM_ELEMENTS_SCHEMA**.

      schemas?: Array<SchemaMetadata | any[]>

- **id? :** Um nome ou caminho que identifica exclusivamente este NgModule em getModuleFactory. Se deixado indefinido, o NgModule não será registrado com getModuleFactory.

      id?: string

- **jit? :** Quando presente, este módulo é ignorado pelo compilador **AOT**. Ele permanece no código distribuído e o compilador JIT tenta compilá-lo em tempo de execução, no navegador. Para garantir o comportamento correto, o aplicativo deve importar **@angular/compiler**.

      jit?: true
      
> LINK DE REFERÊNCIA: https://angular.io/api/core/NgModule

