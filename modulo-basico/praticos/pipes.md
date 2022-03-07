## Pipes

Decorador que marca uma classe como sendo um canalizador de dados que fornece metadados de configuração. O Angular fornece pipes úteis por padrão para poderem serem uitlizados em qualquer aplicação.

### Metadados

- **name** - O nome do pipe a ser usado em associações de modelo. Normalmente usa lowerCamelCase porque o nome não pode conter hífens.
- **pure?** - Quando true, o pipe é puro, o que significa que o método **transform()** é invocado somente quando seus argumentos de entrada são alterados. Pipes são puros por padrão.

Se o pipe tiver um estado interno (ou seja, o resultado depende de outro estado além de seus argumentos), defina pure como `false`. Nesse caso, o pipe é invocado em cada ciclo de detecção de alterações, mesmo que os argumentos não tenham sido alterados.

### Pipes Padrão

-  [AsyncPipe](https://angular.io/api/common/AsyncPipe) 
-  [CurrencyPipe](https://angular.io/api/common/CurrencyPipe)
-  DatePipe 
-  DecimalPipe 
-  I18nPluralPipe 
-  I18nSelectPipe 
-  JsonPipe 
-  KeyValuePipe 
-  LowerCasePipe 
-  PercentPipe 
-  SlicePipe 
-  TitleCasePipe 
-  UpperCasePipe

> AsyncPipe

Desempacota um valor de um dado assíncrono (Observables/Promises) e retorna o valor mais recente emitido. Quando um novo valor é emitido, o asyncpipe marca o componente a ser verificado quanto a alterações. Quando o componente é destruído, o asyncpipe cancela a assinatura automaticamente para evitar possíveis vazamentos de memória. Quando a referência da expressão muda, o asyncpipe automaticamente cancela a assinatura do antigo Observable ou Promisee assina o novo.

    @Component({
      selector: 'async-observable-pipe',
      template: '<div><code>observable|async</code>: Time: {{ time | async }}</div>'
    })
    export class AsyncObservablePipeComponent {
      time = new Observable<string>((observer: Observer<string>) => {
        setInterval(() => observer.next(new Date().toString()), 1000);
      });
    }

> CurrencyPipe

Transforma um número em uma string de moeda, formatada de acordo com as regras de localidade que determinam o tamanho e o separador do grupo, caractere de ponto decimal e outras configurações específicas de localidade.

 #### - Parâmetros
 
| Parâmetro | Tipo | Descrição|
|- |- |- |
|currencyCode | string | como USD para o dólar americano e EUR para o euro. O código de moeda padrão pode ser configurado usando o token de injeção DEFAULT_CURRENCY_CODE |
|display | string /boolean| code: Mostra o código (como USD).
symbol(default): Mostra o símbolo (como $).
symbol-narrow: Use o símbolo estreito para localidades que tenham dois símbolos para sua moeda. Por exemplo, o dólar canadense CAD tem o símbolo CA$ e o símbolo estreito $. Se a localidade não tiver um símbolo restrito, usa o símbolo padrão para a localidade.
String: Use o valor de string fornecido em vez de um código ou símbolo. Por exemplo, uma string vazia suprimirá a moeda e o símbolo.
Boolean (marcado como obsoleto na v5): true para símbolo e false para código. |

    @Component({
      selector: 'currency-pipe',
      template: `<div>
        <p>A: {{ a | currency}}</p>    // '$0.26'

        <p>A: {{ a | currency:'CAD'}}</p>    // 'CA$0.26'

        <p>A: {{ a | currency:'CAD':'code'}}</p>    // 'CAD0.26'

        <p>B: {{ b | currency:'CAD':'symbol':'4.2-2'}}</p>    // 'CA$0,001.35'

        <p>B: {{ b | currency:'CAD':'symbol-narrow':'4.2-2'}}</p>    // '$0,001.35'

        <p>B: {{ b | currency:'CAD':'symbol':'4.2-2':'fr'}}</p>    // '0 001,35 CA$'
      </div>`
    })
    export class CurrencyPipeComponent {
      a: number = 0.259;
      b: number = 1.3495;
    }
