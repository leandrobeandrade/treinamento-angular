## Diretivas Padrão e Personalizdas

Diretivas são propriedades que são inseridas em tags HTML que proporcionam maneiras de controles tanto para dados como para elementos inseridos no DOM.

> Diretivas Padrão

São fornecidas pelo próprio Angular afim de proporcionar controle de diversos modos e maneiras de elementos e dados do componente

### ngIf

Remove ou recria uma parte da árvore DOM com base na expressão da condicional. Utilizada em conjunto com **`else`** juntamente com a diretiva `ng-template`.

    // HTML
    <div *ngIf="control"> ... </div>
    
    <div *ngIf="control; else template"> ... </div>
    <ng-template #template> ... </ng-template>
    
    // Classe
    control: boolean;

### ngFor

Percorre dados iteráveis como arrays e registra cada elemento em uma variável de controle assim também com um index.
    
    // HTML
    <ul>
      <li *ngFor="let item of items">{{ item.id }} - {{ item.name }}</li>
    </ul>
    
    // Classe
    items = [{ id: 1, name: 'Banana' }, { id: 2, name: 'Maçã' }, { id: 3, nome: 'Abacaxi' }];
    
> tackById
    
Uma função opcionalmente passada para a NgForOfdiretiva para personalizar como NgForOfidentifica exclusivamente itens em um iterável

### ngSwitch

Troca condicionalmente o conteúdo do div selecionando por um dos modelos incorporados com base no valor atual da condição.

    // HTML
    <div [ngSwitch]="control">
      <ng-template [ngSwitchCase]="'A'">...</ng-template>
      <ng-template ngSwitchCase="B">...</ng-template>
      <ng-template ngSwitchDefault>...</ng-template>
    </div>
    
    // Classe
    control = 'A';

### ngPlural

Adiciona/remove subárvores DOM com base em um valor numérico. Sob medida para a pluralização.
    
    // HTML
    <div [ngPlural]="value">
      <ng-template ngPluralCase="0">ZERO</ng-template>
      <ng-template ngPluralCase="1">UM</ng-template>
    </div>
    
    // Classe
    value = 0;
    


> Diretivas Personalizadas

Criam interações diversas entre elementos HTML inseridos no DOM, manipulando e controlando comportamentos e dados dos componentes

### Diretiva

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
    
 ### Componente
 
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
    
