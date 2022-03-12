# Propriedades de Rotas

Rotas aceitam as mais diversas configurações para poderem ser aplicadas e consequentemente atender a qualquer aplicação com as mais diversas arquiteturas.

## Routes

Um objeto de configuração que define uma única rota. Um conjunto de rotas é coletado em uma **array** de rotas para definir uma configuração de roteador. O roteador tenta combinar segmentos de uma determinada `URL` com cada rota, usando as opções de configuração definidas.

### path?: string

O caminho para percorrer. Uma string de URL que usa a notação de correspondência de roteador. Não pode ser usado junto com a propriedade **matcher**. Pode ser um wildCard " ** "  que corresponda a qualquer URL. **`O padrão é "/" (o caminho raiz)`**.

    [{
      path: 'team/:id',
      component: Team,
    }]

### pathMatch?: string

A estratégia de correspondência de caminho, pode ser um **'prefixo'** pré definido ou **'full'**. **`O padrão é 'prefixo'`**. Por padrão, o roteador verifica os elementos de URL da esquerda para ver se o URL corresponde a um determinado caminho e para quando há uma correspondência de configuração. `É importante que ainda haja uma correspondência de configuração para cada segmento da URL`. Por exemplo:

> OK
> 
> O link **'/team/11/user'** corresponde ao prefixo `'team/:id'` se um dos filhos da rota corresponder ao segmento `'user'`
>  
>  Corresponde à configuração **`{path: 'team/:id', children: [{path: ':user', component: User}]}`**

> ERRO
> 
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

### component?: Type<any>
  
O componente a ser instanciado quando o caminho corresponder. Pode estar vazio se as rotas filhas especificarem componentes.

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

#### loadChildren?: LoadChildren
  
Um objeto que especifica rotas filho de carregamento lento.
  
    [{
      path: 'lazy',
      loadChildren: () => import('./lazy-route/lazy.module').then(mod => mod.LazyModule),
    }];

### runGuardsAndResolvers?: RunGuardsAndResolvers

Define quando os guardas e resolvedores serão executados. Pode ser **paramsOrQueryParamsChange** que é executado quando os parâmetros de consulta são alterados ou 
**always** que será sempre executado. Por padrão, os guards e os resolvedores são executados somente quando os parâmetros do array da rota são alterados.
  
## rout
  
  
  - [route](https://angular.io/api/router/Route)
  - [c]()
  
