## Comandos NPM / YARN

| O que faz                                   | Comando NPM                   | Comando YARN                |
| -                                           |-                              | -                           |
| Instala dependências necessárias no projeto | npm install / npm i           | yarn                        |
| Executa o projeto                           | npm run                       | yarn run                    |
| Cria um arquivo package.json.               | npm init                      | yarn init                   |
| Instala uma dependência                     | npm install bootstrap         | yarn add bootstrap          |
| Atualiza uma dependência                    | npm install bootstrap@latest  | yarn up bootstrap --latest  |
| Atualiza uma dependência                    | npm install bootstrap@4.6.0   | yarn up bootstrap@4.6.0     |
| Desinstala uma dependência                  | npm uninstall bootstrap       | yarn remove bootstrap       |

## Comandos npm install / npm ci

| O que faz                   | Comando NPM install                  | Comando NPM ci                   |
| -                           |-                                     | -                                |
| Fonte de instalação         | Usa package.json e package-lock.json | Somente package-lock.json        |
| Modifica arquivos           | Sim, package-lock.json               | Não modifica                     |
| Atualiza node_modules       | Mantém ou atualiza                   | Apaga tudo e reinstala           |
| Velocidade                  | Mais lento porém mais flexível       | Mais rápido e direto             |
| Consistência entre máquinas | Pode variar de máquina pra máquina   | Sempre igual, 100% reprodutível  |
| Falha caso divergências     | Não falha                            | Falha se arquivos divergirem     |

> Casos para utilizar npm ci
- Para aplicações Angular de alto desempenho.
- Angular é muito sensível à compatibilidade entre versões de dependências, como @angular/core, rxjs, e zone.js.
- Usar npm ci em ambientes de build evita bugs difíceis de rastrear causados por diferenças de versão.
- Garante que o build de produção é exatamente o mesmo do ambiente de desenvolvimento homologado.