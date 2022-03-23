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

Estende a classe AbstractControl que implementa a maior parte da funcionalidade básica para acessar o valor, status de validação, interações do usuário e eventos.

- **`setValue()`**

> Define um novo valor para o controle do formulário
>
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o valor de controle é atualizado. Quando false, nenhum evento é emitido
> 
> emitModelToViewChange: Quando true ou não fornecido (padrão), cada alteração aciona um evento onChange para atualizar a exibição
> 
> emitViewToModelChange: Quando true ou não fornecido (padrão), cada alteração aciona um evento ngModelChange para atualizar o modelo

- **`patchValue()`**

> Corrige o valor de um controle
> 
> Esta função é funcionalmente a mesma que setValue a este nível. Ela existe para simetria com o patchValue em FormGroups e FormArrays, onde ela se comporta de forma diferente

- **`reset()`**

> Redefine o controle de formulário, marcando-o como novo e intocado e redefinindo o valor. O novo valor será o valor fornecido (se passado), null ou o valor inicial se initialValueIsDefault foi definido no construtor por meio de FormControlOptions
>
> Redefine o controle com um valor inicial ou um objeto que define o valor inicial e o estado desabilitado
> 
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é redefinido. Quando false, nenhum evento é emitido

- **`registerOnChange()`**

> Registra um ouvinte para eventos de alteração
> 
> O método que é chamado quando o valor muda

- **`registerOnDisabledChange()`**

> Registra um ouvinte para eventos deficientes
> 
> O método que é chamado quando o status de desativado muda

### FormGroup

Estende a classe AbstractControl que implementa a maior parte da funcionalidade básica para acessar o valor, status de validação, interações do usuário e eventos.

- **`registerControl()`**

> Registra um controle com a lista de controles do grupo
>
> Este método não atualiza o valor ou a validade do controle. Utilizar addControl() em seu lugar

- **`addControl()`**

> Adiciona um controle a este grupo
> 
> emitEvent: Quando true ou não fornecido (padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é adicionado. Quando false, nenhum evento é emitido
> 
> Se já existir um controle com um determinado nome, ele não será substituído por um novo. Em necessidade de se substituir um controle existente, usar o método setControl. Este método também atualiza o valor e a validade do controle

- **`removeControl()`**

> Remova um controle deste grupo
> 
> emitEvent: Quando true ou não fornecido (padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é removido. Quando false, nenhum evento é emitido
> 
> Este método também atualiza o valor e a validade do controle

- **`setControl()`**

> Substitui um controle existente
> 
> emitEvent: Quando true ou não fornecido (padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é substituído por um novo. Quando false, nenhum evento é emitido
> 
> Se um controle com um determinado nome não existir neste FormGroup, ele será adicionado

- **`contains()`**

> Verifica se há um controle habilitado com o nome fornecido no grupo
> 
> Relatórios falsos para controles desabilitados. Para verificar a existência apenas no grupo, usar get() em vez disso

- **`setValue()`**

> Define o valor do FormGroup. Ele aceita um objeto que corresponda à estrutura do grupo, com nomes de controle como chaves
> 
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o valor de controle é atualizado. Quando false, nenhum evento é emitido

- **`patchValue()`**

> Corrige o valor do FormGroup. Ele aceita um objeto com nomes de controle como chaves e faz o possível para corresponder os valores aos controles corretos no grupo
> 
> onlySelf: Quando true, cada alteração afeta apenas este controle e não seu pai. O padrão é verdadeiro
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o valor de controle é atualizado. Quando false, nenhum evento é emitido. As opções de configuração são passadas para o método updateValueAndValidity
> 
> Ele aceita superconjuntos e subconjuntos do grupo sem gerar um erro

- **`reset()`**

> Redefine o FormGroup, marca todos os descendentes como originais e intocados e define o valor de todos os descendentes como nulo
>
> onlySelf: Quando verdadeiro, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
>
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é redefinido. Quando false, nenhum evento é emitido. As opções de configuração são passadas para o método updateValueAndValidity
> 
> Redefine para um estado de formulário específico passando um mapa de estados que corresponde à estrutura do formulário, com nomes de controle como chaves. O estado é um valor autônomo ou um objeto de estado de formulário com um valor e um status desabilitado.

- **`getRawValue()`**

> O valor agregado do FormGroup, incluindo quaisquer controles desabilitados
> 
> Recupera todos os valores, independentemente do status desabilitado. A propriedade value é a melhor forma de obter o valor do grupo, pois exclui controles desabilitados no FormGroup

### FormArray

Um FormArray agrega os valores de cada FormControl filho em um array. Ele calcula seu status reduzindo os valores de status de seus filhos. Por exemplo, se um dos controles em um FormArray for inválido, o array inteira se tornará inválido.

- **`at()`**

> Obtenha o AbstractControl no índice fornecido no array

- **`push()`**

> Insira um novo AbstractControl no final do array
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é inserido. Quando false, nenhum evento é emitido

- **`insert()`**

> Insira um novo AbstractControl no índice fornecido no array
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é inserido. Quando false, nenhum evento é emitido

- **`removeAt()`**

> Remova o controle no índice fornecido no array
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é removido. Quando false, nenhum evento é emitido

- **`setControl()`**

> Substitua um controle existente
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é substituído por um novo. Quando false, nenhum evento é emitido

- **`setValue()`**

> Define o valor do FormArray. Ele aceita um array que corresponde à estrutura do controle 
> 
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o valor de controle é atualizado. Quando false, nenhum evento é emitido. As opções de configuração são passadas para o método updateValueAndValidity
> 
> Esse método executa verificações rigorosas e gera um erro se você tentar definir o valor de um controle que não existe ou se você excluir o valor de um controle

- **`patchValue()`**

> Corrige o valor do FormArray. Ele aceita um array que corresponde à estrutura do controle e faz o possível para corresponder os valores aos controles corretos no grupo
> 
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o valor de controle é atualizado. Quando false, nenhum evento é emitido. As opções de configuração são passadas para o método updateValueAndValidity
> 
> Ele aceita superconjuntos e subconjuntos do grupo sem gerar um erro

- **`reset()`**

> Redefine o FormArray e todos os descendentes são marcados como pristine e untouched, e o valor de todos os descendentes para mapas nulos ou nulos
>
> onlySelf: Quando true, cada alteração afeta apenas este controle, e não seu pai. O padrão é falso
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle é redefinido. Quando false, nenhum evento é emitido. As opções de configuração são passadas para o método updateValueAndValidity
> 
> Você redefine para um estado de formulário específico passando um array de estados que corresponde à estrutura do controle. O estado é um valor autônomo ou um objeto de estado de formulário com um valor e um status desabilitado.

- **`getRawValue()`**

> O valor agregado do array, incluindo quaisquer controles desabilitados
> 
> Relata todos os valores, independentemente do status desabilitado. Apenas para controles habilitados, a propriedade value é a melhor maneira de obter o valor do array

- **`clear()`**

> Remove todos os controles no FormArray
> 
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando todos os controles nesta instância de FormArray são removidos. Quando false, nenhum evento é emitido
