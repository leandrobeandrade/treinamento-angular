## Operadores Gerais Comuns

Existem mais de 100 operadores disponibilizados pela bilbioteca RXJS, sendo estes dividos em 10 categorias sendo `Operadores de Criação`, **Operadores de Criação e Junção**, **Operadores de Transformação**, **Operadores de Filtragem**, **Operadores de Junção**, **Operadores Multicasting**, **Operadores de Tratamento de Erros**, **Operadores de Utilidades**, **Operadores Condicionais e Booleanos**, **Operadores Matemáticos e de Agregação**.

### Operadores de Criação

> **`of`**

Converte os argumentos para uma sequência observável. Ao contrário do `from`, não faz nenhum achatamento e emite cada argumento em conjunto como uma próxima notificação separada

    import { of } from 'rxjs';

    of([1, 2, 3])
    .subscribe(
      next => console.log('next: ', next),
      err => console.log('error:', err),
      () => console.log('fim'),
    );

    // Logs:
    // next [1, 2, 3]
    // fim
    
> **`from`**

Cria um Observável a partir de um Array, um objeto semelhante a um Array, uma Promessa, um objeto iterável, ou um objeto semelhante a um Observável. Uma String, neste contexto, é tratada como um array de caracteres

    import { from } from 'rxjs';

    const array = [10, 20, 30];
    const result$ = from(array);

    result$.subscribe(x => console.log(x));

    // Logs:
    // 10
    // 20
    // 30
    
> **`interval`**

Cria um Observável que emite números seqüenciais a cada intervalo de tempo especificado. Retorna um Observável que emite uma sequência infinita de inteiros ascendentes, com um intervalo constante de tempo à sua escolha entre essas emissões. A primeira emissão não é enviada imediatamente, mas apenas após o primeiro período ter passado

    import { interval } from 'rxjs';
    import { take } from 'rxjs/operators';

    const numbers$ = interval(1000);

    const takeFourNumbers$ = numbers$.pipe(take(4));    // Operador take executa a instrução até o valor especificado

    takeFourNumbers$.subscribe(x => console.log('Next: ', x));

    // Logs:
    // Next: 0
    // Next: 1
    // Next: 2
    // Next: 3
    
> **`timer`**

Utilizado para emitir uma notificação após um atraso. Este observável é útil para criar atrasos em código, ou correr contra outros valores para timeouts. O delay é especificado por padrão em milissegundos. Uma vez que o intervalo espera pelo atraso passado antes de começar, às vezes isso não é o ideal, necessitando iniciar um intervalo imediatamente. o `timer` funciona bem para isso

    import { timer, interval } from 'rxjs';

    timer(0, 1000).subscribe(n => console.log('timer', n));       // disparado imediatamente
    interval(1000).subscribe(n => console.log('interval', n));    // disparado após o valor especificado (1 segundo)
    
> **`iif`**

Verifica um booleano no momento da assinatura, e escolhe entre uma de duas fontes observáveis


    import { iif, of } from 'rxjs';

    let subscribeToFirst;
    
    const firstOrSecond$ = iif(
      () => subscribeToFirst,
      of('Entrou pq foi verdadeiro'),
      of('Entrou pq foi falso'),
    );

    subscribeToFirst = true;
    firstOrSecond$.subscribe(value => console.log(value));

    // Logs:
    // "Entrou pq foi verdadeiro"

    subscribeToFirst = false;
    firstOrSecond$.subscribe(value => console.log(value));

    // Logs:
    // "Entrou pq foi falso"
    
### Operadores de Criação e Junção
  
> **`concat`**

Cria um output Observable que emite sequencialmente todos os valores do primeiro Observable dado e depois avança para o próximo. A qualquer momento, apenas um Observable passado para o operador emite valores. Une vários Observables, inscrevendo-os um de cada vez e mesclando seus resultados na saída Observable. Pode passar um array de Observables ou colocá-los diretamente como argumentos. Passar um array vazio resultará em Observable que será concluído imediatamente. Se precisar emitir valores de Observables passados simultaneamente, considerar utilizar o `merge`

    import { concat, interval } from 'rxjs';
    import { take } from 'rxjs/operators';

    const timer1$ = interval(1000).pipe(take(10));
    const timer2$ = interval(2000).pipe(take(6));
    const timer3$ = interval(500).pipe(take(10));

    const result$ = concat(timer1$, timer2$, timer3$);
    result$.subscribe(x => console.log(x));

    // Logs:
    // 0  1 ... 9     <= vai até 9 a cada 1 segundo 
    // 0  1 ... 5     <= vai até 5 a cada 2 segundos
    // 0  1 ... 9     <= vai até 9 a cada meio segundo
    
> **`forkJoin`**

forkJoin é um operador que recebe qualquer número de observáveis de entrada que podem ser passados como uma matriz ou um dicionário de observáveis de entrada. Se nenhum observável de entrada for fornecido (por exemplo, uma matriz vazia for passada), o fluxo resultante será concluído imediatamente. Passar um array de n observáveis para o operador, então o array resultante terá n valores, onde o primeiro valor é o último emitido pelo primeiro observável, o segundo valor é o último emitido pelo segundo observável e assim por diante

    import { forkJoin, from, of, timer } from 'rxjs';

    const observable$ = forkJoin({
      foo: of(1, 2, 3),
      bar: Promise.resolve(4),
      baz: timer(4000),
    });
    observable$.subscribe({
     next: value => console.log(value),
     complete: () => console.log('Finaliza!'),
    });

    // Logs:
    // { foo: 3, bar: 4, baz: 0 } Só loga depois de 4 segundos
    // "Finaliza!"
    
> **`merge`**

Cria um output Observable que simultaneamente emite todos os valores de cada input Observable. Assina cada entrada Observable (como argumentos) e simplesmente encaminha (sem fazer nenhuma transformação) todos os valores de todos os Observables de entrada para a saída Observable. A saída Observável só é concluída quando todos os Observáveis de entrada são concluídos. Qualquer erro entregue por uma entrada Observável será imediatamente emitido na saída Observável

    import { merge, take } from 'rxjs/operators';
    import { interval } from 'rxjs';

    const first$ = interval(2500);
    const second$ = interval(1000);
    
    const example$ = first.pipe(merge(second), take(10));
    const subscribe = example.subscribe(val => console.log(val));
    
    // Logs:
    // 0¹ 1¹ 0² 2¹ 3¹ 1² 4¹ 5¹ 6 2²     <= ¹ resultado do interval de 1 segundo, ² resultado do interval de 2 segundos e meio

### Operadores de Transformação    

> **`concatMap`**

Projeta cada valor de origem para um Observable que é mesclado no Observable de saída, de forma serializada, esperando que cada um seja concluído na ordem que foram declarados antes de mesclar o próximo. Retorna um Observable que emite itens com base na aplicação de uma função fornecida a cada item emitido pelo Observable de origem, em que essa função retorna um Observable (chamado "interno"). Cada novo Observável interno é concatenado com o Observável interno anterior

    import { fromEvent, interval } from 'rxjs';
    import { concatMap, take } from 'rxjs/operators';

    const clicks$ = fromEvent(document, 'click');
    const result$ = clicks.pipe(
      concatMap(ev => interval(1000).pipe(take(5)))
    )
    .subscribe(x => console.log('Evento disparado', x + 1));

    // Após evento de clique é disparado a execução dentro do concatMap()
    // Cada execução do concatMap() é executado após o encerramento do primeiro concatMap()
    // Mesmo que disparado 2 eventos sequencialmente a segunda emissão de valores só será executada após o fim da primeira emissão

> **`map`** 

Aplica uma determinada função que mapeia cada valor emitido pelo Observable de origem e emite os valores resultantes como um Observable, semelhamte ao método map do JavaScript

    import { from } from 'rxjs';
    import { map } from 'rxjs/operators';

    const source$ = from([
      { name: 'Fulano', age: 30 },
      { name: 'Beltrano', age: 20 },
      { name: 'Ciclano', age: 50 }
    ]);

    const example$ = source$.pipe(map(({ name }) => name));
    const subscribe = example$.subscribe(val => console.log(val));
    
    // Logs: 
    // "Fulano" "Beltrano" "Ciclano"
    
> **`mergeMap`**

Projeta cada valor de origem para um Observable que é mesclado na saída Observable e em seguida, nivela todos esses Observables internos usando mergeAll implícito

    import { of } from 'rxjs';
    import { concatMap, delay, mergeMap } from 'rxjs/operators';

    const source$ = of(2000, 1000);

    source$
      .pipe(concatMap((val) => of(`Atraso de: ${val}ms`).pipe(delay(val))))
      .subscribe((val) => console.log(`Usando concatMap: ${val}`));
      
    // Logs:
    // Executa a primeira execução de 2 segundos e ao término a segunda de 1 segundo 

    // Diferença entre concatMap e mergeMap
    source$
      .pipe(
        mergeMap((val) => of(`Atraso de by: ${val}ms`).pipe(delay(val))),
        delay(5000)    // delay de 500 apenas para os dois exemplos não executarem junto   
      )
      .subscribe((val) => console.log(`Usando mergeMap: ${val}`));

    // Logs:
    // Executa a primeira execução de 1 segundo e ao término a segunda de 2 segundos 

> switchMap

Projeta cada valor de origem para um Observable que é mesclado no Observable de saída, emitindo valores apenas do Observable projetado mais recentemente. Retorna um Observable que emite itens com base na aplicação de uma função fornecida a cada item emitido pelo Observable de origem, em que essa função retorna um Observable (chamado "interno"). Cada vez que observa um desses Observables internos, o Observable de saída começa a emitir os itens emitidos por esse Observable interno. Quando um novo Observable interno é emitido, switchMap para de emitir itens do Observable interno emitido anteriormente e começa a emitir itens do novo. Ele continua a se comportar assim para Observáveis internos subsequentes.

    import { fromEvent, interval } from 'rxjs';
    import { switchMap, take } from 'rxjs/operators';

    const clicks$ = fromEvent(document, 'click');
    const result$ = clicks.pipe(switchMap((ev) => interval(1000)));
    result.subscribe(x => console.log(x));
    
    // Logs:
    // A cada novo evento de clique disparado os valores são inicializados
    
### Operadores de Filtragem

> debounce

Emite uma notificação da fonte Observável somente após um determinado intervalo de tempo determinado por outro Observável ter passado sem emissão de outra fonte. Atrasa as notificações emitidas pela fonte Observável, mas descarta as emissões atrasadas pendentes anteriores se uma nova notificação chegar à fonte Observável

    import { interval, timer } from 'rxjs';
    import { debounce } from 'rxjs/operators';

    const interval$ = interval(1000);

    interval$
      .pipe(debounce((val) => {
        console.log('=> ', val)
        return timer(val * 200)      // aumenta o tempo de debounce em 200ms a cada segundo
      }))
      .subscribe((val) => console.log(`Example Two: ${val}`));

    // Após 5 segundos, o tempo de debounce será maior que o tempo de intervalo,
    // todos os valores futuros serão descartados

> debounceTime

Emite uma notificação da fonte Observável somente após um determinado intervalo de tempo ter passado sem outra emissão de fonte. Atrasa as notificações emitidas pela fonte Observável, mas descarta as emissões atrasadas pendentes anteriores se uma nova notificação chegar na fonte Observável. Este operador acompanha a notificação mais recente da fonte Observável e a emite somente quando o dueTime tiver passado sem que nenhuma outra notificação apareça na fonte Observável.

    import { fromEvent } from 'rxjs';
    import { debounceTime, map } from 'rxjs/operators';

    const searchBox = document.getElementById('search');
    const keyup$ = fromEvent(searchBox, 'keyup')

    keyup$.pipe(
      map((i: any) => i.currentTarget.value),
      debounceTime(500)
    )
    .subscribe(console.log);
    
    // Logs:
    // Valores que foram digitados no input no intervalo de meio segundo, muito utilizado para fazer filtros com dados de um API

> distinct

Retorna um Observable que emite todos os itens emitidos pelo Observable de origem que são distintos por comparação dos itens anteriores

    import { of } from 'rxjs';
    import { distinct } from 'rxjs/operators';

    of(1, 1, 2, 2, 2, 1, 2, 3, 4, 3, 2, 1)
      .pipe(distinct())
      .subscribe(x => console.log(x));

    // Logs:
    // 1 2 3 4

> filter

Filtra os itens emitidos pela fonte Observável emitindo apenas aqueles que atendem a um predicado especificado

    import { from } from 'rxjs';
    import { filter } from 'rxjs/operators';

    const source$ = from([1, 2, 3, 4, 5]);
    const example$ = source$.pipe(filter(num => num % 2 === 0))

    const subscribe = example$.subscribe(val => console.log(`Even number: ${val}`));

    // Logs:
    // 2 4 
    
> first

Emite apenas o primeiro valor (ou o primeiro valor que atende a alguma condição) emitido pela fonte Observável
 
    import { from } from 'rxjs';
    import { first } from 'rxjs/operators';

    const source$ = from([1, 2, 3, 4, 5]);
    const example$ = source$.pipe(first());
    
    const subscribe = example$.subscribe(val => console.log(`valor: ${val}`));
    
    // Logs:
    // "valor: 1"
    
> last

Retorna um Observable que emite apenas o último item emitido pelo Observable de origem

    import { from } from 'rxjs';
    import { last } from 'rxjs/operators';

    const source$ = from([1, 2, 3, 4, 5]);
    const example$ = source$.pipe(last());
    
    const subscribe = example$.subscribe(val => console.log(`Last value: ${val}`));
    
    // Logs:
    // "valor: 5"
 
> take

Emite apenas os primeiros valores de contagem emitidos pela fonte Observável

    import { interval } from 'rxjs';
    import { take } from 'rxjs/operators';

    const interval$ = interval(1000);
    const example$ = interval$.pipe(take(5));
    
    const subscribe = example$.subscribe(val => console.log(val));
    
    // Logs:
    // 0, 1, 2, 3, 4
    
> takeLast

Aguarda a conclusão da origem e emite os últimos N valores da origem, conforme especificado pelo argumento de contagem

    import { of } from 'rxjs';
    import { takeLast } from 'rxjs/operators';

    const source$ = of('Ignore', 'Ignore', 'Hello', 'World!');
    
    const example = source$.pipe(takeLast(2)).subscribe(val => console.log(val));
    
    // Logs;
    // Hello, World!

> takeUntil

Emite os valores emitidos pela fonte Observable até que um notificador Observable emita um valor

    import { timer } from 'rxjs';
    import { takeUntil } from 'rxjs/operators';

    const source$ = timer(0, 1000);
    const timer$ = timer(5000);

    source$.pipe(takeUntil(timer$)).subscribe(val => console.log(val));
    
    // Logs:
    // 0, 1, 2, 3

> takeWhile

Emite valores emitidos pela fonte Observable desde que cada valor satisfaça o predicado fornecido e, em seguida, conclui assim que esse predicado não é satisfeito

    import { of } from 'rxjs';
    import { takeWhile } from 'rxjs/operators';

    const source$ = of(1, 2, 3, 4, 5);
    const example$ = source.pipe(takeWhile(val => val <= 4));
    
    const subscribe = example.subscribe(val => console.log(val));

    // Logs:
    // 1 2 3 4
 
 ### Operadores de Junção
 
 > mergeAll

Converte um Observável de ordem superior em um Observável de primeira ordem que simultaneamente entrega todos os valores que são emitidos nos Observáveis internos

    import { fromEvent, interval } from 'rxjs';
    import { take, map, mergeAll } from 'rxjs/operators';

    const clicks$ = fromEvent(document, 'click');
    const higherOrder$ = clicks.pipe(
      map((ev) => interval(1000).pipe(take(10))),
    );
    const firstOrder$ = higherOrder.pipe(mergeAll(2));
    firstOrder$.subscribe(x => console.log(x));
    
    // Logs:
    // Executa os valores a cada 1 segundo após o evento, 
    // se na execução deste evento for disparado outro evento os dois eventos executam em simultâneo

### Operadores Multicasting

> share

Retorna um novo Observable que faz multicast (compartilha) o Observable original. Enquanto houver pelo menos um Assinante, este Observável estará inscrito e emitindo dados. Quando todos os assinantes cancelarem a assinatura, ele cancelará a assinatura da fonte Observável
 
    import { interval } from 'rxjs';
    import { share, map, take } from 'rxjs/operators';

    const source$ = interval(1000).pipe(
      map((x: number) => {
        console.log('Processing: ', x);
        return x * x;
      }),
      share(),
      take(5)
    );

    source$.subscribe((x) => console.log('subscription 1: ', x));
    source$.subscribe((x) => console.log('subscription 1: ', x));
 
### Operadores de tratamento de erros

> catchError

Captura erros no observável a ser tratado retornando um novo observável ou lançando um erro

    import { throwError, of } from 'rxjs';
    import { catchError } from 'rxjs/operators';
    
    const source = throwError('Algum problema!');   // Lança um erro
   
    const example$ = source.pipe(catchError(val => of(`ERRO: ${val}`)));
    
    const subscribe = example$.subscribe(val => console.log(val));
    
    // Logs:
    // "ERRO: Algum problema|"
    
### Operadores de Utilidades

> tap

Usado para executar efeitos colaterais para notificações da fonte observável. O uso mais comum de tap é na verdade para depuração. Você pode colocar um tap(console.log) em qualquer lugar do seu pipe observável, desconectar as notificações conforme elas são emitidas pela fonte retornada pela operação anterior

    import { of } from 'rxjs';
    import { tap, map } from 'rxjs/operators';

    const source = of(1, 2, 3, 4, 5);

    const example = source.pipe(
      tap(val => console.log(`ANTES MAP: ${val}`)),
      map(val => val + 10),
      tap(val => console.log(`APÓS MAP: ${val}`))
    );

    const subscribe = example.subscribe(val => console.log('FINAL => ', val));
    
    // Logs:
    // ANTES MAP: 1 APÓS MAP: 11 FINAL => 11
    // ANTES MAP: 2 APÓS MAP: 12 FINAL => 12
    // ANTES MAP: 3 APÓS MAP: 13 FINAL => 13

> delay

Atrasa a emissão de itens da fonte Observável por um determinado tempo limite ou até uma determinada Data

    import { fromEvent } from 'rxjs';
    import { delay } from 'rxjs/operators';

    const clicks = fromEvent(document, 'click');
    const delayedClicks = clicks.pipe(delay(1000)); // valores serão emitidos após 1 segundo depois do clique
    delayedClicks.subscribe(x => console.log(x));
  
### Operadores Condicionais e Booleanos

> every

Retorna um Observable que emite se cada item da fonte satisfaz ou não a condição especificada

    import { of } from 'rxjs';
    import { every } from 'rxjs/operators';

     of(1, 2, 3, 4, 5, 6).pipe(
        every(x => x < 6),
    )
    .subscribe(x => console.log(x)); 
    
    // Logs:
    // false

### Operadores Matemáticos e de Agregação

> reduce

...

    import { fromEvent, interval } from 'rxjs';
    import { reduce, takeUntil, mapTo } from 'rxjs/operators';

    const clicksInFiveSeconds = fromEvent(document, 'click').pipe(
      takeUntil(interval(5000)),
    );
    const ones = clicksInFiveSeconds.pipe(mapTo(1));
    const seed = 0;
    const count = ones.pipe(reduce((acc, one) => acc + one, seed));
    count.subscribe(x => console.log(x));
    
|Referências|
|-|

- [operadores](https://rxjs.dev/guide/operators)
- [operadores](https://www.learnrxjs.io/learn-rxjs/operators)
    
