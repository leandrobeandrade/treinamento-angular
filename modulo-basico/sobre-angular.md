## Breve Introdução

> [Angular](https://angular.io/) (comumente referido como *"Angular 2+" ou "Angular 2"*) é uma plataforma de aplicações web de código-fonte aberto e front-end baseado em **TypeScript** liderado pela Equipe Angular do Google.

### Algumas características

- Angular não tem um conceito de "escopo" ou controladores, em vez disso, ele usa uma hierarquia de componentes como o seu principal conceito arquitetônico
- Angular tem uma expressão diferente de sintaxe, concentrando-se no uso de **`[ ]`** para a propriedade de ligação, e no uso de **`( )`** para ligação de evento
- Modularidade – muito das funcionalidades principais foram movidas para os módulos 
- Angular recomenda o uso da linguagem da Microsoft **TypeScript**, que apresenta as seguintes características:
  - É baseada em classes com o paradigma de programação orientada a objetos
  - Tipagem estática
  - Gerenciamento simples e eficáz de Injeção de Dependências (DI)
  - Suporte a programação reativa utilizando `RxJS` entre outras bibliotecas
  
### Outras características específicas

- Baseada em componentes (WEB Components)
- Seus componentes são considerados diretivas com templates
- Suas diretivas são consideradas componentes sem templates

#### Componentes

O conceito de componentes é fundamental quando falamos de framework para front-end. Praticamente tudo se baseia neles, que são responsáveis por permitirem a criação de códigos,
que podem ser reutilizados e testados sem o risco de colisões. Uma aplicação Angular é iniciada por um componente principal, o **AppComponent**. 
A partir dele, conecta-se uma hierarquia de outros componentes ao modelo de objeto de documento de página `(DOM)`.

#### Módulos

Com a utilização do Angular, um aplicativo é definido por uma junção de módulos. Se imaginarmos os módulos como blocos que podem ser utilizados para construir coisas, 
no Angular, essa ação se traduziria em agrupar, exportar e esconder componentes, diretivas, pipes e serviços relacionados.
Esses módulos servem para formar uma aplicação e são chamados de NgModules. Cada aplicação é composta por pelo menos uma categoria dessa classificação, que é o módulo root 
da aplicação.

![modules](https://user-images.githubusercontent.com/24658433/155215901-90f347b0-33c9-4dd1-8b21-29fbe6ae88c6.png)

    Propriedades de módulos

**1. Imports:** são arranjos com outros módulos, necessários para utilizar componentes declarados dentro da aplicação

**2. Declarations:** recebe um arranjo de componentes, diretivas e pipes, que fazem parte do módulo

**3. Exports:** Define o conjunto de componentes e pipes, disponíveis para outros módulos

**4. Providers:** Faz a declaração dos serviços, onde, se um módulo for root, eles estarão disponíveis para toda a aplicação

> Módulos mais comumente utilizados

|NgModule                                 |	Importar de               |Por que você usa|
|-                                        |-                          |-               |
|BrowserModule                            |	@angular/platform-browser |	Executar o aplicativo em um navegador|
|CommonModule                             |	@angular/common           |	Utilizar NgIf, NgFor, diretivas em geral|
|FormsModule                              |	@angular/forms            |	Criar formulários orientados por modelo (inclui NgModel)|
|ReactiveFormsModule                      |	@angular/forms            |	Criar formulários reativos|
|RouterModule                             | @angular/router           |	Utilizar RouterLink, .forRoot(), e.forChild()|
|HttpClientModule                         |	@angular/common/http      |	Integrar com um servidor usando o protocolo HTTP|

#### Two-way data binding

Essa é uma das principais características do framework. O termo pode ser definido como uma associação de dados bidirecional, onde a informação entra através da visualização
ou template, passando instantaneamente para uma propriedade da classe do componente. O dado em questão já é exibido automaticamente em um elemento do DOM (Document Object Model)
no template do componente.

A principal proposta do two-way data binding é automatizar a circulação de dados, facilitando a vida do desenvolvedor ao não exigir a criação de handlers para atualizar a 
visualização.

Dessa forma, quando um valor de um componente mudar, o próprio framework realizará a atualização na página. A ligação de dados bidirecional combina a entrada e saída em um 
único processo.

|Referências|
|-|

- [commom modules](https://angular.io/guide/frequent-ngmodules#browsermodule-and-commonmodule)
