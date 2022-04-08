## RXJS - Descrição

Reactive Extensions for JavaScript, ou RxJS, é uma biblioteca JavaScript que usa observáveis para programação reativa. Ele pode ser utilizado com outras bibliotecas e estruturas JavaScript além de se integrar muito bem ao Angular. O paradigma reativo pode ser usado em muitas linguagens diferentes através do uso de bibliotecas reativas. Essas bibliotecas são APIs baixadas que fornecem funcionalidades para ferramentas reativas como observadores e operadores. 

RxJS fornece programação reativa para lidar com implementação assíncrona, retornos de chamada e programas baseados em eventos. Pode ser usado em um navegador ou com Node.js. O RxJS possui alguns recursos principais que ajudam a lidar com a implementação assíncrona:

- Observável **`(Observable)`**
- Observador **`(Observer)`**
- Inscrição **`(Subscription)`**
- Operador **`(Operator)`**
- Sujeito **`(Subject)`**
- Agendador **`(Scheduler)`**

### `Observable`

Os observadores RxJS permitem a implementação e captação de eventos, podendo realizar a modelagem do fluxo deste evento. Observables têm dois métodos: *`subscription`* e *`unsubscription`*, sendo o observable executado quando ocorre a inscrição do mesmo.

    import { Observable } from 'rxjs';

    const observable = new Observable(subscriber => {
      subscriber.next(1);
      subscriber.next(2);
      subscriber.next(3);
      subscriber.complete();
    });

    observable.subscribe(resp => console.log(resp));
    
O código dentro de **`new Observable(subscriber => {...})`** representa uma "Execução Observable", uma computação lenta (lazy) que só acontece para cada Observer que se inscreve. A execução produz vários valores ao longo do tempo, de forma síncrona ou assíncrona.

Existem três tipos de valores que uma execução observável pode fornecer:

- `next`: propaga um valor como um número, uma string, um objeto, etc
- `error`: propaga um erro ou exceção de JavaScript
- `complete`: encerra o fluxo e não propaga valor

As notificações `next` são o tipo mais importante e comum representando dados reais que estão sendo entregues a um assinante. As notificações de `error` e `complete` podem ocorrer apenas uma vez durante a Execução Observável, e só pode haver uma delas.

### `Observer`

Um observador é um objeto com os métodos *`next()`*, *`error()`* e *`complete()`* que é chamado quando há uma interação com o observável, são os objetos que assinam observáveis, é um consumidor de valores entregues por um Observable. Os observadores são simplesmente um conjunto de retornos de chamada **(callbacks)**, um para cada tipo de notificação entregue pelo Observable: next, error e complete.

### `Subscription`

É a assinatura do observável que acionará a execução do observável. É um objeto que representa um recurso descartável, geralmente a execução de um Observável. Uma Subscription tem um método importante, o **`unsubscribe`**, que não aceita argumentos e apenas descarta o recurso retido pela assinatura.

    import { Observable } from 'rxjs';

    const observable = new Observable(subscriber => {
      subscriber.next(1);
      subscriber.next(2);
      subscriber.next(3);
      subscriber.complete();
    });

    const obs = observable.subscribe(resp => console.log(resp));
    
    obs.unsubscribe();
    
### `Operator`

Um operador é uma função que permite realizar certas ações em eventos executados por observáveis. Os operadores são as peças essenciais que permitem que código assíncrono complexo seja facilmente composto de maneira declarativa. Existem dois tipos de operadores:

- Canalizáveis **(Pipeable)**

  > Operadores Pipeable são do tipo que podem ser canalizados para Observables usando a sintaxe observableExemplo.pipe(operator()). Estes incluem, filter(...), e mergeMap(...), etc... Quando chamados, eles não alteram a instância Observable existente. Em vez disso, eles retornam um novo Observable, cuja lógica de assinatura é baseada no primeiro Observable.
- Criação **(Creation)**

  > Os operadores de criação são o outro tipo de operador, que podem ser chamados como funções independentes para criar um novo observável. Por exemplo: of(1, 2, 3) cria um observável que emitirá 1, 2 e 3, um após o outro. Outros exemplos de operadores de criação são from, empty, interval, timer, etc...

### `Subject`

Um Sujeito RxJS é um tipo especial de Observable que permite que valores sejam multicast para muitos Observadores. Enquanto os Observables simples são unicast (cada Observer inscrito possui uma execução independente do Observable), os Subjects são multicast. Os Subjects são como EventEmitters: eles mantêm um registro de muitos ouvintes.

> **Todo Subject é um Observable**. Dado um Subject, este poderá ser assinado, disponibilizando um Observer, que passará a receber valores normalmente. Da perspectiva do Observer, ele não pode dizer se a execução do Observable vem de um Unicast simples de um Observable ou de um Multicast de um Subject.

Internamente ao Subject, subscribe não invoca uma nova execução que entrega valores. Ele simplesmente registra o Observador fornecido em uma lista de Observadores, de forma semelhante a como o addListener geralmente funciona em outras bibliotecas e linguagens.

**Todo Subject é um Observer**. É um objeto com os métodos *`next()`*, *`error()`* e *`complete()`*. Para alimentar um novo valor para o Subject, basta chamar next(valor), e ele será multicast para os Observables cadastrados para escutar o Subject.

No exemplo abaixo, temos dois Observers anexados a um Subject e alimentamos alguns valores para o Subject:

    import { Subject } from 'rxjs';

    const subject = new Subject<number>();

    subject.subscribe({
      next: (v) => console.log(`observerA: ${v}`)
    });
    
    subject.subscribe({
      next: (v) => console.log(`observerB: ${v}`)
    });

    subject.next(1);
    subject.next(2);

    // Logs:
    // observerA: 1
    // observerB: 1
    // observerA: 2
    // observerB: 2

### `Scheduler`

Um Scheduler controla quando uma assinatura é iniciada e quando as notificações são entregues. É composto por três componentes.

- Um Scheduler é uma estrutura de dados. Ele sabe como armazenar e enfileirar tarefas com base na prioridade ou em outros critérios.
- Um Scheduler é um contexto de execução. Denota onde e quando a tarefa é executada (por exemplo, imediatamente, ou em outro mecanismo de retorno de chamada, como setTimeout ou process.nextTick, ou o quadro de animação).
- Um Scheduler tem um relógio (virtual). Ele fornece uma noção de "tempo" por um método getter now() no agendador. As tarefas que estão sendo agendadas em um agendador específico seguirão apenas a hora indicada por esse relógio.

Um Agendador permite definir em qual contexto de execução um Observable entregará notificações ao seu Observador.

|Referências|
|-|

- [Observable](https://rxjs.dev/guide/observable)
- [Observer](https://rxjs.dev/guide/observer)
- [Subscription](https://rxjs.dev/guide/subscription)
- [Operator](https://rxjs.dev/guide/operators)
- [Subject](https://rxjs.dev/guide/subject)
- [Scheduler](https://rxjs.dev/guide/scheduler)
