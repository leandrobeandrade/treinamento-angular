## Formulários Reativos - Configurações

Em formulários do tipo `Orientado a Dados` o form é criado programaticamente e gerenciado dentro da classe do componente assim como validações e controle dos dados. Esse gerenciamento é realizado a partir de uma instância de métodos e sincronizado através de proprieades com o Template. Para fazer o uso de formulários do tipo Reativo é necessário que no módulo seja declarado o módulo de formulário **`ReactiveFormsModule`**.

Os formulários reativos diferem dos formulários orientados por template de forma distinta. Os formulários reativos fornecem acesso síncrono ao modelo de dados, imutabilidade com operadores observáveis e rastreamento de mudanças através de fluxos observáveis.

Os formulários dos tipo reativo fornece três blocos de construção fundamentais, *`FormControl`*, *`FormGroup`* e *`FormArray`*. Estas classes extendem a classe [**AbstractControl**](https://github.com/leandrobeandrade/treinamento-angular/blob/main/modulo-intermediario/forms/data-driven/abstract-control.md) que implementa a maior parte das funcionalidades básicas para acessar o valor, status de validação, interações do usuário e eventos.

### FormControl

Rastreia o valor e o status de validação de um controle de formulário individual.

> **Explícito**

    name = new FormControl();
    
> **Implícito**

    user = new FormGroup({
      name: [],
    })
    
#### Formas de declarar FormControl()

- `name: new FormControl()` - instância com valor null
- `name: new FormControl(null)` - instância com valor null
- `name: new Formcontrol('')` - instância string vazia
- `name: []` - valor null
- `name: [null]` - valor null
- `name: ['']` - valor string vazia
- `name: ''` - valor string vazia

#### Exemplo de uso

    <form>
      <label>Name: </label>
      <input type="text" [formControl]="name">
    </form>

### FormGroup

Rastreia o valor e o estado de validade de um grupo de instâncias do FormControl. Agrega os valores de cada filho FormControl em um objeto, com cada nome de controle sendo uma chave.

    user: FormGroup;

    this.user = new FormGroup({
      name: new FormControl(),
    })
    
#### Exemplo de uso

    <form [formGroup]="user">
      <label>Name: </label>
      <input type="text" formControlName="name">
    </form>
    
### FormArray

Rastreia o valor e o estado de validade de um array de instâncias FormControl, FormGroup ou FormArray. Agrega os valores de cada FormControl filho em uma matriz.
    
    user: FormGroup;
    
    this.user = new FormGroup({
      name: [],
      courses = new FormArray([]);
    })
    
    // Cria um acesso a propriedade formArray do formulário
    get courses() {
      return this.user.get('courses') as FormArray;
    }
    
#### Exemplo de uso
    
    <form [formGroup]="user">
      <label>Name: </label>
      <input type="text" formControlName="name">
      
      <div formArrayName="courses" *ngFor="let course of courses.controls; index as i">
        <label>Curso:</label>
        <input type="text" [formControlName]="i">
      </div>
    </form>

### FormBuilder

O Angular também fornece um método construtor de formulários que além de ajudar na leitura do código facilita na hora de escrever o mesmo. Utilizando a classe de serviço **[FormBuilder](https://angular.io/api/forms/FormBuilder)** que após ser injetada na classe, fornece três métodos para a criação de um formulário.

    constructor(private fb: FormBuilder) {}

> control()

Constrói um novo FormControl gerenciando estado, validadores e opções do contole

    user: formControl;
    
    this.user = this.fb.control({name: [], disabled: true});
    this.user = this.fb.control({age: null});

> group()

Constrói uma nova instância do FormGroup
    
    user: FormGroup;
    
    this.user = this.fb.group({ 
      name: [], disabled: true,
      age: null
    })

> array()

Constrói um novo FormArray a partir de um determinado conjunto de configurações, validadores e opções

    user: FormArray;
    
    this.user = this.fb.array([])


#### Exemplo completo de implementação de um formArray

    user: formGroup;
    
    constructor(private fb: formBuilder) {}

    this.user = this.fb.group({
      name: ['', Validators.required],
      age: [null],
      address: new FormGroup({
        street: [],
        city: [null],
        state: '',
        zip: ['']
      }),
      courses: this.fb.array([
        this.fb.control('')
      ])
    });
    
*O controle de `cursos` na instância do grupo de formulários agora é preenchido com um único controle até que mais controles sejam adicionados dinamicamente. O uso de  **getter** cria uma propriedade de classe de cursos para recuperar o controle do array do formulário pai.*

    get courses() {
      return this.user.get('courses') as FormArray;
    }
    
*Define um método para inserir dinamicamente um controle de curso no array de formulário dos cursos. O método **FormArray.push()** insere o controle como um novo item no array.*

    addCourses() {
      this.courses.push(this.fb.control(''));
    }
    
*Assim também como um método para deletar em específico um curso no array de cursos. O método **removeAt()** deleta o campo ou grupo de campos recebendo o index do campo ou grupo a ser deletado.*

    removeCourse(index: number) {
      this.courses.removeAt(index);
    }
    
*Como os elementos do array de formulário não têm nome, foi atribuido o índice à variável **`i`** o que é passado para cada controle para vinculá-lo à entrada  `formControlName`.*

    <div formArrayName="courses">
      <h2>Cursos</h2>
      <button type="button" (click)="addCourses()">Adicionar cursos</button>

      <div *ngFor="let course of courses.controls; let i = index">
        <label>Curso:</label>
        <input type="text" [formControlName]="i">
        
        <button (click)="removeCourse(i)">Remover curso</button>
      </div>
    </div>

|Referências|
|-

- [Reactive Forms](https://angular.io/guide/reactive-forms)
- [FormControl](https://angular.io/api/forms/FormControl)
- [FormGroup](https://angular.io/api/forms/FormGroup)
- [FormArray](https://angular.io/api/forms/FormArray)
