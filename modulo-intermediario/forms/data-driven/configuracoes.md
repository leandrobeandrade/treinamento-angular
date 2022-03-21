## Formulários Reativos

Em formulários do tipo `Orientado a Dados` o form é criado programaticamente e gerenciado dentro da classe do componente assim como validações e controle dos dados. Esse gerenciamento é realizado a partir de uma instância de métodos e sincronizado através de proprieades com o Template. Para fazer o uso de formulários do tipo Reativo é necessário que no módulo seja declarado o módulo de formulário **`ReactiveFormsModule`**.

Os formulários reactivos diferem dos formulários orientados por modelos de forma distinta. Os formulários reativos fornecem acesso síncrono ao modelo de dados, imutabilidade com operadores observáveis e rastreamento de mudanças através de fluxos observáveis.

Os formulários dos tipo reativo fornece três blocos de construção fundamentais, *`FormControl`*, *`FormGroup`* e *`FormArray`*. Estas classes extendem a classe [**AbstractControl**](https://angular.io/api/forms/AbstractControl) que implementa a maior parte das funcionalidades básicas para acessar o valor, status de validação, interações do usuário e eventos.

### FormControl

Rastreia o valor e o status de validação de um controle de formulário individual. Só pode ser utilizado dentro de **grupos** ou **arrays**.

> **Explícito**

    user: formGroup;

    user = new FormGroup({
      name: new FormControl(),
    })
    
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

### FormGroup

Rastreia o valor e o estado de validade de um grupo de instâncias do FormControl. Agrega os valores de cada filho FormControl em um objeto, com cada nome de controle sendo uma chave.

    user: FormGroup;

    this.user = new FormGroup({
      name: new FormControl(),
    })
    
### FormArray

Rastreia o valor e o estado de validade de um array de instâncias FormControl, FormGroup ou FormArray. Agrega os valores de cada FormControl filho em uma matriz.
    
    user: formArray;

    user = new FormArray([
      new FormControl('Fulano'),
    ])

### FormBuilder

O Angular também fornece um método construtor de formulários que além de ajudar na leitura do código facilita na hora de escrever o mesmo. Utilizando a classe de serviço **[FormBuilder](https://angular.io/api/forms/FormBuilder)** que após ser injetada na classe, fornece três métodos para a criação de um formulário.

    constructor(private fb: FormBuilder) {}

> control()

Constrói um novo FormControl gerenciando estado, validadores e opções do contole

    user: formControl;
    
    this.user = this.fb.control({name: [], disabled: true})

> group()

Constrói uma nova instância do FormGroup
    
    user: FormGroup;
    
    this.user = this.fb.group({ name: [], disabled: true })

> array()

Constrói um novo FormArray a partir de um determinado conjunto de configurações, validadores e opções

    user: FormArray;
    
    this.user = this.fb.array([])


|Referências|
|-

- [reactive forms](https://angular.io/guide/reactive-forms)
- [formControl](https://angular.io/api/forms/FormControl)
- [formGroup](https://angular.io/api/forms/FormGroup)
- [formArray](https://angular.io/api/forms/FormArray)
- []()
