## Diretivas Angular

Arquivo responsável por gerenciar elementos do DOM virtual Angular. Uma Diretiva Angular pode ser considerada como um Componente sem template (HTML).

### Descrição @Directive

> Decorator que marca uma classe como uma diretiva Angular. Você pode definir suas próprias diretivas para anexar comportamento personalizado a elementos no DOM.

- **selector? -** O seletor CSS que identifica esta diretiva em um modelo e dispara a instanciação da diretiva:

      selector?: string

    - `name_element:` Seleciona pelo nome do elemento.
    - `.class:` Seleciona pelo nome da classe.
    - `[atribute]:` Seleciona pelo nome do atributo.
    - `[attribute=value]:` Seleciona pelo nome e valor do atributo.
    - `:not (sub_selector):` Seleciona apenas se o elemento não corresponder ao sub_selector.
    - `selector1, selector2:` Seleciona se seletor1 ou seletor2 corresponder.

      > Angular só permite que as diretivas sejam aplicadas em seletores CSS que não ultrapassam os limites dos elementos.
      >Para o seguinte modelo de HTML, uma diretiva com um seletor de entrada **[type=text]** seria instanciada apenas no elemento <input type = "text">.

- **inputs? -** Enumera o conjunto de propriedades de entrada vinculadas a dados para uma diretiva:

      inputs?: string[]
      
    > O Angular atualiza automaticamente as propriedades de entrada durante a detecção de alterações. A propriedade inputs define um conjunto de **diretivaProperty** para a configuração **bindingProperty**.

    - `directivaProperty:` especifica a propriedade do componente onde o valor é escrito.
    - `bindingProperty:` especifica a propriedade DOM de onde o valor é lido.

      > Quando bindingProperty não é fornecido, ele é considerado igual a directiveProperty. O exemplo a seguir cria um componente com duas propriedades vinculadas a dados.
      
          @Component({
            selector: 'bank-account',
            inputs: ['bankName', 'id: account-id'],
            template: `
              Bank Name: {{bankName}}
              Account Id: {{id}}
            `
          })
          class BankAccount {
            bankName: string;
            id: string;
          }

- **outputs? -** Enumera o conjunto de propriedades de saída associadas ao evento:

      outputs?: string[]
      
     > Quando uma propriedade de saída emite um evento, um manipulador de eventos anexado a esse evento no modelo é chamado.
     > A propriedade outputs define um conjunto de **diretivaProperty** para a configuração **bindingProperty**:

     - `directivaProperty:` especifica a propriedade do componente que emite eventos.
     - `bindingProperty:` especifica a propriedade DOM à qual o manipulador de eventos está anexado.
     
      @Component({
        selector: 'child-dir',
        outputs: [ 'bankNameChange' ]
        template: `<input (input)="bankNameChange.emit($event.target.value)" />`
      })
      class ChildDir {
        bankNameChange: EventEmitter<string> = new EventEmitter<string>();
      }

      @Component({
        selector: 'main',
        template: `
          {{ bankName }} <child-dir (bankNameChange)="onBankNameChange($event)"></child-dir>
        `
      })
      class MainComponent {
        bankName: string;

        onBankNameChange(bankName: string) {
          this.bankName = bankName;
        }
      }

- **providers? -** Configura o injetor desta diretiva ou componente com um token que mapeia para um provedor de uma dependência:

      providers?: Provider[]

- **exportAs? -** Define o nome que pode ser usado no modelo para atribuir esta diretiva a uma variável:

      exportAs?: string
      
      @Directive({
        selector: 'child-dir',
        exportAs: 'child'
      })
      class ChildDir { ... }

      @Component({
        selector: 'main',
        template: `<child-dir #c="child"></child-dir>`
      })
      class MainComponent { ... }

- **queries? -** Configura as consultas que serão injetadas na diretiva:

      queries?: {
        [key: string]: any;
      }
      
     > As consultas de conteúdo são definidas antes que o retorno de chamada **ngAfterContentInit** seja chamado. As consultas de visualização são definidas antes que o retorno de chamada **ngAfterViewInit** seja chamado.
     > O exemplo a seguir mostra como as consultas são definidas e quando seus resultados estão disponíveis em ganchos de ciclo de vida:
     
      @Component({
      selector: 'someDir',
        queries: {
          contentChildren: new ContentChildren(ChildDirective),
          viewChildren: new ViewChildren(ChildDirective)
        },
        template: '<child-directive></child-directive>'
      })
      class SomeDir {
        contentChildren: QueryList<ChildDirective>,
        viewChildren: QueryList<ChildDirective>

        ngAfterContentInit() {
          // contentChildren está definido
        }

        ngAfterViewInit() {
          // viewChildren está definido
        }
      }

- **host? -** Mapeia as propriedades da classe para hospedar ligações de elemento para propriedades, atributos e eventos, usando um conjunto de pares de chave-valor:

      host?: {
        [key: string]: string;
      }
      
     > O Angular verifica automaticamente as ligações de propriedade do host durante a detecção de alterações. Se uma ligação muda, o Angular atualiza o elemento de host da diretiva. Quando a chave é uma propriedade do elemento host, o valor da propriedade é propagado para a propriedade DOM especificada.

     > Quando a chave é um atributo estático no DOM, o valor do atributo é propagado para a propriedade especificada no elemento host.

     Para manipulação de eventos:

     > A chave é o evento DOM que a diretiva escuta. Para ouvir eventos globais, adicione o destino ao nome do evento. O alvo pode ser a janela, o documento ou corpo.
O valor é a instrução a ser executada quando o evento ocorrer. Se a instrução for avaliada como falsa, **preventDefault** será aplicado ao evento DOM. Um método manipulador pode se referir à variável local **$event**.

- **jit? -** Quando presente, esta diretiva/componente é ignorada pelo compilador **AOT**. Ele permanece no código distribuído e o compilador JIT tenta compilá-lo em tempo de execução, no navegador. Para garantir o comportamento correto, o aplicativo deve importar **@angular/compiler**:

      jit?: true

> LINK DE REFERÊNCIA: https://angular.io/api/core/Directive
