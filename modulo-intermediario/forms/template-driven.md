## Formulários - Template Driven

Em formulários do tipo `Orientado por Template` o form tem sua configuração criada e manipulada no próprio arquivo **HTML**, assim como controle e validações. É criada uma propriedade **`FormGroup`** que fica responsável pelo gerenciamento dos dados através do Template tendo estes valores sendo submetidos por **`ngSubmit`**. 

Na construção do formulário o Angular encarrega-se de criar um `Formulário Reativo` por trás do formulário orientado e template para gerenciar e controlar os modelos.

### Desrcrição

Os formulários controlados por modelo dependem de diretivas definidas no **FormsModule**. A diretiva **`NgModel`** reconcilia as alterações de valor no elemento de formulário anexado com as alterações no modelo de dados, permitindo que você responda à entrada do usuário com validação de entrada e tratamento de erros.

A diretiva `NgForm` cria uma instância **FormGroup** de nível superior e a vincula a um elemento **`<form>`** para rastrear o valor agregado do formulário e o status de validação. Assim que você importa FormsModule, esta diretiva se torna ativa por padrão em todas as tags `<form>`. A diretiva **NgModelGroup** cria e vincula uma instância FormGroup a um elemento DOM.

Para obter acesso ao NgForm e ao status geral do formulário, declare uma variável de referência de modelo.

<form #form="ngForm"> ... </form>

A variável de modelo form agora é uma referência à instância da diretiva `NgForm` que controla o formulário como um todo.

|Referências|
|-|

- [template-driven](https://angular.io/guide/forms)
