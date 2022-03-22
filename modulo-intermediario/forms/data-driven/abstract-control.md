## Classe Abstrata de Controles

Fornece alguns dos comportamentos compartilhados que todos os controles e grupos de controles têm, como executar validadores, calcular o status e redefinir o estado. 
Também define as propriedades que são compartilhadas entre todas as subclasses, como `value`, `valid` e `dirty`. **Não devendo ser instanciada diretamente**.

### Propriedades

- **`value`**

> Para um FormControl, o valor atual
>
> Para um FormGroup habilitado, os valores dos controles habilitados como um objeto com um par chave-valor para cada membro do grupo
>
> Para um FormGroup desabilitado, os valores de todos os controles como um objeto com um par chave-valor para cada membro do grupo
>
> Para um FormArray, os valores dos controles habilitados como um array

- **`validator`**

> Retorna a função usada para determinar a validade desse controle de forma síncrona. Se vários validadores foram adicionados, esta será uma única função composta

- **`asyncValidator`**

> Retorna a função que é usada para determinar a validade desse controle de forma assíncrona. Se vários validadores foram adicionados, esta será uma única função composta

- **`parent`**

> O controle dos elementos pais

- **`status`**

> O status de validação do controle. Esses valores de status são mutuamente exclusivos, ou seja, um controle não pode ser válido E inválido ou inválido E desabilitado

- **`valid`**

> Um controle é válido quando seu status é VALID

- **`invalid`**

> Um controle é inválido quando seu status é INVALID

- **`pending`**

> Um controle está pendente quando seu status é PENDING

- **`disabled`**

> Um controle é desabilitado quando seu status é DISABLED
>
> Os controles desabilitados estão isentos de verificações de validação e não são incluídos no valor agregado de seus controles ancestrais

- **`enabled`**

> Um controle está habilitado desde que seu status não seja DISABLED

- **`erros`**

> Um objeto contendo quaisquer erros gerados por falha na validação ou nulo se não houver erros

- **`pristine`**

> Um controle está intacto se o usuário ainda não alterou o valor na interface do usuário

- **`dirty`**

> Um controle está sujo se o usuário alterou o valor na interface do usuário

- **`touched`**

> True se o controle estiver sido tocado. Um controle é marcado como tocado quando o usuário aciona um evento de blur nele

- **`untouched`**

> Verdadeiro se o controle não tiver sido tocado. Um controle não é tocado se o usuário ainda não acionou um evento de blur nele

- **`valueChanges`**

> Um observável de multicast que emite um evento toda vez que o valor do controle é alterado, na interface do usuário ou programaticamente. Ele também emite um evento cada vez que você chama enable() ou disable() sem passar {emitEvent: false} como um argumento da função

- **`statusChanges`**

> Um observável de multicast que emite um evento toda vez que o status de validação do controle é recalculado

- **`updateOn`**

> Relata a estratégia de atualização do AbstractControl (ou seja, o evento no qual o controle se atualiza). Valores possíveis: change, blur e submit

- **`root`**

> Recupera o ancestral de nível superior desse control

### Métodos

- **`setValidators()`**

> Define os validadores síncronos que estão ativos neste controle. Ao invocá-la isto substituirá quaisquer validadores síncronos existentes
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito
> 
> Para adicionar um novo validador sem afetar os existentes, utilizar o método addValidators()

- **`setAsyncValidators()`**

> Define os validadores assíncronos que estão ativos neste controle. Ao invocá-la isto substituirá quaisquer validadores assíncronos existentes
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito
>
> Para adicionar um novo validador sem afetar os existentes, utilizar o método addAsyncValidators()

- **`addValidators()`**

> Adiciona um validador ou validadores síncronos a esse controle, sem afetar outros validadores
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito
>
> Adicionar um validador que já existe não terá efeito. Se funções de validador duplicadas estiverem presentes no array de validadores, somente a primeira instância será adicionada a um controle de formulário

- **`addAsyncValidators()`**

> Adiciona um validador ou validadores assíncronos a este controle, sem afetar outros validadores
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito
>
> Adicionar um validador que já existe não terá efeito

- **`removeValidators()`**

> Remove um validador síncrono deste controle, sem afetar outros validadores. Os validadores são comparados por referência de função; você deve passar uma referência para a mesma função validadora que foi definida originalmente. Se um validador fornecido não for encontrado, ele será ignorado
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito

- **`removeAsyncValidators()`**

> Remove um validador assíncrono deste controle, sem afetar outros validadores. Os validadores são comparados por referência de função; você deve passar uma referência para a mesma função validadora que foi definida originalmente. Se um validador fornecido não for encontrado, ele será ignorado
>
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito

- **`hasValidator()`**

> Verifica se uma função de validação síncrona está presente neste controle. O validador fornecido deve ser uma referência à mesma função que foi fornecida

- **`hasAsyncValidator()`**

> Verifica se uma função de validação assíncrona está presente neste controle. O validador fornecido deve ser uma referência à mesma função que foi fornecida

- **`clearValidators()`**

> Esvazia a lista de validadores síncronos
> 
> Ao adicionar ou remover um validador em tempo de execução, deve-se chamar updateValueAndValidity() para que a nova validação tenha efeito

- **`clearAsyncValidators()`**

> Esvazia a lista de validadores assíncronos
> 
> Ao adicionar ou remover um validador em tempo de execução, você deve chamar updateValueAndValidity() para que a nova validação tenha efeito

- **`markAsTouched()`**

> Marca o controle como tocado. Um controle é tocado por eventos de foco e desfoque que não alteram o valor
> 
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos

- **`markAsDirty()`**

> Marca o controle como sujo. Um controle fica sujo quando o valor do controle é alterado por meio da interface do usuário
> 
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso

- **`markAsPristine()`**

> Marca o controle como intocado
>
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso

- **`markAsPending()`**

> Marca o controle como pendente
> 
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso
>
> emitEvent: Quando true ou não fornecido (padrão), o observável statusChanges emite um evento com o status mais recente que o controle está marcado como pendente. Quando false, nenhum evento é emitido

- **`disabled()`**

> Desativa o controle. Isso significa que o controle está isento de verificações de validação e excluído do valor agregado de qualquer pai. Seu status é DESATIVADO
>
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso.
> 
> emitEvent: Quando true ou não fornecido (padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle está desabilitado. Quando false, nenhum evento é emitido
>
> Se o controle tiver filhos, todos os filhos também serão desabilitados

- **`enable()`**

> Habilita o controle. Isso significa que o controle está incluído nas verificações de validação e no valor agregado de seu pai. Seu status é recalculado com base em seu valor e em seus validadores
> 
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle está habilitado. Quando false, nenhum evento é emitido
> Por padrão, se o controle tiver filhos, todos os filhos serão habilitados

- **`setParent()`**

> Define o pai do controle

- **`setValue()`**

> Define o valor do controle. Método abstrato (implementado em subclasses)

- **`patchValue()`**

> Corrige o valor do controle. Método abstrato (implementado em subclasses)

- **`reset()`**

> Redefine o controle. Método abstrato (implementado em subclasses)

- **`updateValueAndValidity()`**

> Recalcula o valor e o status de validação do controle
> 
> onlySelf: Quando verdadeiro, marca apenas este controle. Quando falso ou não fornecido, marca todos os ancestrais diretos. O padrão é falso
> emitEvent: Quando true ou não fornecido (o padrão), os observáveis statusChanges e valueChanges emitem eventos com o status e o valor mais recentes quando o controle está habilitado. Quando false, nenhum evento é emitido
> Por padrão, também atualiza o valor e a validade de seus ancestrais

- **`setErrors()`**

> Define erros em um controle de formulário ao executar validações manualmente, em vez de automaticamente
> Invocar setErrors também atualiza a validade do controle pai

- **`get()`**

> Recupera um controle filho dado o nome ou caminho do controle
> 
> Uma string delimitada por pontos ou uma matriz de valores de string/número que definem o caminho para o controle

    // Exemplo com FormGroup()
    this.form.get('user.name');
    this.form.get(['user', 'name']);

    // Exemplo com FormArray()
    this.form.get('products.0.price');
    this.form.get(['products', 0, 'price']);

- **`getError()`**

> Informa dados de erro para o controle com o caminho fornecido

- **`hasError()`**

> Informa se o controle com o caminho fornecido tem o erro especificado
