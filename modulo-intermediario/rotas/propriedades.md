# Propriedades de Rotas

Rotas aceitam as mais diversas configurações para poderem ser aplicadas e consequentemente atender a qualquer aplicação com as mais diversas arquiteturas.

## Routes

Um objeto de configuração que define uma única rota. Um conjunto de rotas é coletado em uma **array** de rotas para definir uma configuração de roteador. O roteador tenta combinar segmentos de uma determinada `URL` com cada rota, usando as opções de configuração definidas.

> Propriedades

### path?: string

O caminho para percorrer. Uma string de URL que usa a notação de correspondência de roteador. Não pode ser usado junto com a propriedade **matcher**. Pode ser um wildCard " ** "  que corresponda a qualquer URL. **`O padrão é "/" (o caminho raiz)`**.

    [{
      path: 'team/:id',
      component: Team,
    }]

### pathMatch?: string

A estratégia de correspondência de caminho, pode ser um **'prefixo'** pré definido ou **'full'**. **`O padrão é 'prefixo'`**. Por padrão, o roteador verifica os elementos de URL da esquerda para ver se o URL corresponde a um determinado caminho e para quando há uma correspondência de configuração. `É importante que ainda haja uma correspondência de configuração para cada segmento da URL`. Por exemplo:

- Configuração Correta
   
    > O link **'/team/11/user'** corresponde ao prefixo `'team/:id'` se um dos filhos da rota corresponder ao segmento `'user'`
    > 
    > Corresponde à configuração **`{path: 'team/:id', children: [{path: ':user', component: User}]}`**

- Configuração Errada
     
    > O link **'/team/11/user'** corresponde ao prefixo `'team/:id'`
    >
    > Não corresponde quando não existem filhos como na configuração **`{path: 'team/:id', component: Team}`**

A estratégia de correspondência de caminho **'full'** corresponde a URL cheia. É importante fazer isso ao redirecionar rotas de caminho vazio. Caso contrário, como um caminho vazio é um prefixo de qualquer URL, o roteador aplicaria o redirecionamento ao navegar para o mesmo destino de redirecionamento, criando um loop infinito.

    [{
      path: '',
      pathMatch: 'full',
      redirectTo: 'team'
    }, {
      path: 'user',
      component: User
    }]

### matcher?: UrlMatcher

Uma função de correspondência de URL personalizada. Não pode ser usado junto com a propriedade **path**.

### component?: Type< any >
  
O componente a ser instanciado quando o caminho corresponder. Pode estar vazio se as rotas filhas especificarem componentes.

    [{
      path: 'team/:id',
      component: Team,
    }]

### redirectTo?: string
  
Uma URL para redirecionar quando o caminho corresponder. Pode ser **absoluto** se a URL começar com uma barra (/), caso contrário, **relativo** a URL do path. Observe que nenhum redirecionamento adicional é avaliado após um redirecionamento absoluto. Quando não está presente, o roteador não redireciona.
  
    [{
      path: '',
      pathMatch: 'full',
      redirectTo: 'team'
    }]

### outlet?: string
  
O nome de um objeto **RouterOutlet** onde o componente pode ser colocado quando o caminho corresponder.
  
    [{
      path: 'team/:id',
      component: Team
    }, {
      path: 'chat/:user',
      component: Chat
      outlet: 'aux'
    }]

### canActivate?: any[]
  
Um array de tokens de injeção de dependência usada para pesquisar manipuladores `CanActivate()`, a fim de determinar se o usuário atual tem permissão para ativar o componente. Por padrão, qualquer usuário pode estar ativo.

### canActivateChild?: any[]
  
Um array de tokens DI usados para pesquisar manipuladores `CanActivateChild()`, a fim de determinar se o usuário atual tem permissão para ativar um filho do componente. Por padrão, qualquer usuário pode estar ativo em um componente filho.

### canDeactivate?: any[]
  
Um array de tokens DI usados para pesquisar manipuladores `CanDeactivate()`, a fim de determinar se o usuário atual tem permissão para ser desativado para o componente. Por padrão, qualquer usuário pode estar desativado.

### canLoad?: any[]
  
Um array de tokens DI usados para pesquisar manipuladores `CanLoad()`, a fim de determinar se o usuário atual tem permissão para carregar o componente. Por padrão, qualquer usuário pode carregar.

### data?: Data
  
Dados adicionais definidos pelo desenvolvedor fornecidos ao componente via `ActivatedRoute`. Por padrão, nenhum dado adicional é passado.

### resolve?: ResolveData

Um mapa de tokens DI usados para pesquisar resolvedores de dados.

### children?: Routes
  
Um array de objetos `Route` de rotas filhas que especifica uma configuração de rota aninhada.

### loadChildren?: LoadChildren
  
Um objeto que especifica rotas filho de carregamento lento **`(lazy loading)`**. Rotas que fazem lazy loading tem o path do seu **routing.module** vazio `path: ''`
  
    // Módulo de roteamento pai
    [{
      path: 'child',
      loadChildren: () => import('./child/child.module').then(mod => mod.ChildModule),
    }];
    
    // Módulo de roteamento filho
    [{
      path: '',
      componente: ChildComponent
    }];

### runGuardsAndResolvers?: RunGuardsAndResolvers

Define quando os guardas e resolvedores serão executados. Pode ser **paramsOrQueryParamsChange** que é executado quando os parâmetros de consulta são alterados ou 
**always** que será sempre executado. Por padrão, os guards e os resolvedores são executados somente quando os parâmetros do array da rota são alterados.
  
## Navegação via Template e Componente

Quando aplicado a um elemento em um template ou componente, torna esse elemento um link que inicia a navegação para uma rota. A navegação abre um ou mais componentes roteados em um ou mais locais `<router-outlet>` na página.
  
### queryParams?: Params | null

Define os parâmetros de consulta para a URL.

- Via Componente

      this.router.navigate(['/user'], { queryParams: { page: 1 } });           // Navega para /user?page=1
    
- Via Template
        
      <a [routerLink]="['/user']" [queryParams]="{debug: true}">link</a>      // Navega para /user?page=1

### fragment?: string

Define o fragmento de hash para a URL.

- Via Componente
 
      this.router.navigate(['/user'], { fragment: 'top' });                    // Navega para /user#top
    
- Via Template

      <a [routerLink]="['/user']" fragment="top">link</a>                      // Navega para /user#top

### queryParamsHandling?: QueryParamsHandling | null

Gerencia os parâmetros de consulta no link do roteador para a próxima navegação. Utiliza `preserve` para preservar os parâmetros atuais e `merge` para mesclar novos parâmetros com parâmetros atuais. A opção "preserve" descarta quaisquer novos parâmetros de consulta.

- Via Componente

      // de /user?page=1 para /other?page=1
      this.router.navigate(['/other'], { queryParams: { page: 2 },  queryParamsHandling: "preserve"});
    
      // de /user?page=1 para /other?page=1&otherKey=2
      this.router.navigate(['/other'], { queryParams: { otherKey: 2 },  queryParamsHandling: "merge"});
  
- Via Template

      // de /user?page=1 para /other?page=1&otherKey=2
      <a [routerLink]="['/other']" [queryParams]="{otherKey: true}" queryParamsHandling="merge">link</a>

### preserveFragment: boolean

Quando true, preserva o fragmento de URL para a próxima navegação.

    // de /user#top para /other#top
    this.router.navigate(['/other'], { preserveFragment: true });

### skipLocationChange: boolean

Quando true, navega sem setar um novo estado para o histórico. Navegação silenciosa.

    this.router.navigate(['/user'], { skipLocationChange: true });

### replaceUrl: boolean

Quando true, navega enquanto substitui o estado atual no histórico. Várias navegações feitas em um curto período de tempo podem não substituir a URL corretamente. Como solução pode-se envolver uma função de navegação em um tempo limite para que cada uma seja acionada em um ciclo separado.

    this.router.navigate(['/user'], { replaceUrl: true });                   // Navega para /user

### state?: {[k: string]: any}

Especifica um valor de estado para ser pesrsistido pelo browser. Este valor pode ser retornado pelo método **`Router.getCurrentNavigation()`**.

    <a [routerLink]="['/user']" [state]="{tracingId: 123}">link</a>
    
    // Obtem o valor do state
    router.events.pipe(filter(e => e instanceof NavigationStart)).subscribe(e => {
      const navigation = router.getCurrentNavigation();
      tracingService.trace({id: navigation.extras.state.tracingId});
    });

### relativeTo?: ActivatedRoute | null

Especifica um valor quando não for utilizar `routerLink`, que é a rota ativada no momento. Um valor **indefinido** aqui usará o padrão routerLink.

    [{
      path: 'parent',
      component: ParentComponent,
      children: [{
        path: 'list',
        component: ListComponent
      },{
        path: 'child',
        component: ChildComponent
      }]
    }]
    
A seguinte função `go()` navega para a rota da lista interpretando o URI de destino como relativo à rota filha ativada.

    @Component({...})
    class ChildComponent {
      constructor(private router: Router, private route: ActivatedRoute) {}

      go() {
        this.router.navigate(['../list'], { relativeTo: this.route });
      }
    }

### routerLink: string | any[]

Anexa segmentos de URL à árvore de URL atual para criar uma nova árvore de URL.

    <a [routerLink]="['/user']">link</a>

### urlTree: UrlTree | null

Propriedade apenas de leitura que descreve a árvore URL da rota especificada .
  
  - [route](https://angular.io/api/router/Route)
  - [routerLink](https://angular.io/api/router/RouterLink)
  - [navigation extras](https://angular.io/api/router/NavigationExtras)
  - [navigation behavior](https://angular.io/api/router/NavigationBehaviorOptions)
  
