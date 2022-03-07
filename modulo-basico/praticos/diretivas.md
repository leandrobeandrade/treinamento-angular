## Diretivas Padrão e Personalizdas

Diretivas são propriedades que são inseridas em tags HTML que proporcionam formas de controles tanto para dados como para elementos inseridos no DOM.

### Diretivas Padrão - Controle

São fornecidas pelo próprio Angular afim de proporcionar controle de diversas maneiras sobre elementos e dados do componente.

> ngIf

Remove ou recria uma parte da árvore DOM com base na expressão da condicional. Utilizada em conjunto com **`else`** juntamente com a diretiva `ng-template`

    // HTML
    <div *ngIf="control"> ... </div>
    
    <div *ngIf="control; else template"> ... </div>
    <ng-template #template> ... </ng-template>
    
    // Classe
    control: boolean;

> ngFor

Percorre dados **iteráveis** como arrays e registra cada elemento em uma variável de controle assim como um `index` para cada elemento
    
    // HTML
    <ul>
      <li *ngFor="let item of items; let i = index">{{ item.id }} - {{ item.name }}</li>
    </ul>
    
    // Classe
    items = [{ id: 1, name: 'Banana' }, { id: 2, name: 'Maçã' }, { id: 3, nome: 'Abacaxi' }];
    
**trackBy**
    
Uma função opcionalmente passada para a diretiva ngFor para personalizar como serão identificados itens exclusivos em um iterável. Permite atualizar dados na tela conforme o dado for atualizado não atualizando toda a lista. Informações a mais relacionadas a função [trackBy](https://stackoverflow.com/questions/49881607/angular-efficiently-using-trackby-with-ngfor).

    // HTML
    <div *ngFor="let item of items; trackBy:identify">
      Id: {{ item.id }} Name:{{ item.name }}
    </div>

    // Classe
    identify(index: number, item: Item): number {
      return item.id;
    }

> ngSwitch

Troca condicionalmente o conteúdo do div selecionando por um dos modelos incorporados com base no valor atual da condição

    // HTML
    <div [ngSwitch]="control">
      <ng-template [ngSwitchCase]="'A'">...</ng-template>
      <ng-template ngSwitchCase="B">...</ng-template>
      <ng-template ngSwitchDefault>...</ng-template>
    </div>
    
    // Classe
    control = 'A';

> ngPlural

Adiciona/remove subárvores DOM com base em um valor numérico. Sob medida para a pluralização
    
    // HTML
    <div [ngPlural]="value">
      <ng-template ngPluralCase="0">ZERO</ng-template>
      <ng-template ngPluralCase="1">UM</ng-template>
    </div>
    
    // Classe
    value = 0;
    
### Diretivas Padrão - Conteúdo

São fornecidas para controle de conteúdo e consequentemente de dados entre os componentes.

> ng-content

Utilizado para projeção de conteúdo, permite passar **qualquer** conteúdo entre as tags de abertura e fechamento do componente criado

    // Componente 1
    
    // HTML
    <div>
      <h1>
        <ng-content select="[title]"></ng-content>
      </h1>

      <p>
        <ng-content select="[description]"></ng-content>
      </p>
    </div>
    
    // Componente 2
    
    // HTML
    <app-comp1>
      <span title>Título</span>
      <span description>Description</span>
    </app-comp1>
    
**select**

Utilizado para referenciar e assim diferenciar elementos HTML para serem renderizados através de um `ng-content`.

> ng-template

Define um template que não é renderizado por padrão. Podendo ter sua definição no template HTML sendo feita direta ou indiretamente

    // HTML
    <div *ngIf="control; else template">
        <p>Alguma coisa ...</p>
    </div>
    
    <ng-template #template>
        <p>Outra coisa ...</p>
    </ng-template>
    
> ng-container

Funciona como um elemento especial que pode conter diretivas estruturais sem adicionar novos elementos ao DOM

    <div *ngIf="control" *ngFor="let item of items"> ... </div>     ERRO DE SINTAXE
    
    <ng-container *ngIf="control>
        <div  *ngFor="let item of items"> ... </div>     OK
    </ng-container>
    
> router-outlet

Atua como um espaço reservado que o Angular preenche dinamicamente com base no estado atual do roteador

    // HTML - Classe Pai
    <a routerLink="A">Página A</a>
    <a routerLink="B">Página B</a>

    <br />

    <a [routerLink]="[{ outlets: { secondRouter: ['C'] } }]">Página C</a>
    <a [routerLink]="[{ outlets: { secondRouter: ['D'] } }]">Página D</a>

    <router-outlet></router-outlet>
    <router-outlet name="secondRouter"></router-outlet>
    
    // Classes - Filhas
    @Component({
      selector: 'app-a',
      template: '<h1>Conteúdo Página A</h1>',
    })
    export class AComponent {}

    @Component({
      selector: 'app-b',
      template: '<h1>Conteúdo Página B</h1>',
    })
    export class BComponent {}

    @Component({
      selector: 'app-c',
      template: '<h1>Conteúdo Página C</h1>',
    })
    export class CComponent {}

    @Component({
      selector: 'app-d',
      template: '<h1>Conteúdo Página D</h1>',
    })
    export class DComponent {}
    
**OBS:** Mais sobre `router-outlet` no módulo intermediário sobre **`rotas`**
    
### Diretivas Personalizadas

Criam interações diversas entre elementos HTML inseridos no DOM, manipulando e controlando comportamentos e dados dos componentes.

#### Diretiva

    import { Directive, ElementRef, HostListener, Input, Renderer2 } from '@angular/core';

    @Directive({
      selector: '[appHighlight]'
    })
    export class HighlightDirective {
      @Input() appHighlight: string = '';
      @Input() defaultColor = 'orange';

      constructor(private el: ElementRef, private rend: Renderer2) { }

      @HostListener('mouseenter') onMouseEnter() {
        this.highlight(this.appHighlight || this.defaultColor  || 'red');
      }

      @HostListener('mouseleave') onMouseLeave() {
        this.highlight('');
      }

      private highlight(color: string) {
        this.rend.setStyle(this.el.nativeElement, 'backgroundColor', color);
      }
    }
    
#### Componente
 
    // HTML
    <div>
      <input type="radio" name="colors" (click)="color = 'lightgreen'" />Verde
      <input type="radio" name="colors" (click)="color = 'yellow'" />Amarelo
      <input type="radio" name="colors" (click)="color = 'cyan'" />Ciano
    </div>

    <h3 [appHighlight]="color">Passe o mouse</h3>

    <h3 [appHighlight]="color" defaultColor="violet">Passe o mouse</h3>
    
    // Classe
    color = '';
    
