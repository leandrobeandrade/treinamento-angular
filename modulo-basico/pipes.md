## Pipe Angular

Arquivo responsável por criar métodos que agem no DOM virtual Angular criando transformações sobre determinados dados em comum, por exemplo um pipe para dados de data.


### Descrição @Pipe

> Decorador que marca uma classe como canal(pipe) e fornece metadados de configuração.

- `name:` Uma string com o nome do pipe a ser usado em ligações de modelo. Normalmente usa **lowerCamelCase** porque o nome não pode conter hifens.
- `pure?:` Booleano que quando verdadeiro, o pipe é puro, o que significa que o método **transform()** é invocado apenas quando seus argumentos de entrada mudam. Pipes são puros por padrão.

  > Se o pipe conter um estado interno (ou seja, o resultado depende do estado diferente de seus argumentos), definir pure como falso. Nesse caso, o pipe é chamado em cada ciclo de detecção de mudança, mesmo que os argumentos não tenham mudado.
  
      import { Pipe, PipeTransform } from '@angular/core';

      @Pipe({
        name: 'filterArray'
      })
      export class FilterArrayPipe implements PipeTransform {

        transform(val: any, args?: any): any {

          if(val.length === 0 || args === undefined) {
            return val;
          }

          let filter = args.toLocaleLowerCase();
          
          return val.filter(v => v.toLocaleLowerCase().indexOf(filter) != -1);    
        }

      }
      
      
      {{ exp | filterArray }}
      
     > O resultado da expressão é passado para o método **transform()** do pipe.
    Um pipe deve pertencer a um **NgModule** para estar disponível para um modelo. Para torná-lo membro de um NgModule, liste-o no campo de declarações dos metadados NgModule.
    
> LINK DE REFERÊNCIA: https://angular.io/api/core/Pipe
