## Estilo 

Define a estilização dos elementos do Template, utilizando a linguagem de estilo SCSS. 

### Características

- Escrito na formatação padrão CSS, porém, com funcionalidades e recursos adicionais
- Possui a extensão **scss**, que quando compilado transforma as regras em css padrão

### Recursos SCSS

- Criação de regras aninhadas
- Abreviação de nomenclaturas ( & )
- Criação de variáveis ( $ )
- Herança
- Criação de mixins

> Aninhamento de regras

    .parent {
      ...
      
      .child {
        ....
      }
    }

> Abreviação

    .some-class.another-class { ... }           
    CSS
    
    .some-class {                               SCSS
      ...
      
      &.another-class { ... }
    }
    
O seletor pai também pode ser seguido por um sufixo, ou seja, ele pode ser apenas parte do nome do seletor.
    
    .big {                                      SCSS
      ...
    
      &-button {
        font-size: 1.5em;
      }
    }
     
    .big-button {                               CSS
      font-size: 1.5em; 
    }

> Variáveis

    $bg-color: blue;

    p {
      color: $bg-color;
    }
    
> Herança 

    .button {                                   SCSS
      color: white;
      border: 2px solid black;
    }

    .small-button {
      @extend .button;
      font-size: 0.7em;
      padding: 0.2em;
    }
    
    .button, .small-button {                    CSS
      color: white;
      border: 2px solid black; 
    }

    .small-button {
      font-size: 0.7em;
      padding: 0.2em; 
    }
    
> Mixins

    @mixin cabecalho {
      width: 100%;
      padding-left: 1em;
      padding-right: 1em;
      margin-bottom: 2em;
    }
    
    @mixin botao($font-color, $font-size: 1em) {
      font-size: $font-size;
      padding: $font-size/2;
      background-color: #426800;
      color: $font-color;
    }

    header {
      @include cabecalho;
    }

    .small-button {
      @include botao(#fff);
    }
    
    .medium-button {
      @include botao($font-color: #a930d8, $font-size: 1.2em);
    }

[Referência com mais funcionalidades SCSS](https://devschannel.com/sass/introducao-ao-sass)
