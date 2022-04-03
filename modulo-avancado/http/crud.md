## CRUD

Para integrações *`cliente/servidor`* utilizamos a implementação **CRUD (create - read - update - delete)** com a especificação **LAFUD**:

- **L**ist (R - read) - Realiza a listagem dos dados retornados pela API, geralmente utilizados em tabelas
- **A**ddition (C - create) - Realiza a adição/criação de um novo registro no banco
- **F**ind (read) - Realiza a listagem de um registro em específico no banco, geralmente através de um Id único
- **U**pdate (U - update) - Realiza a atualização de um registro específico no banco
- **D**elete - (D - delete) - Realiza a remoção de um registro no banco

> Exemplo de uso

    import { IUserModel, IUserService } from '@core/data';
    import { ApiServiceOld } from '@core/utils';
    import { IResponseOld } from '../helpers';
    
    @Injectable()
    export class UserService extends IUserService {
      readonly url = 'v1/generals/users';

      constructor(private apiService: ApiServiceOld) {
        super();
      }

      listUsers(): Observable<IResponseOld<IUserModel>> {
        return this.apiService
          .crudeList({
            url: this.url,
          })
          .pipe(map((res) => new IResponseOld<IUserModel>(res, IUserModel)));
      }

      addUser(categories: IUserModel): Observable<IResponseOld<IUserModel>> {
        return this.apiService
          .crudeAdd({
            url: this.url,
            payload: categories.toJSON(),
          })
          .pipe(map((res) => new IResponseOld<IUserModel>(res, IUserModel)));
      }

      findCurrentUser(id: string | number): Observable<IResponseOld<IUserModel>> {
        return this.apiService
          .crudeFind({
            url: this.url,
            id: id.toString(),
          })
          .pipe(map((res) => new IResponseOld<IUserModel>(res, IUserModel)));
      }

      updateUser(categories: IUserModel): Observable<IResponseOld<IUserModel>> {
        return this.apiService
          .crudeUpdate({
            url: this.url,
            id: categories.id.toString(),
            payload: categories.toJSON(),
          })
          .pipe(map((res) => new IResponseOld<IUserModel>(res, IUserModel)));
      }

      deleteUser(id: string | number): Observable<any> {
        return this.apiService.delete({
          url: this.url,
          id: id.toString(),
        });
      }
    }
 
 ### Outros casos
 
Existe necessidades específicas em partes da aplicação que faz-se necessário a utilização de configuração extras nas chamadas as API's como configuração da rota, passagem de queryParams entre outros.
 
> Configuração de rotas

Acrescenta recursos em qualquer parte da url, exemplo que faz a listagem de itens raízes

    url = 'v1/generals/items';
    
    listItems(): Observable<IResponseOld<IItemsModel>> {
      return this.apiService
        .crudeList({
          url: `${this.url}/roots}`,
        })
        .pipe(map((res) => new IResponseOld<IItemsModel>(res, IItemsModel)));
    }
    
> QueryParams

Exemplo de service onde é passado um array de id's para que sejam listados em específico

    listItemsByWarehouse(id: number | number[]): Observable<IResponseOld<IItemsByWarehouseModel>> {
      return this.apiService
        .crudeList({
          url: this.url,
          queryParams: {
            category: ApiServiceOld.mapToQueryParam(id)
          }
        })
        .pipe(map((res) => new IResponseOld<IItemsByWarehouseModel>(res, IItemsByWarehouseModel)));
    }



