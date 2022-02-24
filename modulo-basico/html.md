## Template HTML Angular

Define o modelo HTML associado ao Component equivalente.

### Características

- É criado um virtual DOM para que a engine do Angular possa controlar praticamente tudo dentro do template
- Recebe notação Html com tags HTML5 reconhecidas e suporta tags personalizadas Angular
- Linkada ao arquivo de estilo equivalente para estilização, podendo receber estilos de outro arquivo de escopo mais externo por "herança"
- Linkada ao arquivo da lógica TypeScript para receber e enviar dados através da aplicação
- Suporte contra erros de HTML como não fechamento de tags explícitos pelo navegador

### Prático

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
