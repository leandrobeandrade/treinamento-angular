## Breve Introdução

> TypeScript é uma linguagem de programação fortemente tipada que se baseia em JavaScript, oferecendo melhores ferramentas em qualquer escala.

### Algumas Características

- Utilizado apenas em ambiente de desenvolvimento, depois sendo "compilado" para JavaScript
- Fortemente tipado, tanto por descrição como por inferência
- Programação Orientada a Objetos clássica
- Fornece classes, interfaces e tipagem estática opcional
- Possibilita a detecção de erros em tempo de execução pelas IDE's de desenvolvimento

### Outras características específicas

- Tipagem estática
- Herança
- Polimorfismo (Sobrescrita)
- Classes Abstratas
- Interfaces
- Tipos Genéricos
- Tipos Utilitários
- Enums

### Breve Exemplos

> Tipagem

Permite fornecer um tipo para o dado em runtime.

#### Tipos de valores de variáveis

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
    
#### Tipos das variáveis
    
    const classe: Classe = new Classe()                         // tipo const - não será alterado o valor, guardar referência

    interface Rx {
      readonly x: number;                                       // tipo READONLY
    }
    
    let rx: Rx = { x: 10 };
    console.log(rx)
    rx.x = 1; // error
    
#### Tipos especializados

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

    // Sobrescrita
    
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
        return "world";
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
    
> Generics Types

   Os tipos genéricos em TypeScript permitem que se escreva o código de uma forma reutilizável e generalizada em funções, classes e interfaces.
   
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
  
> Generics Types
  
  ...
  
> Enums

Enums permitem que um desenvolvedor defina um conjunto de constantes nomeadas. O TypeScript fornece enumerações **numéricas** e baseadas em **string**.
    
    enum Direction {                                            // tipo ENUM - Nummber
      Up,
      Down,
      Left,
      Right,
    }
    
    console.log(Direction.Up)
    
    enum Direction {                                            // tipo ENUM - String
      Up = "Up",
      Down = "Down",
      Left = "Left",
      Right = "Right",
    }
    
    console.log(Direction.Up)

    enum Dia { SEGUNDA, TERCA, QUARTA=5, QUINTA, SEXTA };
    let dia1: Dia = Dia.SEGUNDA;
    let dia2 = Dia[1];

    console.log(dia1);                                  
    console.log(dia2);
    console.log(`${Dia.QUARTA} | ${Dia.QUINTA}`);
   
