## Formulários Reativos

Em formulários do tipo `Orientado a Dados` o form é criado programaticamente e gerenciado dentro da classe do componente assim como validações e controle dos dados. Esse gerenciamento é realizado a partir de uma instância de métodos e sincronizado através de proprieades com o Template. Para fazer o uso de formulários do tipo Reativo é necessário que no módulo seja declarado o módulo de formulário **`ReactiveFormsModule`**.

Os formulários reactivos diferem dos formulários orientados por modelos de forma distinta. Os formulários reativos fornecem acesso síncrono ao modelo de dados, imutabilidade com operadores observáveis e rastreamento de mudanças através de fluxos observáveis.

Os formulários dos tipo reativo fornece três blocos de construção fundamentais, *`FormControl`*, *`FormGroup`* e *`FormArray`*. Estas classes extendem a classe [**AbstractControl**](https://angular.io/api/forms/AbstractControl) que implementa a maior parte da funcionalidades básicas para acessar o valor, status de validação, interações do usuário e eventos.

### FormControl

Rastreia o valor e o status de validação de um controle de formulário individual.

...

### FormGroup

Rastreia o valor e o estado de validade de um grupo de instâncias do FormControl.

...

|Referências|
|-

- [reactive forms](https://angular.io/guide/reactive-forms)
