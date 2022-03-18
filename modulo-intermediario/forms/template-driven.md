## Formulários - Template Driven

Em formulários do tipo `Orientado por Template` o form tem sua configuração criada e manipulada no próprio arquivo **HTML**, assim como controle e validações. É criada uma propriedade **`FormGroup`** que fica responsável pelo gerenciamento dos dados através do Template tendo estes valores sendo submetidos por **`ngSubmit`**. 

Na construção do formulário o Angular encarrega-se de criar um `Formulário Reativo` por trás do formulário orientado e template para gerenciar e controlar os modelos.

### Descrição

Os formulários controlados por modelo dependem de diretivas definidas no **FormsModule**. A diretiva **`NgModel`** reconcilia as alterações de valor no elemento de formulário anexado com as alterações no modelo de dados, permitindo que você responda à entrada do usuário com validação de entrada e tratamento de erros.

A diretiva `NgForm` cria uma instância **FormGroup** de nível superior e a vincula a um elemento **`<form>`** para rastrear o valor agregado do formulário e o status de validação. Assim que você importa FormsModule, esta diretiva se torna ativa por padrão em todas as tags `<form>`. A diretiva **NgModelGroup** cria e vincula uma instância FormGroup a um elemento DOM.

Para obter acesso ao NgForm e ao status geral do formulário, declare uma variável de referência de modelo.

    <form #form="ngForm"> ... </form>

A variável de modelo form agora é uma referência à instância da diretiva `NgForm` que controla o formulário como um todo.

Ao utilizar **[(ngModel)]** em um elemento, você deve definir um atributo **`name`** para esse elemento. Angular usa o nome atribuído para registrar o elemento com a diretiva NgForm anexada ao elemento pai `<form>`.

#### Estados de controle de rastreamento
  
A diretiva NgModel em um controle rastreia o estado desse controle. Ele informa se o usuário tocou no controle, se o valor mudou ou se o valor se tornou inválido. Angular define classes **`CSS`** especiais no elemento de controle para refletir o estado, conforme a tabela a seguir.

|Estado                           | Se verdadeiro |Se falso     |
|-                                |-              |-            |
|O controle foi visitado          |ng-touched     |ng-untouched |
|O valor do controle foi alterado |ng-dirty       |ng-pristine  |
|O valor do controle é válido     |ng-valid       |ng-invalid   |

A utilização dessas classes CSS servem para definir os estilos para o controle com base em seu status. Além disso, o Angular aplica a classe **ng-submitted** aos elementos `<form>` após o envio. Esta classe não se aplica a controles internos, apenas leitura.

Nas ferramentas de desenvolvedor do navegador, o elemento `<input>` que corresponde ao input correspondente poderá ser visto que o elemento possui várias classes CSS além de "controle de formulário".

    <input ... class="form-control ng-untouched ng-pristine ng-valid" ...>
    
#### Criando feedback visual para estados

As classes **ng-valid/ng-invalid** são particularmente interessantes, porque podem caracterizar um sinal visual forte quando os valores são inválidos. Também podendo marcar os campos como obrigatórios. Pode-se marcar campos obrigatórios e dados inválidos ao mesmo tempo com uma barra colorida à esquerda da caixa de entrada por exemplo:
 
![image](https://user-images.githubusercontent.com/24658433/158731835-8de9294a-4b32-4eb1-a9e5-d1e83a32687e.png)
![image](https://user-images.githubusercontent.com/24658433/158732129-d72a6cb4-f45e-4e77-a7c8-b04223742504.png)

    .ng-valid[required], .ng-valid.required  {
      border-left: 5px solid #42A948; /* green */
    }

    .ng-invalid:not(form)  {
      border-left: 5px solid #a94442; /* red */
    }
 
 #### Envio do formulário com ngSubmit
 
O usuário deve ser capaz de enviar o formulário após preenchê-lo. O botão Enviar na parte inferior do formulário não faz nada sozinho, mas aciona um evento de envio de formulário devido ao seu tipo **`(type="submit")`**. Para responder a este evento, vincule a propriedade de evento **ngSubmit** do formulário ao método `onSubmit()` do componente de formulário.

    <form #form="ngForm" (ngSubmit)="onSubmit()">
 
Com a variável de referência de modelo, `#form` para acessar o formulário que contém o botão **Enviar** cria-se uma associação de evento que será vinculado a propriedade do formulário que indica sua validade geral à propriedade desabilitada do botão Enviar.
 
    <button type="submit" class="btn btn-success" [disabled]="!form.valid">Submit</button>
    
## NgModel

Propriedade responsável em criar uma instância **`FormoControl`** de um modelo de domínio *(propriedade da classe)* e a associá-lo a um elemento de controle de formulário.

### Descrição

A instância `FormControl` rastreia o valor, a interação do usuário e o status de validação do controle e mantém a exibição sincronizada com o modelo. Se usado em um formulário pai, a diretiva também se registra no formulário como um controle filho. Esta diretiva é usada sozinha ou como parte de um formulário maior.

Se existir uma ligação **unidirecional** para ngModel com sintaxe **`[]`**, alterar o valor do modelo de domínio na classe do componente definirá o valor na exibição. Se você tiver uma ligação bidirecional com a sintaxe **`[()]`** *(também conhecida como 'sintaxe banana-in-a-box')*, o valor na interface do usuário sempre será sincronizado com o modelo de domínio em sua classe **(two-way binding)**.

Para inspecionar as propriedades do FormControl associado (como o estado de validade), exporte a diretiva para uma variável de modelo local usando `ngModel como chave`

    <input type="text" name="age" #myVar="ngModel" [(ngModel)]="user.age" />
        
    <div>{{ myVar.value | json }}</div>

Você pode então acessar o controle usando a propriedade control da diretiva. No entanto, as propriedades mais usadas (como valid e dirty) também existem no controle para acesso direto. Veja uma lista completa de propriedades diretamente disponíveis em [AbstractControlDirective](https://angular.io/api/forms/AbstractControlDirective).

#### Outras formas de utilização ngModel

É fornecido algumas formas de vinculação de propriedades do componente com elementos de entrada de usuário como `inputs` para gerenciamento dos dados e controle pelo formulário abstrato criado implicitamente pelo Angular.

> Não é vinculado a nenhum model de classe, apenas cria uma propriedade no formulário abstrato
    
    <input type="text"  name="nome" ngModel />

> Vincula uma propriedade ao model do componente, mas, não atualiza este model e cria uma propriedade no formulário abstrato
    
    <input type="text" name="nome" [ngModel]="user.name" />

> Vincula uma propriedade ao model do componente, mas, não atualiza este model e não cria uma propriedade no formulário abstrato
    
    <input type="text" name="name4" [value]="user.name" />

|Referências|
|-|

- [template-driven](https://angular.io/guide/forms)
- [ngModel](https://angular.io/api/forms/NgModel)
