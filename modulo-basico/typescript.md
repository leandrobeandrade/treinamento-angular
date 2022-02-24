# Introdução e fundamentação TypesScript

- Tipagem estática
- Herança
- Polimorfismo (Sobrescrita)
- Classes Abstratas
- Interfaces
- Tipos Genéricos
- Tipos Utilitários
- Enums

## Exemplos

Exemplos de aplicação prática que podem ser testados online no próprio [site da linguagem](https://www.typescriptlang.org/play?#code/Q).

### Tipagem

Permite fornecer um tipo para o dado em runtime.

> Tipos de valores de variáveis

    let numer: number = 10;                                     // tipo NUMBER
    let boole: boolean = true;                                  // tipo BOOLEAN
    let nulos: null;                                            // tipo NULL
    let indef: undefined;                                       // tipo UNDEFINED
    let annys: any = 'tyufsd'+ 10;                              // tipo ANY - qualquer tipo
    let text1: string = 'Testando';                             // tipo STRING
    
    function func(): void {}                                    // tipo VOID

    let listas: [];                                             // tipo VETOR
    let lista1: number[] = [1, 2 ,3];                           // tipo VETOR
    let lista2: Array<number> = [1, 2, 3];

    let tupla: [string, number];                                // tipo TUPLA
    tupla = ['Qualquer texto!', 2018];
    console.log(tupla[0].length);
    console.log(tupla[1]);
    
> Tipos das variáveis
    
    const classe: Classe = new Classe()                         // tipo const - não será alterado o valor, guardar a referência
    
> Tipos especializados

    let result: number | string;                                // type union
    result = 10;
    result = 'olá'
    
    type things = string | number;                              // type alias
    let anywhere: type;
    anywhere = 10;
    anywhere = 'olá';
    
    interface Obj {
      name: string;
      age?: number;                                             // propriedade opcional
    }
    
    function test(str: string, num?: number) {}                 // parâmetro opcional
    
    let input = document.querySelector('input');                // casting de tipo
    let thing = input as HTMLInputElement;      
    
### Orientação a Objetos

> Herança

Amplamente utilizada no paradigma de orientação a objetos, onde uma classe filha `herda` propriedades (característica) e métodos (comportamento) da classe mãe utilizando 
a palavra reservada **`extends`**.

    class Pessoa {
      public nome: string;

      constructor(nome: string) {
        this.nome = nome;
      }

      falar(): void {
        console.log('A pessoa falou')
      }
    }

    class Funcionario extends Pessoa {

      constructor(nome: string) {
        super(nome);
      }
    }

    const func = new Funcionario('Fulano');
    console.log(func);
    func.falar();
    
> Polimorfismo

Permite que o comportamento de classes filhas tenham diferentes implementaçãoes através da **Sobrescrita (override)** de métodos.

    class Pessoa {
      public nome: string;

      constructor(nome:string) {
        this.nome = nome;
      }

      print(): void {
        console.log(`nome: ${this.nome}`);
      }
    }

    class Empregado  extends Pessoa {
      private salario: number;

      constructor(nome:string, salario:number){
        super(nome);
        this.salario = salario;
      }

      print(): void {
        super.print();
        console.log(`O nome é ${this.nome} e o salario: ${this.salario}`);
      }
    }

    let pess: Pessoa = new Pessoa('Fulano');
    pess.print();
    console.log(pess);

    let func: Pessoa = new Empregado('Ciclano', 3500);		// polimorfismo
    func.print();
    console.log(func);
    
> Classe Abstrata

Classes, métodos e propriedades no TypeScript podem ser abstratos. Um método ou propriedade abstrato ou campo abstrato é aquele que não teve uma implementação fornecida. 
Esses membros devem existir dentro de uma classe abstrata, que não pode ser instanciada diretamente. O papel das classes abstratas é servir como uma classe base para 
subclasses que implementam todos os membros abstratos.

    abstract class Base {
      abstract getName(): string;
 
      printName() {
        console.log(`Olá ${this.getName()}`);
      }
    }
 
    const b = new Base();     // ERRO
    
    class Derived extends Base {
      getName() {
        return 'mundo';
      }
    }
 
    const d = new Derived();
    d.printName();
    
> Interfaces

Assim como classes abstratas, interfaces servem como um modelo que deve ser implementado no momento que for declarado seu tipo, a nível de classes para
a implementação de uma interface utiliza-se a plavara reservada **`implements`**.

    interface IEmployee {
      empCode: number;
      empName: string;
      getSalary: (num: number) => number;
      getManagerName(str: string): void; 
      isMan(bool: boolean): boolean;
    }

    const itf: IEmployee = {
      empCode: 10,
      empName: 'Test',
      getSalary: (val) => val + 10,
      getManagerName: (val) => console.log(`Olá ${val}`),
      isMan(val) {
        return val;
      }
    }

    console.log(itf.empCode);
    console.log(itf.empName);
    console.log(itf.getSalary(4990));
    itf.getManagerName('fulano');
    console.log(itf.isMan(true));
 
### Tipos Especializados 
 
> Generics Types

Os tipos genéricos em TypeScript permitem que se escreva o código de uma forma reutilizável e generalizada em funções, classes e interfaces.
   
    // Generic Function
    function aleatorios<T>(items: T[]): T {
      let aleatIndex = Math.floor(Math.random() * items.length);
      return items[aleatIndex];
    }

    let numbers = [1, 5, 7, 4, 2, 9];
    let aleatorioNum = aleatorios<number>(numbers); 
    console.log(aleatorioNum);

    let strings = ['1', '5', '7', '4', '2', '9'];
    let aleatorioString = aleatorios<string>(strings); 
    console.log(aleatorioString);
    
    // Generic Class
    class Gaveta<TipoDeRoupa> {
      conteudo: TipoDeRoupa[] = [];

      adicionar(objeto: TipoDeRoupa) {
        this.conteudo.push(objeto);
      }

      remover() {
        return this.conteudo.pop();
      }
    }

    interface Meia {
      cor: string;
    }

    interface Camiseta {
      tamanho: 'p' | 'm' | 'g';
    }

    const gavetaDeMeias = new Gaveta<Meia>();
    gavetaDeMeias.adicionar({ cor: 'branco' });
    console.log(gavetaDeMeias.conteudo);

    const gavetaDeCamisetas = new Gaveta<Camiseta>();
    gavetaDeCamisetas.adicionar({ tamanho: 'm' });
    console.log(gavetaDeCamisetas.conteudo);

    // Union Types 
    const gavetaMista = new Gaveta<Meia | Camiseta>();
    gavetaMista.adicionar({ cor: 'verde'});
    gavetaMista.adicionar({ tamanho: 'g'});
    console.log(gavetaMista.conteudo);
  
> Utility Types
  
Tipos utilitários para facilitar transformações de tipo comum. Esses utilitários estão disponíveis globalmente.

#### Partial< Type >
Constrói um tipo com todas as propriedades de Type definidas como opcionais. Esse utilitário irá retornar um tipo que representa todos os subconjuntos de um determinado tipo.
    
    interface Todo {
      title: string;
      description: string;
    }

    function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
      return { ...todo, ...fieldsToUpdate };
    }

    const todo1: Todo = {
      title: 'Organizar armário',
      description: 'limpar, arrumar',
    };
    console.log(updateTodo(todo1, todo1));

    const todo2: Todo = updateTodo(todo1, {
      description: 'Sujeira jogada no lixo',
    });
    console.log(todo2);

    const todo3: Todo = updateTodo(todo2, { title: 'Armário arrumado!' })
    console.log(todo3);
    
#### Required< Type >
Constrói um tipo que consiste em todas as propriedades de Type definidas como obrigatórias. O oposto de `Partial`.

    interface Props {
      a?: number;
      b?: string;
    }

    const obj1: Props = { a: 5 };
    const obj2: Required<Props> = { a: 5 };   // ERRO
    
#### Readonly< Type >
Constrói um tipo com todas as propriedades de Type definidas como leitura, o que significa que as propriedades do tipo construído não podem ser reatribuídas.

    interface Todo {
      title: string;
    }

    const todo: Readonly<Todo> = {
      title: 'Delete inactive users',
    };

    todo.title = 'Hello';   // ERRO
    
#### Record< Keys, Type >
Constrói um tipo de objeto cujas chaves de propriedade são Keys e cujos valores de propriedade são Type. Este utilitário pode ser usado para mapear as propriedades de um tipo para outro tipo.

    interface Pessoa {
      nome: string;
      idade: number;
    }

    type Departamento = 'TI' | 'RH' | 'Operacional';

    const pessoa: Record<Departamento, Pessoa> = {
      TI: { idade: 30, nome: 'Fulano' },
      RH: { idade: 26, nome: 'Beltrano' },
      Operacional: { idade: 21, nome: 'Ciclano' },
    };
 
    console.log(pessoa);
    
#### Pick< Type, Keys >
Constrói um tipo escolhendo o conjunto de propriedades Keys (literal de string ou união de literais de string) de Type.

    interface Todo {
      title: string;
      description: string;
      completed: boolean;
    }

    type TodoPreview = Pick<Todo, 'title' | 'completed'>;

    const todo: TodoPreview = {
      title: 'Limpar Quarto',
      completed: false,
      description: ''    // ERRO
    };

    console.log(todo);
    
#### Omit< Type, Keys >
Constrói um tipo selecionando todas as propriedades de Type e removendo Keys (literal de string ou união de literais de string).

    interface Todo {
      title: string;
      description: string;
      completed: boolean;
      createdAt: number;
    }

    type TodoPreview = Omit<Todo, 'description'>;

    const todo1: TodoPreview = {
      title: 'Limpar quarto',
      completed: false,
      createdAt: 1615544252770,
    };

    console.log(todo1);
    
[link para ver todos os Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
  
> Enums

Enums permitem que um desenvolvedor defina um conjunto de `constantes nomeadas`. O TypeScript fornece enumerações **numéricas** e baseadas em **string**.
    
    enum Direction {                                            // tipo ENUM - Nummber
      Up,
      Down,
      Left,
      Right,
    }
    
    console.log(Direction.Up)
    
    enum Direction {                                            // tipo ENUM - String
      Up = 'Up',
      Down = 'Down',
      Left = 'Left',
      Right = 'Right',
    }
    
    console.log(Direction.Up)

    enum Dia { SEGUNDA, TERCA, QUARTA=5, QUINTA, SEXTA };
    let dia1: Dia = Dia.SEGUNDA;
    let dia2 = Dia[1];

    console.log(dia1);                                  
    console.log(dia2);
    console.log(`${Dia.QUARTA} | ${Dia.QUINTA}`);
