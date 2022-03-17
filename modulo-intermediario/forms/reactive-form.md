## Formulários Reativos

Em formulários do tipo `Orientado a Dados` o form é criado programaticamente e gerenciado dentro da classe do componente assim como validações e controle dos dados. Esse gerenciamento é realizado a partir de uma instância de um **`FormGroup`** e sincronizado através de proprieades com o Template.


podemos construir fomularios atraves de instâncias de FormGroup() com formControl()

    form_: FormGroup();

    this.form_ = new FormGroup({
        name: new FormControl(null),
        email: new FormControl(null),
    })

Ou com FormBuilder() que possui 3 porpriedades dec criação (group - array - control)

    constructor(private fb: FormBuilder) {}

    this.form_ = this.fb.group({
        nome: [null],
        email: [null]
    })




|Referências|
|-

- [reactive forms](https://angular.io/guide/reactive-forms)
