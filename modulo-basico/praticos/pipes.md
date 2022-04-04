## Pipes

Decorador que marca uma classe como sendo um canalizador de dados que fornece metadados de configuração. O Angular fornece pipes úteis por padrão para poderem serem uitlizados em qualquer aplicação.

### Metadados

- **name** - O nome do pipe a ser usado em associações de modelo. Normalmente usa lowerCamelCase porque o nome não pode conter hífens.
- **pure?** - Quando true, o pipe é puro, o que significa que o método **`transform()`** é invocado somente quando seus argumentos de entrada são alterados. Pipes são puros por padrão.

Se o pipe tiver um estado interno (ou seja, o resultado depende de outro estado além de seus argumentos), defina pure como `false`. Nesse caso, o pipe é invocado em cada ciclo de detecção de alterações, mesmo que os argumentos não tenham sido alterados.

### Pipes Padrão

-  [AsyncPipe](https://angular.io/api/common/AsyncPipe) - `{ obj_expression | async }}`
-  [CurrencyPipe](https://angular.io/api/common/CurrencyPipe) - `{{ value_expression | currency [ : currencyCode [ : display [ : digitsInfo [ : locale ] ] ] ] }}`
-  [DatePipe](https://angular.io/api/common/DatePipe) - `{{ value_expression | date [ : format [ : timezone [ : locale ] ] ] }}`
-  [DecimalPipe](https://angular.io/api/common/DecimalPipe) - `{{ value_expression | number [ : digitsInfo [ : locale ] ] }}` 
-  [I18nPluralPipe](https://angular.io/api/common/I18nPluralPipe) - `{{ value_expression | i18nPlural : pluralMap [ : locale ] }}` 
-  [I18nSelectPipe](https://angular.io/api/common/I18nSelectPipe) - `{{ value_expression | i18nSelect : mapping }}`
-  [JsonPipe](https://angular.io/api/common/JsonPipe) - `{{ value_expression | json }}` 
-  [KeyValuePipe](https://angular.io/api/common/KeyValuePipe) - `{{ input_expression | keyvalue [ : compareFn ] }}` 
-  [LowerCasePipe](https://angular.io/api/common/LowerCasePipe) - `{{ value_expression | lowercase }}`
-  [UpperCasePipe](https://angular.io/api/common/UpperCasePipe) - `{{ value_expression | uppercase }}`
-  [TitleCasePipe](https://angular.io/api/common/TitleCasePipe) - `{{ value_expression | titlecase }}`
-  [SlicePipe](https://angular.io/api/common/SlicePipe) - `{{ value_expression | slice : start [ : end ] }}`
-  [PercentPipe](https://angular.io/api/common/PercentPipe) - `{{ value_expression | percent [ : digitsInfo [ : locale ] ] }}`

> AsyncPipe

Desempacota um valor de um dado assíncrono *`(Observables/Promises)`* e retorna o valor mais recente emitido. Quando um novo valor é emitido, o asyncpipe marca o componente a ser verificado quanto a alterações. Quando o componente é destruído, o asyncpipe cancela a assinatura automaticamente para evitar possíveis vazamentos de memória. Quando a referência da expressão muda, o asyncpipe automaticamente cancela a assinatura do antigo Observable ou Promise assina o novo

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

Transforma um número em uma string de moeda, formatada de acordo com as regras de localidade que determinam o tamanho e o separador do grupo, caractere de ponto decimal e outras configurações específicas de localidade

 #### Parâmetros

- `currencyCode (string)` - O código de moeda ISO 4217, como USD para o dólar americano e EUR para o euro. O código de moeda padrão pode ser configurado usando o token de injeção DEFAULT_CURRENCY_CODE. O padrão é this._defaultCurrencyCode.

- `display (string | boolean)` - O formato do indicador de moedacom várias formatações, com o padrão sendo symbol.

    - **code:** Mostra o código (como USD).
    - **symbol(default):** Mostra o símbolo (como $).
    - **symbol-narrow:** Usa o símbolo para localidades que tenham dois símbolos para sua moeda. Por exemplo, o dólar canadense `CAD` tem o símbolo CA$ e o símbolo estrito `$`. Se a localidade não tiver um símbolo restrito, o símbolo padrão será usado para a localidade.
    - **String:** Usa o valor de string fornecido em vez de um código ou símbolo. Por exemplo, uma string vazia suprimirá a moeda e o símbolo.
    - **Boolean (obsoleto na v5):** true para símbolo e false para código.

- `digitsInfo (string)` - Opções de representação decimal, especificadas por uma string no seguinte formato: 
    
      {minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}.

    - **minIntegerDigits:** O número mínimo de dígitos inteiros antes do ponto decimal. O padrão é 1.
    - **minFractionDigits:** O número mínimo de dígitos após o ponto decimal. O padrão é 2.
    - **maxFractionDigits:** O número máximo de dígitos após o ponto decimal. O padrão é 2. Se não for fornecido, o número será formatado com a quantidade adequada de dígitos, dependendo do que a *ISO 4217* especifica. Por exemplo, o dólar canadense tem 2 dígitos, enquanto o peso chileno não tem nenhum.
   
- `locale (string)` - Um código de localidade para as regras de formato de localidade a serem usadas. Quando não fornecido, usa o valor de `LOCALE_ID`, que é en-US por padrão.

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
      
> DatePipe

Formata um valor de data de acordo com as regras de localidade. DatePipe é executado apenas quando detecta uma mudança pura no valor de entrada. Uma alteração pura é uma alteração em um valor de entrada primitivo (como String, Number, Boolean ou Symbol) ou uma referência de objeto alterada (como Date, Array, Function ou Object).
Observe que a mutação de um objeto Date não faz com que o pipe seja renderizado novamente. Para garantir que o pipe seja executado, você deve criar um novo objeto Date

- Somente os dados de localidade en-US vêm com o Angular. Para localizar datas em outro idioma, você deve importar os dados de localidade correspondentes.
- O fuso horário do valor formatado pode ser especificado passando-o como o segundo parâmetro do pipe ou definindo o padrão por meio do token de injeção DATE_PIPE_DEFAULT_TIMEZONE. O valor que é passado como segundo parâmetro tem precedência sobre aquele definido usando o token de injeção.

#### Opções pré-definidas

|Opção      |Equivalente                        | Exemplos utilizando a localidade en-US            |
|-          |-                                  |-                                                  |
|'short'    |'M/d/yy, h:mm a'                   |	6/15/15, 9:03 AM                                |
|'medium'   |'MMM d, y, h:mm:ss a'              |	Jun 15, 2015, 9:03:01 AM                        |
|'long'     |'MMMM d, y, h:mm:ss a z'           |	June 15, 2015 at 9:03:01 AM GMT+1               |
|'full'     |'EEEE, MMMM d, y, h:mm:ss a zzzz'  |	Monday, June 15, 2015 at 9:03:01 AM GMT+01:00   |

Todas as opções podem ser vistas [aqui](https://angular.io/api/common/DatePipe#pre-defined-format-options).

Estes exemplos transformam uma data em vários formatos, supondo que **dateObj** seja um objeto Date JavaScript para `ano: 2015, mês: 6, dia: 15, hora: 21, minuto: 43, segundo: 11`, fornecido na hora local para o **`en-US`** dos EUA.

    {{ dateObj | date }}               // 'Jun 15, 2015'
    {{ dateObj | date:'medium' }}      // 'Jun 15, 2015, 9:43:11 PM'
    {{ dateObj | date:'shortTime' }}   // '9:43 PM'
    {{ dateObj | date:'mm:ss' }}       // '43:11'

Exemplo de uso

    @Component({
     selector: 'date-pipe',
     template: `<div>
       <p>Hoje é {{ hoje | date }}</p>
       <p>Ou se preferir, {{ today | date:'fullDate' }}</p>
       <p>O tempo é {{ hoje | date:'h:mm a z' }}</p>
     </div>`
    })
    export class DatePipeComponent {
      hoje: number = Date.now();
    }
    
> DecimalPipe

Formata um valor de acordo com as opções de dígitos e as regras de localidade. A localidade determina o tamanho e o separador do grupo, o caractere de ponto decimal e outras configurações específicas da localidade

#### Parâmetros

-  `digitsInfo (string)` - Define a representação de dígitos e decimais com o seguinte formato

        {minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}
      
    - **minIntegerDigits:** O número mínimo de dígitos inteiros antes do ponto decimal. O padrão é 1.
    - **minFractionDigits:** O número mínimo de dígitos após o ponto decimal. O padrão é 0.
    - **maxFractionDigits:** O número máximo de dígitos após o ponto decimal. O padrão é 3.
    
- `locale (string)` - Especifica quais regras de formato de localidade usar.

> I18nPluralPipe

Mapeia um valor para uma string que pluraliza o valor de acordo com as regras de localidade

#### Parâmetros

- `pluralMap (object)` - Objeto que imita o formato ICU
- `locale (string)`- Uma string que define a localidade a ser usada (usa o LOCALE_ID atual por padrão).

> I18nSelectPipe

Seletor genérico que exibe a string que corresponde ao valor atual

#### Parâmetros

- `mapping	(object)` - Objeto que indica o texto que deve ser exibido para valores diferentes do valor fornecido.

Exemplo de uso

    @Component({
      selector: 'i18n-select-pipe', 
      template: `<div>{{ genere | i18nSelect: convite }} </div>`
    })
    export class I18nSelectPipeComponent {
      genero = 'masculino';
      convite = {'masculino': 'Convide ele.', 'feminino': 'Convide ela.', 'outros': 'Convite eles.'};
    }

> JsonPipe

Converte um valor em sua representação no formato JSON. Útil para depuração

Exemplo de uso

    @Component({
      selector: 'json-pipe',
      template: `<div>
        <p>Without JSON pipe:</p>
        <pre>{{object}}</pre>
        <p>With JSON pipe:</p>
        <pre>{{object | json}}</pre>
      </div>`
    })
    export class JsonPipeComponent {
      object: Object = {foo: 'bar', baz: 'qux', nested: {xyz: 3, numbers: [1, 2, 3, 4, 5]}};
    }
    
> KeyValuePipe

Transforma **Objeto** ou **Mapa** em uma matriz de pares de valores-chave. A matriz de saída será ordenada por chaves. Por padrão, o comparador será pelo valor do ponto Unicode. Você pode opcionalmente passar um **`compareFn`** se suas chaves forem tipos complexos

#### Parâmetros

- `compareFn (função)` - Função de transformação. Opcional

Exmplo de uso

    @Component({
      selector: 'keyvalue-pipe',
      template: `<span>
        <p>Object</p>
        <div *ngFor="let item of object | keyvalue">
          {{item.key}}:{{item.value}}
        </div>
        <p>Map</p>
        <div *ngFor="let item of map | keyvalue">
          {{item.key}}:{{item.value}}
        </div>
      </span>`
    })
    export class KeyValuePipeComponent {
      object: {[key: number]: string} = {2: 'foo', 1: 'bar'};
      map = new Map([[2, 'foo'], [1, 'bar']]);
    }
    
> LowerCasePipe

Transforma o texto em todas as letras minúsculas

> UpperCasePipe

Transforma o texto em letras maiúsculas

> TitleCasePipe

Transforma o texto em maiúsculas e minúsculas. Coloca em maiúscula a primeira letra de cada palavra e transforma o resto da palavra em minúscula. As palavras são delimitadas por qualquer caractere de espaço em branco, como um caractere de espaço

> SlicePipe

Cria um novo Array ou String contendo um subconjunto (fatia) dos elementos. Todo o comportamento é baseado no comportamento JavaScript `Array.prototype.slice()` e `String.prototype.slice()`

 - Ao operar em um Array, o Array retornado é sempre uma cópia, mesmo quando todos os elementos estão sendo retornados.
 - Ao operar em um valor em branco, o pipe retorna o valor em branco.

#### Parâmetros

- `start (number)` - Define um número como índice para o início 	
- `end (number)` - Define um número com índice para o fim	

Exemplo de uso - Array

    @Component({
      selector: 'slice-list-pipe',
      template: `<ul>
        <li *ngFor="let i of collection | slice:1:3">{{ i }}</li>   // <li>b</li> <li>c</li>
      </ul>`
    })
    export class SlicePipeListComponent {
      collection: string[] = ['a', 'b', 'c', 'd'];
    }

Exemplo de uso - String

    @Component({
      selector: 'slice-string-pipe',
      template: `<div>
        <p>{{ str }}[0:4]: '{{ str | slice:0:4 }}'</p>      //  'abcd'
        <p>{{ str }}[4:0]: '{{ str | slice:4:0 }}'</p>      // ''
        <p>{ {str }}[-4]: '{{ str | slice:-4 }}'</p>        // 'ghij'
        <p>{{ str }}[-4:-2]: '{{ str | slice:-4:-2 }}'</p>  // 'gh'
        <p>{{ str }}[-100]: '{{ str | slice:-100 }}'</p>    // 'abcdefghij'
        <p>{{ str }}[100]: '{{ str | slice:100 }}'</p>      // ''
      </div>`
    })
    export class SlicePipeStringComponent {
      str: string = 'abcdefghij';
    }

> PercentPipe

Transforma um número em uma string de porcentagem, formatada de acordo com as regras de localidade que determinam o tamanho e o separador do grupo, caractere de ponto decimal e outras configurações específicas de localidade

#### Parâmetros

- `digitsInfo (string)` - Opções de representação decimal, especificadas por uma string no seguinte formato:

      {minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}.

    - **minIntegerDigits:** O número mínimo de dígitos inteiros antes do ponto decimal. O padrão é 1.
    - **minFractionDigits:** O número mínimo de dígitos após o ponto decimal. O padrão é 0.
    - **maxFractionDigits:** O número máximo de dígitos após o ponto decimal. O padrão é 0.

- `locale (string)`- Um código de localidade para as regras de formato de localidade a serem usadas. Quando não fornecido, usa o valor de LOCALE_ID, que é **en-US** por padrão.      
## Pipes Personalizados

Além dos pipes fornececidos pelo próprio Angular, também podemos criar nossos próprios pipes personalizados com funcionalidades diversas.

### Pipe

    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
      name: 'letras'
    })
    export class LetrasPipe implements PipeTransform {

      transform(value: any, args?: any): any {
        if (args != null) {
          if (args === 'ingles')
            switch (value) {
              case 1: return 'one';
              case 2: return 'two';                  
            }
          if (args === 'portugues')
            switch (value) {
              case 1: return 'um';
              case 2: return 'dois';                  
            }        
          }
          switch (value) {
            case 1: return 'uno';
            case 2: return 'dos';
          }  
        return null;
      }
    }
    
### Classe 

    // HTML
    <h1>Números em Espanhol</h1>
    <ul>
      <li *ngFor="let valor of dados">
        {{ valor | letras }}
      </li>
    </ul>
    <h1>Números em inglês</h1>
    <ul>
      <li *ngFor="let valor of dados">
        {{valor | letras:'ingles'}}
      </li>
    </ul>
    <h1>Números en português</h1>
    <ul>
      <li *ngFor="let valor of dados">
        {{valor | letras:'portugues'}}
      </li>
    </ul>
    
    // Classe
    dados = [1, 2];
    
|Referências|
|-|

- [pipes](https://angular.io/api/common/CommonModule#pipes)
