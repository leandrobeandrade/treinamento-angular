## Operadores Gerais Comuns

Existem mais de 100 operadores disponibilizados pela bilbioteca RXJS, sendo estes dividos em 10 categorias:

- Operadores de Criação
- Operadores de Criação e Junção
- Operadores de Transformação
- Operadores de Filtragem
- Operadores de Junção
- Operadores Multicasting
- Operadores de Tratamento de Erros
- Operadores de Utilidades
- Operadores Condicionais e Booleanos
- Operadores Matemáticos e de Agregação

### Operadores de Criação

> of

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
    
> from

Cria um Observável a partir de um Array, um objeto semelhante a um Array, uma Promessa, um objeto iterável, ou um objeto semelhante a um Observável. Uma String, neste contexto, é tratada como um array de caracteres

    import { from } from 'rxjs';

    const array = [10, 20, 30];
    const result = from(array);

    result.subscribe(x => console.log(x));

    // Logs:
    // 10
    // 20
    // 30
    
> interval

Cria um Observável que emite números seqüenciais a cada intervalo de tempo especificado. Retorna um Observável que emite uma sequência infinita de inteiros ascendentes, com um intervalo constante de tempo à sua escolha entre essas emissões. A primeira emissão não é enviada imediatamente, mas apenas após o primeiro período ter passado

    import { interval } from 'rxjs';
    import { take } from 'rxjs/operators';

    const numbers = interval(1000);

    const takeFourNumbers = numbers.pipe(take(4));    // Operador take executa a instrução até o valor especificado

    takeFourNumbers.subscribe(x => console.log('Next: ', x));

    // Logs:
    // Next: 0
    // Next: 1
    // Next: 2
    // Next: 3
    
> timer

Utilizado para emitir uma notificação após um atraso. Este observável é útil para criar atrasos em código, ou correr contra outros valores para timeouts. O delay é especificado por padrão em milissegundos. Uma vez que o intervalo espera pelo atraso passado antes de começar, às vezes isso não é o ideal, necessitando iniciar um intervalo imediatamente. o `timer` funciona bem para isso

    import { timer, interval } from 'rxjs';

    timer(0, 1000).subscribe(n => console.log('timer', n));       // disparado imediatamente
    interval(1000).subscribe(n => console.log('interval', n));    // disparado após o valor especificado (1 segundo)
    
> iif

Verifica um booleano no momento da assinatura, e escolhe entre uma de duas fontes observáveis


    import { iif, of } from 'rxjs';

    let subscribeToFirst;
    
    const firstOrSecond = iif(
      () => subscribeToFirst,
      of('Entrou pq foi verdadeiro'),
      of('Entrou pq foi falso'),
    );

    subscribeToFirst = true;
    firstOrSecond.subscribe(value => console.log(value));

    // Logs:
    // "Entrou pq foi verdadeiro"

    subscribeToFirst = false;
    firstOrSecond.subscribe(value => console.log(value));

    // Logs:
    // "Entrou pq foi falso"
    
### Operadores de Criação e Junção
  
> concat

Cria um output Observable que emite sequencialmente todos os valores do primeiro Observable dado e depois avança para o próximo. A qualquer momento, apenas um Observable passado para o operador emite valores. Une vários Observables, inscrevendo-os um de cada vez e mesclando seus resultados na saída Observable. Pode passar um array de Observables ou colocá-los diretamente como argumentos. Passar um array vazio resultará em Observable que será concluído imediatamente. Se precisar emitir valores de Observables passados simultaneamente, considerar utilizar o `merge`

    import { concat, interval } from 'rxjs';
    import { take } from 'rxjs/operators';

    const timer1 = interval(1000).pipe(take(10));
    const timer2 = interval(2000).pipe(take(6));
    const timer3 = interval(500).pipe(take(10));

    const result = concat(timer1, timer2, timer3);
    result.subscribe(x => console.log(x));

    //Logs:
    // 0  1 ... 9     <= vai até 9 a cada 1 segundo 
    // 0  1 ... 5     <= vai até 5 a cada 2 segundos
    // 0  1 ... 9     <= vai até 9 a cada meio segundo
    
> forkJoin

forkJoin é um operador que recebe qualquer número de observáveis de entrada que podem ser passados como uma matriz ou um dicionário de observáveis de entrada. Se nenhum observável de entrada for fornecido (por exemplo, uma matriz vazia for passada), o fluxo resultante será concluído imediatamente. Passar um array de n observáveis para o operador, então o array resultante terá n valores, onde o primeiro valor é o último emitido pelo primeiro observável, o segundo valor é o último emitido pelo segundo observável e assim por diante

    import { forkJoin, from, of, timer } from 'rxjs';

    const observable = forkJoin({
      foo: of(1, 2, 3),
      bar: Promise.resolve(4),
      baz: timer(4000),
    });
    observable.subscribe({
     next: value => console.log(value),
     complete: () => console.log('Finaliza!'),
    });

    // Logs:
    // { foo: 3, bar: 4, baz: 0 } Só loga depois de 4 segundos
    // "Finaliza!"
    
> merge

Cria um output Observable que simultaneamente emite todos os valores de cada input Observable. Assina cada entrada Observable (como argumentos) e simplesmente encaminha (sem fazer nenhuma transformação) todos os valores de todos os Observables de entrada para a saída Observable. A saída Observável só é concluída quando todos os Observáveis de entrada são concluídos. Qualquer erro entregue por uma entrada Observável será imediatamente emitido na saída Observável

    import { merge, take } from 'rxjs/operators';
    import { interval } from 'rxjs';

    const first = interval(2500);
    const second = interval(1000);
    
    const example = first.pipe(merge(second), take(10));
    const subscribe = example.subscribe(val => console.log(val));
    
    // Logs:
    // 0¹ 1¹ 0² 2¹ 3¹ 1² 4¹ 5¹ 6 2²     <= ¹ resultado do interval de 1 segundo, ² resultado do interval de 2 segundos e meio

### Operadores de Transformação    

...

