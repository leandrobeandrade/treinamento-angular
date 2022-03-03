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

Consiste na comunicação `bidirecional` de dados por proprieadade, comunicação esta entre Template/Classe e vice-versa através da diretiva **`ngModel`**, tendo a diretiva um evento epecífico chamado **ngModelChange()** que atualiza os valores referentes aquela propriedade quando a mesma sofre alguma alteração
    
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
      val: string;
    }
    
    // Componente Pai
    
    // HTML
    <hello name="{{ name }}"></hello>
    <hello name="{{ name }}"></hello>
    <hello name="{{ name }}"></hello>
    
    // Classe
    name = 'Fulano';

    // Accessando múltiplos elementos nativos DOM usando QueryList
    @ViewChildren(ChildComponent) childComponent: QueryList<ChildComponent>;

    ngAfterViewInit() {
      console.log('Olá ', childComponent.length);   // 3
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

      @ContentChildren('nameInput') children;

      ngAfterContentInit() {
        console.log('1 => ', this.nameVar.nativeElement);
        console.log('2 => ', this.nameVarAsNgModel.control);
        console.log('3 => ', this.nameVarAsElemRef.nativeElement);

        console.log('4 => ', this.children);
        console.log('5 => ', this.children.length);
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

Prove acesso a dados presentes em `Diretivas`, `Componentes filhos` e `Elementos no DOM`. Ao contrário do **@ViewChild()**, é utilizado para acessar vários elementos. A resposta da lista de elementos será sempre um **`QueryList`** que será atualizado sempre que qualquer elemento filho for adicionado, atualizado ou removido da árvore HTML DOM.

    @ContentChildren(myFatherComponent) myChildComponents;
    
#################
first: returns the first item in the list.
last: get the last item in the list.
length: get the length of the items.
changes: Is an observable. It emits a new value, whenever the Angular adds, removes or moves the child elements.
It also supports JavaScript array methods like map(), filter() , find(), reduce(), forEach(), some(). etc
################

### @Input()

Declara uma propriedade de entrada que você pode atualizar por meio de vinculação de propriedade. 

    @Input() myProperty: string;

    <my-cmp [myProperty]="someExpression">

### @Output()

Declara uma propriedade de saída que dispara eventos que você pode assinar com uma associação de evento, ver exemplo completo [aqui](https://www.google.com):

    myEvent = new EventEmitter();
    
    <my-cmp (myEvent)="doSomething()">
    
### @HostBinding()
Vincula uma propriedade de elemento de host **(classe `valid`)** a uma propriedade de diretiva/componente **isValid**, ver exemplo completo [aqui](https://www.google.com):

     @HostBinding('class.valid') isValid;

### @HostListener()
Inscreve-se em um evento de elemento de host **(`click`)** com um método de diretiva/componente **onClick**, passando opcionalmente um argumento `($event)`, ver exemplo completo [aqui](https://www.google.com):

    @HostListener('click', ['$event']) onClick(e) {
        ...
    }

> LINK DE REFERÊNCIA: https://angular.io/guide/cheatsheet
