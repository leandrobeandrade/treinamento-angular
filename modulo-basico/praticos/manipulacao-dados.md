## Manipulação de Dados - Comunicação entre Template (HTML) e Componente (Classe)


### Propriedades de Classe

- Eventos Html possuem interfaces para serem utilizadas. Ex: this.input = <HTMLInputElement>(event.target).value; / Dá para pegar o tipo no log do browser
- ngModel refere-se a uma propriedade no ts. Ex: prop
- ngModelChange atualiza sempre que esse model (propriedade) sofrer mudança de valor. Ex: (ngModelChange)="prop = event"
- No metadado @Input() o valor dentro dos parênteses serve para expor essa propriedade com um nome diferente da variável. Ex: @Input('cursos') nomeCursos: string
- No metadado @Component tbm é possível declarar variáveis do tipo @Input com o nome utilizado externamente. Ex: inputs: ['cursos']
- EventEmitter pode ser utilizado no HTML como um evento personalizado. Ex: <input setar="algo($event)"> @Output() setar = new EventEmitter() algo(ev) { }  
- O metadado @ViewChild() é utilizado para pegar elementos no template @ViewChild('element') el: HTMLElement / <input #element />
- O metadado @ViewChild() pode ter a variável relacionada a dois tipos de interface HTMLElement e ElementRef que acessa nativeElement
- O metadado @ViewChildren() é utilizado para pegar elementos no template @ViewChild('element') el: HTMLElement / <input #element />
- O metadado @ViewChildren() pode ter a variável relacionada a dois tipos de interface HTMLElement e ElementRef que acessa nativeElement
  
      // ngSwitch
      <div [ngSwitch]="control">
        <p *ngSwitchCase="true">1</p>
        <p *ngSwitchCase="false">2</p>
        <p *ngSwitchDefault>3</p>
      </div>

      // ngClass > class
      <p [class.my_class1]="control === true>...</p>
      <p [class.my_class2]="control === false>...</p>


      <div [ngClass]={'my_class1': control === true, 'my_class2' : control === false }">...</div>
      <div [ngClass]="{true : 'my_class1', false : 'my_class2'}[control]"></div>
      <div [ngClass]="control === true ? 'my_class1' : 'my_class2'"></div>

      // ngStyle > style
      <button [style.backgroundColor]="control ? 'blue' : 'red'"  [style.fontSize]="size + 'px''">Clicar<button>
      <button [ngStyle]="{'backgroundColor': (control ? 'blue' : 'red'), 'fontSize': size + 'px'}">Clicar<button>
                                                                                              
                                                                                                
- O seletor de uma diretiva pode receber um elemento em específico, bastando informar a tag, tbm serve para componentes. Ex: @directive({ selector: p[mudaCor] })
- ElementRef acessa a refrência de elementos no DOM pelo propriedade nativeElement, sendo necessário injetar no construtor o ElementRef porém não é recomendado

        this._elementRef.nativeElement.style.backgroundColor = 'red';

- No lugar de utilizar ElementRef pode se utilizar Renderer que tbm é injetado

        this._renderer.setElementStyle(this._elementRef.nativeElement, 'background-color', 'red');

- @HostListener escuta o evento relacionado ao elemento hospedeiro

        @HostListener('focus') onFocus() {
            this.renderer.setStyle(this._elementref.nativeElement, 'background-color', 'yellow');
        }

- @HostBinding faz a assosiação entre o elemento HTML e um atributo da diretiva

        @HostBinding('style.backgroundColor') backgroundColor: string;

                                                                                              
                                                                                          
