## Arquivos referentes a aplicação

> Pasta Raíz

| Arquivo                 | Descrição |
|-                        |- |
| .editorconfig           | Configuração para editores de código |
| .gitignore              | Especificação de arquivos não rastreados intencionalmente que o Git deve ignorar |
| .npmrc                  | Otimização da configuração do ambiente Node.js |
| .prettierrc             | Configuração a cerca do formatador de código padrão |
| .stylelintrc.json       | Configuração dfos padrões de estilos a serem aplicados no projeto |
| angular.json            | Configuração da CLI para todos os projetos no workspace, incluindo opções de configuração para ferramentas de compilação, serviço e teste que a CLI usa, como Karma e Protractor |
| browserlist             | Configuração do compartilhamento dos navegadores de destino e versões do Node.js entre várias ferramentas de front-end |
| docker-compose.dev.yml  | Configuração do ambiente de desenvolvimento no contâiner |
| docker-compose.prod.yml | Configuração do ambiente de produção no contâiner |
| docker-compose.qa.yml   | Configuração do ambiente de quality assurance no contâiner |
| docker-compose.yml      | Configuração do ambiente normal no contâiner |
| Dockerfile              | Configuração das entradas entre a aplicação e o contâiner |
| karma.confi.js          | Configuração do Karma específica do aplicativo |
| ngcc.config.js          | Configuração do arquivo que  que recompila vários pacotes npm para o formato Ivy |
| nginx.conf              | Configuração das entradas para o servidor nginx |
| package.json            | Configuração das dependências do pacote npm que estão disponíveis para todos os projetos no workspace |
| README.md               | Documentação introdutória para o aplicativo raiz |
| tsconfig.app.json       | Configuração de TypeScript específica do aplicativo, incluindo opções de compilador de modelo TypeScript e Angular |
| tsconfig.json           | Configuração básica do TypeScript para projetos no espaço de trabalho. Todos os outros arquivos de configuração herdam deste arquivo base |
| tsconfig.spec.json      | Configuração do TypeScript para os testes do aplicativo |
| tslint.json             | Configuração da ferramenta de análise estática extensível que verifica o código TypeScript quanto a erros de legibilidade, manutenção e funcionalidade |
| webpack.config.js       | Configurações da ferramenta que trata as importações utilizadas pela aplicação |

![image](https://user-images.githubusercontent.com/24658433/155579673-941ffd6a-1607-456f-a42a-c53ab27f3127.png)

> Pasta src

| Arquivo       | Descrição       |
|-              |-                |
| favicon.ico   | Um ícone a ser usado para este aplicativo na barra de favoritos |
| index.html    | A página HTML principal exibida quando alguém visita seu site. A CLI adiciona automaticamente todos os arquivos JavaScript e CSS ao criar o aplicativo|
| main.ts       | O principal ponto de entrada para seu aplicativo. Compila o aplicativo com o compilador JIT e inicializa o módulo raiz do aplicativo (AppModule) para ser executado no navegador|
| polyfills.ts  | Fornece scripts polyfill para suporte aos navegadores |

  
![image](https://user-images.githubusercontent.com/24658433/155577568-97ca8fd9-d32e-4403-b0a9-61030b70409e.png)
