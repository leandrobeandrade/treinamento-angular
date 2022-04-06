## Interface Model - Estrutura

Interfaces funcionam como um modelo de dados, são abstrações de entidades do banco de dados que servem como um contrato para realizarem a comunicação dos dados entre o cliente e o servidor. Garantindo que os dados que estão sendo enviados contemplem esse modelo.

### Descrição

Podemos utilizar como base de um modelo tanto interfaces como classes, para descrever os campos necesários das entidades.

> **Observação:** Classes podem ser instânciadas ao contrário de interfaces, o que facilita na utilização do modelo, sendo mais seguras e mais práticas na sua utilização

### Modelo de dados Exemplo

Dado uma entidade que represente um usuário em um sistema com os seguintes atributos:

|Propriedade|Valor            |
|-          |-                |
|id         |INT, PRIMARY, AI |
|name       |VARCHAR, 30      |
|registered |DATETIME         |
|password   |VARCHAR, 16      |
|email      |VARCHAR, 50      |
|active     |BOOL             |

Podemos descrever um modelo para ser utilizado como contrato da seguinte maneira:

    export class IUserModel {
      id: number;
      name: string;
      registered: Date
      password: string;
      email: string;
      active: boolean;
    }
    
Pode ser utilizado a funcionalidade **type union** forncedida pelo `TypeScript` para se tipar os atributos com determinados tipos. 

    id: number | string | null`**, 
    
Assim também como utilizar outras classes para se tipar um atributo, o que é muito comumumente utilizado.

    export class IUserModel {
      id: number;
      name: string;
      registered: Date
      password: string;
      email: string;
      active: boolean;
      company: IUserCompanyModel;
    }
    
    export class IUserCompanyModel {
      companyContractor: ICompanyModel;
      companyView: ICompanyModel;
    }

### Utilização do modelo

Para a utilização do modelo criado basta fazer a importação devida do mesmo na classe utilitária, atribuindo o modelo a uma propriedade da classe e instânciando este modelo para uso:

    import { IUserModel } from 'path-do-arquivo';
    import { IUserCompanyModel } from 'path-do-arquivo';

    export class User {
      userModel: IUserModel;
      
      ngOnInit() {
        this.userModel = new UserModel();
        this.userModel.company = new IUserCompanyModel();
      }
    }

Após a instanciação do modelo este fica disponível na classe utilitária para uso, ou seja, todos os atributos declarados no modelo ficam disponíveis:

    createUser() {
      this.userService.addUser(this.userModel);             // this.userModel será preenchido em um formulário
    }
    
    updateUser() {
      this.userService.deleteUser(this.userModel.id);      // this.userModel.id - utilização de propriedades do modelo
    }



