## Formulários Reativos - Propriedades

Formulários orientados a dados fornecem diversas maneiras para criação e gerenciamento de elementos de entrada de usuário, podendo este serem manipulados conforme necessidade e momentos oportunos através de classes e métodos utilitários.

### Classes

Utilizadas na classe dos componentes Angular fornecendo criação e manipulação de dados e elementos de controle de formulários diversos.

|Classe|Descrição|
|-|-|
|**AbstractControl** |Fornece comportamentos e propriedades comuns para as classes concretas de controle de formulários FormControl, FormGroup e FormArray |
|**FormControl**|Gerencia o valor e o status de validade de um controle de formulário individual. Corresponde a um controle de formulário HTML como inputs ou selects|
|**FormGroup**|Gerencia o valor e o estado de validade de um grupo de instâncias de AbstractControl. As propriedades do grupo incluem seus controles filho. O formulário de nível superior em seu componente é FormGroup|
|**FormArray**|Gerencia o valor e o estado de validade de um array de instâncias de AbstractControl numericamente indexado|
|**FormBuilder**|Um serviço injetável que fornece métodos de fábrica para criar instâncias de controle|

### Diretivas

Utilizadas no template dos componentes Angular fornecendo manipulação e controle de dados para elementos de entrada pelo usuário.

|Diretiva|Descrição|
|-|-|
|**FormControl**|Sincroniza uma instância FormControl autônoma com um elemento de controle de formulário|
|**FormControlName**|Sincroniza FormControl em uma instância de FormGroup com um elemento de controle de formulário por nome|
|**FormGroup**|Sincroniza uma instância de FormGroup existente com um elemento DOM|
|**FormGroupName**|Sincroniza uma instância de FormGroup aninhada com um elemento DOM|
|**FormArrayName**|Sincroniza uma instância de FormArray aninhada com um elemento DOM|

## Descrições gerais

Todas as descrições e funcionalidades fornecidas pelas classes utilitárias.

### FormControl

- **`...`**

