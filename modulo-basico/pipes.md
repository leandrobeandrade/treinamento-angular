## Pipe Angular

Arquivo responsável por criar métodos que agem no DOM virtual Angular criando transformações sobre determinados dados em comum, por exemplo um pipe para dados de data.


### Descrição @Pipe

> Decorador que marca uma classe como canal(pipe) e fornece metadados de configuração.

- `name:` Uma string com o nome do pipe a ser usado em ligações de modelo. Normalmente usa **lowerCamelCase** porque o nome não pode conter hifens.
- `pure?:` Booleano que quando verdadeiro, o pipe é puro, o que significa que o método **transform()** é invocado apenas quando seus argumentos de entrada mudam. Pipes são puros por padrão.

  > Se o pipe conter um estado interno (ou seja, o resultado depende do estado diferente de seus argumentos), definir pure como falso. Nesse caso, o pipe é chamado em cada ciclo de detecção de mudança, mesmo que os argumentos não tenham mudado.
  
#### Arquivo Pipe      
      
    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
      name: 'sayHello'
    })
    export class GreetingsPipe implements PipeTransform {

    public transform(name: string): string {
      return 'Hello, ' + name;
    }
      
#### Componente que usa o Pipe
    
    this.user = {
      firstName: 'Fulano',
      ...
    };
    
    <div>
      <p>You are now logged in our application!</p>
      <p>{{ user.firstName | sayHello }}</p>
    </div>
      
    <!-- output -->
    <p>Hello, Fulano</p>
      
> O resultado da expressão é passado para o método **transform()** do pipe.
    
> O pipe deve pertencer a um **NgModule** para estar disponível para um modelo. Para torná-lo membro de um NgModule, liste-o no campo de declarações dos metadados NgModule.

Angular por padrão fornece pipes integrados para transformações de dados típicos, incluindo transformações para internacionalização (i18n), que usam informações de localidade para formatar dados. A seguir, são pipes internos comumente usados para formatação de dados:

- **DatePipe:** Formata um valor de data de acordo com as regras de localidade.
- **UpperCasePipe:** Transforma o texto em letras maiúsculas.
- **LowerCasePipe:** Transforma o texto em todas as letras minúsculas.
- **CurrencyPipe:** Transforma um número em uma string de moeda, formatada de acordo com as regras de localidade.
- **DecimalPipe:** Transforma um número em uma string com um ponto decimal, formatado de acordo com as regras de localidade.
- **PercentPipe:** Transforma um número em uma string de porcentagem, formatada de acordo com as regras de localidade.
    
|Referências|
|-|

- [pipes](https://angular.io/api/core/Pipe)
- [exemplos](https://angular.io/guide/pipes)
