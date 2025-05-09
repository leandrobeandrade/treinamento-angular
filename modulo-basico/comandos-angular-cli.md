## Comandos Angular CLI (Command-Line Interface)

### Nível de projeto

| O que faz                                                   | Comando                         | Atalho                    |
| -                                                           |-                                | -                         |
| Instala Angular + CLI (Comand Line Interface)               | npm install -g @angular/cli     | npm i -g @angular/cli     |
| Cria um novo projeto * (Opcionais)                          | ng new app-test                 |                           |
| Entra no novo projeto                                       | cd app-test                     |                           |
| Executa o novo projeto e abre no navegador padrão           | ng serve --open                 | ng s -o                   |
|                                                                                                                           |
| * Não instala dependências no projeto                       | ng new app-test --skip-install  | ng new app-test -si       |
| * Não gera os arquivos "spec.ts" para testes                | ng new app-tets --skip-tests	  | ng new app-test -st	    |
| * Não inicializa o repositório local do git                 | ng new app-test --skip-git	    | ng new app-test -sg       |
| * Não efetua o primeiro commit no repósitorio local         | ng new app-test --skip-comit	  | ng neww app-test -sc    |	
| * Tipo de extensão dos arquivos de estilo, padrão "css"     | ng new app-test --style scss    |                           |	 	 
| * Prefixo que será utilizado por todos os componentes       | ng new app-test --prefix ekz	  | ng new app-test -p ekz  |
| * Define o diretório onde a aplicação será criada           | ng new app-test --directory	    | ng new app-test -dir	    |
| * Define o subdiretório onde serão criados os códigos fonte |--source-dir	   	                | ng new app-test -sd       |
|                                                                                                                           |
| * Não são frequentementes utilizados                                                                                      |

### Nível de aplicação
    
| O que faz                                         | Comando                                 | Atalho                  |
| -                                                 |-                                        | -                       |
| Cria um componente                                | ng generate component test-component    | ng g c test-component   |
| Cria um novo módulo                               | ng generate module test-module          | ng g m test-module      |
| Cria um novo serviço                              | ng generate service test-service        | ng g s test-service     |
| Cria uma novo pipe                                | ng generate pipe test-pipe              | ng g p test-pipe        |
| Cria uma nova diretiva                            | ng generate directive test-directive    | ng g d test-directive   |
| Cria uma nova classe utilitária (Arquivo TS) *    | ng generate class                       | ng g cl test-class      |
| Cria um novo enum *                               | ng generate enum test-enum              | ng g p test-enum        |
| Cria um novo interface *                          | ng generate interface test-interface    | ng g i test-interface   |
| Cria um novo guard (Arquivo de guard de rota) *   | ng generate guard test-guard            | ng g g test-guard       |
|                                                                                                                       |
| * Não são frequentementes utilizados                                                                                  |

### Nível de build

| O que faz                         | Comando         | Atalho        |
| -                                 |-                | -             |
| Gera um build de produção *       | ng build --prod | ng b --prod   |
| Gera um build em desenvolvimento  | ng build --dev  | ng b --dev    |
|                                                                     |
| * ng build --prod equivale a ng build --aot (ahead of time)         |
| [lista completa de comandos builds](https://angular.dev/cli/build)  |
