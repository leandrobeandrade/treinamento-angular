## Manipulação de Dados - Comunicação entre Template (HTML) e Componente (Classe)

### Binding

> Interpolação

Consiste na comunicação `unidirecional` de dados, comunicação esta sendo a partir da Classe para o Template

    // Classe
    example = 'Este texto será renderizado na tela!';
    
    // HTML
    <p>{{ example }}</p>
    
    // Saída
    <p>Este texto será renderizado na tela!</p>
    
> Property Binding

Consiste na comunicação `unidirecional` de dados, comunicação esta sendo a partir da Classe para o Template, porém, utilizado dentro de qualquer `tag` HTML válida que seja manipuladora de dados através de suas propriedades

    // HTML
    <input type="text" [value]="example" />
    
    // Classe
    example = 'Este texto será renderizado na tela!';
    
> Event Binding

Consiste na vinculação de eventos a partir do Template ligados à métodos da Classe. Eventos Html possuem `interfaces` que podem ser utilizadas na Classe, podendo-se ver qual interface corresponde aquele elemento pelo próprio log do console no browser 
    
    // HTML
    <button (click)="mostrarAlerta()">Mostrar alerta</button>
    
    this.input = <HTMLInputElement>(event.target).value;        // Dá para pegar o tipo no log do browser
    
    // Classe
    showAlert() {
      alert('Olá!');
    }
    
> Two-way Binding

Consiste na comunicação `bidirecional` de dados por propriedade, comunicação esta entre Template/Classe e vice-versa através da diretiva **`ngModel`**, tendo a diretiva um evento epecífico chamado **ngModelChange()** que atualiza os valores referentes aquela propriedade quando a mesma sofre alguma alteração
    
    // HTML
    <input type="text" [(ngModel)]="val" (ngModelChange)="changeVal($event)" />

    <p>{{ val }}</p>
    
    // Classe
    val = 'algo';
    
    changeVal(value) {
      console.log(value);
    }
    
> Template Variable

Variáveis de Template são utilizadas como armazenadores de dados relacionados ao Template, que podem ser utilizados para condições e validações de dados entre outras funcionalidades
    
    // HTML
    <input type="text" value="algo" #entrada_ (input)="entrada = entrada_.value" />

    <p>{{ entrada }}</p>
    
    // Classe
    entrada = 'Valor qualquer';
    
### @ViewChild()

Prove acesso a dados presentes em `Diretivas`, `Componentes filhos` e `Elementos no DOM`

> @ViewChild() com Diretivas

    // Diretiva
    @Directive(
      { selector: '[names]' }
    )
    export class NamesDirective {
      name = 'Ciclano';

      constructor(elem: ElementRef, renderer: Renderer2) {
        let otherName = renderer.createText('Saudações ');
        renderer.appendChild(elem.nativeElement, otherName);
      }
    }

    // Outro Componente
    
    // HTML
    <span names>Fulano</span>     =>      <span names>Saudações Fulano</span>

    // Classe
    extraName: string;

    @ViewChild(NamesDirective)
    set name(directive: NamesDirective) {
      this.extraName = directive.name;
    };

    ngAfterViewInit() {
      console.log(this.extraName);    // Ciclano
    }
    
    <span names>Fulano</span>     =>      <span names>Saudações Ciclano</span>

> @ViewChild() com Componente Filho

    // Componente Filho
    @Component({
      selector: 'child-app',
      template: '',
    })
    export class ChildComponent {
      val = 'Alguma coisa';
      
      exec(): string {
        return 'Componente Filho';
      }
    }

    // Componente Pai
    @Component({
      selector: 'parent-app',
      template: `<child-app><child-app>`,
    })
    export class ParentComponent implements AfterViewInit {

      // ContentChild
      @ViewChild(ChildComponent) childComponent: ChildComponent;

      ngAfterViewInit() {
        console.log(this.childComponent.name);      Alguma coisa
        console.log(this.childComponent.exec());    Componente Filho
      }
    }
    
> @ViewChild() com Elementos DOM

    // HTML
    <input #inp>
    
    // Classe
    @ViewChild('inp') input: ElementRef;
    
    ngAfterViewInit() {
      this.someInput.nativeElement.value = 'Wowww...!';
    }

### @ViewChildren()

Prove acesso a dados presentes em `Diretivas`, `Componentes filhos` e `Elementos no DOM`. Ao contrário do **@ViewChild()**, é utilizado para acessar vários elementos. A resposta da lista de elementos será sempre um **`QueryList`** que será atualizado sempre que qualquer elemento filho for adicionado, atualizado ou removido da árvore HTML DOM.

    // Componente Filho
    @Component({
      selector: 'child-app',
      template: `<h1>Olá {{ val }}!</h1>`,
    })
    export class ChildComponent {
      @Input() val: string;
    }
    
    // Componente Pai
    
    // HTML
    <child-app val="{{ name }}"></child-app>
    <child-app val="{{ name }}"></child-app>
    <child-app [val]="name"></child-app>
    
    // Classe
    name = 'Fulano';

    // Accessando múltiplos elementos nativos DOM usando QueryList
    @ViewChildren(ChildComponent) childComponent: QueryList<ChildComponent>;

    ngAfterViewInit() {
      console.log(childComponent.length);   // 3
    }
    
    
### @ContentChild()

Prove acesso a dados presentes em `Diretivas`, `Componentes filhos` e `Elementos no DOM`. Diferentemente do **@ViewChild()** consegue manipular elementos dentro das diretivas `ng-template`

    // Componente filho
    @Component({
      selector: 'app-input',
      template: `
        <ng-content></ng-content>
      `,
    })
    export class InputComponent {
      @ContentChild('nameInput', { static: false }) nameVar;
      @ContentChild('nameInput', { static: false, read: NgModel }) nameVarAsNgModel;
      @ContentChild('nameInput', { read: ElementRef }) nameVarAsElemRef;

      ngAfterContentInit() {
        console.log('1 => ', this.nameVar.nativeElement);
        console.log('2 => ', this.nameVarAsNgModel.control);
        console.log('3 => ', this.nameVarAsElemRef.nativeElement);
      }
    }
    
    // Componente Pai
    
    // HTML
    <app-input>
      <input #nameInput [(ngModel)]="input1" />
    </app-input>

    <br /> <br>

    <app-input>
      <input #nameInput [(ngModel)]="input2" />
    </app-input>
    
    // Classe
    input1 = 'Fulano';
    input2 = 'Ciclano';

### @ContentChildren()

Prove acesso a dados presentes em `Diretivas`, `Componentes filhos` e `Elementos no DOM`. Ao contrário do **@ContentChild()**, é utilizado para acessar vários elementos. A resposta da lista de elementos será sempre um **`QueryList`** que será atualizado sempre que qualquer elemento filho for adicionado, atualizado ou removido da árvore HTML DOM.

    // Componente Message
    @Component({
      selector: 'app-message',
      template: '<p>{{ message }}</p>',
    })
    export class MessageComponent {
      @Input() message: string;
    }
    
    // Componente MessageContainer
    @Component({
      selector: 'app-message-container',
      template: `
        <h3>{{ greetMessage }}</h3>
        <ng-content></ng-content>
      `,
    })
    export class MessageContainerComponent implements AfterContentInit {
      greetMessage = 'Componente message-container';

      @ContentChildren(MessageComponent)
      messageComponent: QueryList<MessageComponent>;

      ngAfterContentInit() {
        this.messageComponent.forEach((m, i) => {
          m.message = `Componente message: ${i + 1}`;
        });

        console.log(this.messageComponent.length);
      }
    }
    
    // Componente Pai
    @Component({
      selector: 'my-app',
      template: `
        <app-message-container>
          <app-message></app-message>
          <app-message></app-message>
        </app-message-container>
      `,
    })
    export class AppComponent {}

> Propriedades

- **selector -** O tipo de diretiva ou o nome usado para consulta.
- **read -** Usado para ler um token diferente dos elementos consultados.
- **static -** True resolve os resultados da consulta antes da execução da detecção de alterações e false resolve depois. O padrão é falso.

> Propriedades **QueryList**

- **first -** retorna o primeiro item da lista
- **last -** obtém o último item da lista
- **length -** obtém o comprimento dos itens
- **changes -** É um observável. Ele emite um novo valor, sempre que o Angular adiciona, remove ou move os elementos filhos
- Também suporta métodos padrões de array JavaScript como map(), filter(), find(), reduce(), forEach(), some(), etc...

### @Input()

Declara uma propriedade de entrada que você pode atualizar por meio de vinculação de propriedade **`property binding`**. Prove comunicação dos dados **`de`** componentes pais **`para`** componentes filhos e diretivas. Recebe dados.

    // Componente filho
    
    // HTML
    <p>O item atual é: {{ item }}</p>
    
    // Classe
    export class ChildComponent {
      @Input() item: string;
    }
    
    // Componente Pai
    
    // HTML
    <app-child [item]="currentItem"></app-child>
    
    // Classe
    export class AppComponent {
      currentItem = 'Geladeira';
    }
    
    // Saída
    <p>O item atual é: Geladeira</p>
    
> Utiliza o ciclo de vida **onChanges** para observar alterações na propriedade

> Para se utilizar uma propriedade com nome no template diferente do da classe basta passar o nome que será utlizado no template entre os parentêses

### @Output()

Declara uma propriedade de saída que dispara eventos que você pode assinar com uma associação de evento. Prove comunicação de dados **`de`** componentes filhos e diretivas **`para`** componentes pais. Envia dados

    // Componente filho
    
    // HTML
    <button (click)="sendMsg('Olá Mundo!')">Enviar mensagem</button>
    
    // Classe
    export class ChildComponent {
      @Output() msg = new EventEmitter<string>();
    }
    
    sendMsg(val: string) {
      this.msg.emit(val);
    }

    // Componente Pai
    
    // HTML
    <app-child (msg)="showMessage($event)"></app-child>

    // Classe
    export class AppComponent {
      
      showMessage(val: string) {
        console.log(val);           // Olá Mundo!
      }
    }
    
### @HostBinding()

Declara uma associação de propriedade de host que teram verificadas automaticamente as associações de propriedade do host durante a detecção de alterações. Se uma vinculação for alterada, ela atualizará o elemento host da diretiva.

    

### @HostListener()

Declara um ouvinte de host que invocará o método decorado quando o elemento host emitir o evento especificado, o ouvinte escutará o evento emitido pelo elemento host que é declarado.

    // Diretiva
    @Directive({
      selector: '[hostListen]'
    })
    export class HtlisDirective {

      constructor(private el: ElementRef, private renderer: Renderer2) {
        renderer.setStyle(el.nativeElement, 'backgroundColor', 'gray');
      }

      @HostListener('mouseover') onMouseOver() {
        const part = this.el.nativeElement.querySelector('.card-text');
        this.renderer.setStyle(part, 'display', 'block');
      }

      @HostListener('mouseout') onMouseOut() {
        const part = this.el.nativeElement.querySelector('.card-text');
        this.renderer.setStyle(part, 'display', 'none');
      }

    }
    
    // Componente
    <div class="card card-block" hostListen>
      <h4 class="card-title">Título</h4>
      <p class="card-text" [style.display]="'none'">Conteúdo</p>
    </div>

> LINK DE REFERÊNCIA: https://angular.io/guide/cheatsheet
