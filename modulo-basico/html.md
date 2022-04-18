## Template HTML Angular

Define o modelo HTML associado ao Component equivalente.

### Características

- É criado um virtual DOM para que a engine do Angular possa controlar praticamente tudo dentro do template
- Recebe notação Html com tags HTML5 reconhecidas e suporta tags personalizadas Angular
- Linkada ao arquivo de estilo equivalente para estilização, podendo receber estilos de outro arquivo de escopo mais externo por "herança"
- Linkada ao arquivo da lógica TypeScript para receber e enviar dados através da aplicação
- Suporte contra erros de HTML como não fechamento de tags explícitos pelo navegador

### Exemplos

    <h1>Meu primeiro APP</h1>                                     HTML válido
    
    <h1>{{ prop }}</h1>                                           HTML válido
    
    <input type="text" [value]="prop" />                          HTML válido
    
    <h1 *ngIf="prop === true">{{ prop }}</h1>                     HTML válido
    
    <div *ngFor="let prop of props; let i = index">               HTML válido
      <p>{{ i + 1 }} - {{ prop }}</p>                
    </div>
    
    <p ngStyle="{'backgroundColor: blue'}">{{ prop }}</p>         HTML válido
    
> Disclaimer

Se o Template possuir menos de 20 linhas de conteúdo pode-se optar pela não utilização de um arquivo HTML, mas sim, escrever o HTML dentro da própria classe substituindo 
a propriedade **`templateUrl`** por **`template`** e escrever o código HTML dentro de `template strings` tecla **crase** **( ` )**

## Sintaxe para Template

    // Valor que será computado
    
    greeting = 'World!';

#### Vincula o conteúdo de texto a uma string interpolada. Notaçaõ `INTERPOLATION`
    
    <p> Hello {{ greeting }} </p>         // <p>Hello World!</p>

#### Vincula uma propriedade a uma string interpolada. Equivalente a: <div [title]="'Hello' + greeting">
    
    <div title="Hello {{ greeting }}">

#### Vincula `(bind)` o valor de uma propriedade ao resultado da expressão greeting. Notaçaõ `PROPERTY BIND`
    
    <input [value]="greeting">
    
#### Os vínculos (binds) atribuem `aria-label` ao resultado da expressão actionName.

    <button type="button" [attr.aria-label]="actionName">{{ actionName }} with Aria</button>

#### Vincula a presença da classe CSS `isDanger` no elemento à veracidade da expressão isError.

    <div [class.isDanger]="isError">

#### Vincula a largura da propriedade de estilo ao resultado da expressão mySize em pixels. As unidades são opcionais.

    <div [style.width.px]="mySize">

#### Chama o método cancelAction quando um evento de clique é disparado neste elemento de botão (ou seus filhos) e passa no objeto de evento. Notação `EVENT BIND`

    <button (click)="cancelAction($event)">
    
#### Configura a vinculação de dados bidirecional. Notação `TWO-WAY DATA BINDING`

    <input [value]="name" (blur)="name = $event.target.value">

#### Configura a vinculação de dados bidirecional de forma simplificada. Equivalente a: <my-cmp [title]="name" (titleChange)="name = $event">
    
    <meu-cmp [(title)]="name">

#### Cria uma variável local `movieplayer` que fornece acesso à instância do elemento de vídeo em expressões de vinculação de dados e de evento no modelo atual.

    <video #movieplayer>
        <button (click)="movieplayer.play()">
    </video>

#### O símbolo `*` transforma o elemento atual em um modelo embutido. Equivalente a: <ng-template [myDirective]="myExpression"> ... </ng-template>

    <p *myDirective="myExpression"> ... </p>

#### Transforma o valor atual da expressão cardNumber por meio do pipe denominado myCardNumberFormatter.

    <p>Card No.: {{ cardNumber | myCardNumberFormatter }}</p>

#### O operador de navegação segura `?` significa que o campo do empregador é opcional e, se *`indefinido`*, o restante da expressão deve ser ignorado.

    <p>Employer: {{ employer?.companyName }}</p>

#### Um modelo de snippet SVG precisa de um prefixo svg: em seu elemento raiz para remover a ambigüidade do elemento SVG de um componente HTML.

    <svg: rect x = "0" y = "0" width = "100" height = "100" />

#### Um elemento raiz `<svg>` é detectado como um elemento SVG automaticamente, sem o prefixo.
    
    <svg>
        <rect x = "0" y = "0" largura = "100" altura = "100" />
    </svg>
