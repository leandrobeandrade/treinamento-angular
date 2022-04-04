## Manipulação de Estilos

### ngClass

Vincula dinâmicamente a presença de classes `CSS` no elemento à veracidade dos valores de mapa associados.

> Inserção de classe por condição

    <div [ngClass]="{'my_class1': control === true }"> ... </div>
    
    <div [ngClass]="{'my_class1': control === true, 'my_class2': control === false }"> ... </div>
    
    <div [ngClass]="{'1': 'my_class1', '2': 'my_class2'}[stepper]"> ... </div>
    
> Inserção de classe por condição ternária

    <div [ngClass]="control === true ? 'my_class1' : 'my_class2'"> ... </div>
    
> Inserção de classe por função

    <div [ngClass]="{'my_class1': func() }"> ... </div>

    <div [ngClass]="func() ? 'my_class1' : 'my_class2'"> ... </div>
    
    <div [ngClass]="{'val1': 'my_class1', 'val2': 'my_class2'}[func()]"> ... </div>

### Class Attribute

Vincula dinâmicamente e diretamente classes `CSS` no elemento conforme condição aplicada.

    <div [class.my_class1]="control === true"> ... </div>
    
    <div [class.my_class2]="control === false"> ... </div>
    
    <div [class.my_class2]="func()"> ... </div>

### ngStyle

Permite atribuir estilos a um elemento HTML utilizando CSS diretamente.

> Inserção de estilo por condição
    
    <button [ngStyle]="{'backgroundColor': (control ? 'green' : 'red'), 'fontSize': size + 'px'}">Clicar</button>
    
> Inserção de estilo por condição ternária

    <button [ngStyle]="{opacity: control ? '0.5' : '1'}">Clicar</button>

    <button [ngStyle]="control ? {'background-color': 'green'} : {'background-color': 'red'}">Clicar</button>
    
    
### Style Attribute    
    
Vincula dinâmicamente e diretamente estilos CSS no elemento conforme condição aplicada.
    
    <button [style.backgroundColor]="control ? 'blue' : 'red'"  [style.fontSize]="size + 'px'">Clicar</button>
    
    <button [style.background-color]="control ? 'green' : right ? 'red' : null">Clicar</button>
    
|Referências|
|-|

- [ngClass](https://angular.io/api/common/NgClass)
- [ngStyle](https://angular.io/api/common/NgStyle)
- [attributos](https://angular.io/guide/attribute-binding)
