## Configurações de Rotas

Angular fornece uma arquitetura de gerenciamento e controle de roteamento completo para ser utilizado cobrindo as mais diversas formas de roteamento presentes em um aplicação.
O Framework por si já é totalmente baseado em modularização, e as rotas não são diferentes, o desejável é que cada módulo de funcionalidade criado na aplicação possua um módulo de roteamento para os componentes presentes neste módulo de funcionalidades.

### Exemplo do esqueleto de um módulo de roteamento padrão

Arquivo onde são declaradas as rotas para uso.

    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router';

    const routes: Routes = [];

    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule]
    })
    export class AppRoutingModule {}
    
    // AppModule
    import { AppRoutingModule } from './app-routing.module';
    
    ...
    
    imports: [AppRoutingModule]
    
### Composição das rotas

Na composição das rotas existem prorpiedades que setadas auxiliam na configuração do roteamento, sendo que as duas propriedades principais são `path` e `component`. Também pode ser configurado uma rota **`wildCard`** que renderiza um componente padrão quando a rota acessada não existir.

- `path (string)` - Responsável por descrever o caminho (pasta/url) do componente
- `component` - Responsável por descrever qual componente será renderizado quando aquele **path** for acessado

      const routes: Routes = [
        { path: 'login', component: LoginComponent },
        { path: 'home', component: HomeComponent },
        { path: 'dashboard', component: DashboardComponent },
        { path: '**', component: NotFoundComponent },             // rota wildCard
      ];
    
### Exibição do conteúdo

Quando configuramos uma rota e navegamos até ela, o Angular recupera a URL, checa no arquivo de rotas e trata de carregar o componente equivalente na tela se este existir.

- **navegação por links HTML** - Navegação realizada através de links configurados no Template

      <a routerLink="/login">Login</a>
      <a routerLink="/home">Home</a>
      <a routerLink="/dashboard">Dashboard</a>

- **navegação por componente** - Navegação realizada através de métodos de navegação Angular

      goToLogin() {
        this.router.navigate(['/login']);
      }

### Parâmetros na rotas

Também é fornecido a possibilidade da passagem de parâmetros pelas rotas, o que é comum quando se quer renderizar algo específico por um id ou qualquer outro parâmetro. É possível passar quantos parâmetros forem necessários na rota. Há duas formas de se passar parâmetros nas rotas em Angular, por **`parâmetros diretos`** e por **`consulta de parâmetros (queryParams)`**.

> Passagem de parâmetros diretos

Padrão determinado para parâmetros passados diretamente nas rotas: `dashboard/1`

- configuração dos parâmetros na rota
      
      { path: 'dashboard/:id', component: DashboardComponent },
      { path: 'dashboard/:id/:status', component: DashboardComponent }

- passagem do(s) parâmetro(s) pelo Template

      <a [routerLink]="['/dashboard', id]">Dashboard</a>
      
- passagem do(s) parâmetro(s) pelo componente

      constructor(private router: Router, private activatedRoute: ActivatedRoute) {}
      
      this.router.navigate(['./dashboard', this.id, this.status], {
        relativeTo: this.activatedRoute,
      });
      
- Obtenção dos parâmetros

      constructor(private activatedRoute: ActivatedRoute) {}

      this.id = this.activatedRoute.snapshot.paramMap.get('id');
      
> Passagem de parâmetros por queryParams

Padrão determinado para parâmetros passados diretamente nas rotas: `dashboard?id=1`

- configuração dos parâmetros na rota
      
      { path: 'dashboard', component: DashboardComponent },
      
- passagem do(s) parâmetro(s) pelo componente

      constructor(private router: Router, private activatedRoute: ActivatedRoute) {}
      
      this.route.navigate(['./dashboard'], {
        queryParams: { id: id },
        relativeTo: this.activatedRoute,
      });
      
- Obtenção dos parâmetros

      constructor(private activatedRoute: ActivatedRoute) {}

      this.activatedRoute.queryParams.subscribe((queryParams) => {
        this.id = Number(queryParams['id']);
      });
      
### Carregamento de rotas filhas

Sempre declarar as rotas no módulo raíz como sendo **`forRoot(routes)`**, pois este o módulo raíz que alimenta toda a aplicação e declarar **`forChild(routes)`** para todos os  outros módulos de rotas.

### Guardas de rotas - Guards

As guardas de rotas são cumulativas, ou seja, você pode ter vários arquivos de guardas, e todos eles seguem um mesmo padrão, implementam `CanActivate` e tem um método apenas, chamado também **`CanActivate`**, que retorna verdadeiro ou falso, informando se o usuário pode ou não chegar a esta página.

      // Arquivo de guarda
      import { CanActivate } from '@angular/router';
      import { Injectable } from '@angular/core';

      @Injectable()
      export class SampleGuard implements CanActivate {
        canActivate() {
          return false;
        }
      }
      
      // AppModule
      providers: [SampleGuard],

      const routes: Routes = [
        { path: 'login', component: LoginComponent },
        { path: 'signup', component: SignupComponent },
        {
          path: 'master',
          component: MasterComponent,
          canActivate: [SampleGuard],
          children: [
            { path: 'home', component: HomeComponent },
            { path: 'reports', component: ReportsComponent },
          ],
        },
      ];

- [router](https://angular.io/guide/router)
- [route](https://angular.io/api/router/Route)
