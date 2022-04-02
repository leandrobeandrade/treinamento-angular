## Tipagem de Models

Tendo os modelos criados conforme um entidade referente a uma tabela em um banco de dados, podemos utilizá-las para a tipagem de atributos, métodos e tudo mais quanto for necessário que precise fazer menção a um modelo em específico.

> Exemplo de uso

### Arquivo model com classe espelho do modelo e classe abstrata para o service

    // Classe Modelo
    export class IUserModel {
      id: number;
      name: string;
      role: string;
      email: string;
    }
    
    // Classe Service abstrata com métodos abstratos
    export abstract class IUserService {
      abstract listUsers(): Observable<IUserModel[]>;
      abstract addUser(user: IUserModel): Observable<IUserModel>;
      abstract findUser(id: number | string): Observable<IUserModel>;
      abstract updateUser(userId: number | string, user: IUserModel): Observable<IUserModel>;
      abstract deleteUser(userId: number | string): Observable<IUserModel>
    }

### Arquivo serviço que faz a integração com a API e estende e implementa a classe abstrata

    import { IUserModel, IUserService } from '../model/user.model.ts';

    @Injectable()
    export class UserService extends IUserService {
      url = 'endereco/api/user';

      constructor(private http: HttpClient) {
        super();
      }

      listUsers(): Observable<IUserModel[]> {
        return this.http.get<IUserModel[]>(this.url);
      }

      addUser(user: IUserModel): Observable<IUserModel> {
        return this.http.post<IUserModel>(this.url, post);
      }

      findUser(id: number | string): Observable<IUserModel> {
        return this.http.get<IUserModel>(`${this.url}/${id}`);
      }

      updateUser(id: number | string, post: IUserModel): Observable<IUserModel> {
        return this.http.put<IUserModel>(`${this.url}/${id}`, post);
      }

      deleteUser(id: number | string): Observable<IUserModel> {
        return this.http.delete<IUserModel>(`${this.url}/${id}`);
      }
    }
    
### Arquivo com classe utilitária que utiliza o serviço

    ...
    import { IUserModel, IUserService } from '../model/user.model.ts';
    
    @Component({
      selector: 'app-user',
      templateUrl: './user.component.html',
      styleUrls: ['./user.component.scss'],
    })
    export class UserComponent implements OnInit {
      users: IUserModel[] = [];
      userModel: IUserModel;
    
      constructor(private userService: IUserService)
    
      ngOnInit() {
        this.userModel = new IUserModel();
      
        this.loadUsers();
      }
      
      loadUsers() {
        this.userService.listUsers().subscribe((users) => {
          this.users = users as IUserModel[];
        });
      }

      createUser() {
        this.userService.addUser(this.userModel);
      }

      editUser() {
        this.userService.updateUser(this.userModel.id, this.userModel);
      }

      deleteUser() {
        this.userService.deleteUser(this.userModel.id);
      }
      
      findUser() {
        this.userService.findUser(this.userModel.id).subscribe((user: IUserModel) => {
          this.userModel = this.user as IUserModel;
        })
      }
    }
