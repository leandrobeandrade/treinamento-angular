## Componentes Internos

Alguns componentes foram desenvolvidas de uma forma genérica para que podessem serem utilizados pela aplicação em geral

### Componentes

- **Componente Arquivo:** Envio e visualização de arquivos
- **Componente Content Header:** Define cabeçalhos de navegação
- **Componente Imagem:** Envio e visualização de imagens
- **Componente QrCode:** Gera um código QR Code
- **Componente Smart Select:** Gerencia um controle com menu de opções para o usuário
- **Componente Smart Multi Select:** Gerencia um controle com menu de várias opções para o usuário

---

#### Componente Arquivo

Documentação do componente utilizado para envios de arquivos pela aplicação.

Para construção deste componente foi utilizado o plugin [ngx-awesome-uploader](https://www.npmjs.com/package/ngx-awesome-uploader)

O componente se comporta com um botão, para quando clicado abra o menu de contexto do sistema operacional para escolha do(s) arquivo(s).

![image](https://user-images.githubusercontent.com/24658433/156226487-33f82e4d-1d78-4425-96e6-adbec6d47fe0.png)

#### Exemplo de uso

    <app-file-uploader
       [isImage]="false"                                 1
       [isMultiple]="false"                              2
       (handleFileId)="addFiles($event)"                 3
       (click)="findFiles(params)"                       4 A*
    ></app-file-uploader>

#### Propriedades

**1. isImage** - Define se o arquivo é uma imagem ou outro arquivo | `true/false`

**2. multiple** - Define se poderá ser enviados um ou várias arquivos | `single/multi`

**3. handleImageId** - Método para armazenar os arquivos | `função`

**4. (evento)** - Obtém pela API os arquivos enviados | `função`

> **A** - *Exemplo de função para mostrar os documentos enviados e deletar arquivos enviados*

![image](https://user-images.githubusercontent.com/24658433/156226751-8dc0afed-e646-4c0f-a9c0-fd450bf0413f.png)

    model: any;
    files = [];   // Armazena os arquivos

    constructor(
       private fileManagerService: FileManagerService,
       private awesomeFileUploaderService: AwesomeFileUploaderService
    ) {}

    ngOnInit() {
       this.listfiles(this.model.files)
    }

    listFiles(files_: IFiles[]) {   // Preenche o model de arquivos
       const filesArray = [];

       if (files_.length > 0) {
          files_.forEach((files) => {
             this.fileManagerService
                .findFileImageById(files.uuid)
                .pipe(first())
                .subscribe((fileRet) => {
                   const file = fileRet.data as IFileManagerModel;
                   filesArray.push({
                      uuid: files.uuid,
                      originalName: file.originalName,
                   })
                });
          });
       }
       return filesArray;
    }

    addFiles(file) {    // Adiciona arquivos
       let filesId = [];

       if (file) {
          filesId.push({
             uuid: file.uuid,
             originalName: file.name,
          });
       }

       filesId.forEach((values) => {
          this.files.push(values);
       });
    }

    findFiles(params) {
       this.files = params;    // Obtém os arquivos enviados da API
    }

    showFilesModal(dialog: TemplateRef<any>) {   // Abre o modal de visualização dos arquivos
       this.dialogService
          .open(dialog, {
             context: {
                title: 'Lista de Arquivos Adicionados',
             },
             closeOnBackdropClick: false,
          })
          .onClose.pipe(
             filter((ev) => ev === true),
             takeWhile(() => this.alive)
          )
          .subscribe();
    }

    removeFiles(fileName: string, uuid: string) {     // Remove arquivo
       let remainFiles = [];

       this.awesomeFileUploaderService.softDeleteFile(uuid).subscribe(() => {
          remainFiles = files.value.filter((item) => {
             return item.originalName !== fileName;
          });

          remainFiles.forEach((values) => {
             this.files.push(values);
          });
       });
    }

---

#### Componente Content Header

Documentação do componente utilizado para renderizar botões de navegações entre páginas. Geralmente este componente é inserido dentro de um `<nb-card-header>`

![image](https://user-images.githubusercontent.com/24658433/156227721-0e1d2ad9-97c7-4305-a0bb-adc43ec894aa.png)

> Todas as propriedades são opcionais

#### Exemplo de uso

    <app-content-header
       [title]="titleCard"                             1
       [description]="'Adicionar e excluir ...'"       2
       [menu]="menu"                                   3 A*
       (itemClick)="openCategories()"                  4
       icon="fas fa-object-group"                      5
       badge="warning"                                 6
       badgeStyle="scheduledTrainings"                 7
    ></app-content-header>

#### Propriedades

**1. title** - Define o texto do card (da página atual) | `text`

**2. description** - Define o texto de descrição referente a funcionalidade da página | `text`

**3. menu** - Monta os botões baseado na interface ContentHeaderMenu[] | `array`

**4. (itemClick)** - Evento que dispara uma método para uma ação | `função`

**5. icon** - Ícone referente a ferramenta da página atual | `string`

**6. badge** - Recebe desde array até componente | `any`

**7. badgeStyle** - classe CSS do badge | `string (warning/danger/etc...)`

> **A** - ContentHeaderMenu: interface responsável pelas propriedades dos botões

| Propriedade | Tipo                 | Obrigatório |
| ------      | ------               | ----------  |
| icon        | string               | não         |
| name        | string               | sim         |
| id          | string               | não         |
| to          | string               | não         |
| description | string               | não         |
| badge       | string/number        | não         |
| badgeStyle  | string               | não         |
| queryParams | { [k: string]: any } | não         |
| type        | 'link'/'button'      | não         |

---

#### Componente Imagem

Documentação do componente utilizado para envios de imagens pela aplicação.

Para construção deste componente foi utilizado o plugin [ngx-awesome-uploader](https://www.npmjs.com/package/ngx-awesome-uploader)

![image](https://user-images.githubusercontent.com/24658433/156228053-5889b01d-e421-43df-94b2-4793d23abf5f.png)

#### Exemplo de uso

    <app-awesome-file-uploader
       [isImage]="true"                                1
       multiple="single"                               2
       [cropper]="false"                               3
       [fileId]="image.id"                             4
       [editMode]="false"                              5
       (handleImageId)="getImageId($event)"            6 A*
       height="200px"                                  7 B*
    >
    </app-awesome-file-uploader>

#### Propriedades

**1. isImage** - Define se o arquivo é uma imagem ou outro arquivo | `true/false`

**2. multiple** - Define se poderá ser enviados uma ou várias imagens | `single/multi`

**3. cropper** - Define se a imagem poderá ser cortada | `true/false`

**4. fileId** - Define o id (model) da imagem | `string`

**5. editMode** - Define o componente como apenas um visualizador da imagem | `true/false`

**6. handleImageId** - Método para preencher o model | `função`

**7. height** - Define um tamanho específico para o componente | `string`

> **A** - *Exemplo de função para preencher o model com id da imagem recebido da API*

    image: IFileModel = new IFileModel();

    getImageId(fileId: string) {
       this.image.id = fileId;
    }

> **B** - *A altura do componente recebe um `string`, logo, pode receber valores em **px, pt, vh etc...***

#### Sem Imagem

Por padrão se um **`id`** da imagem não for mais encontrada pela API, o componente exibe um template padrão com a inscrição sem imagem

![image](https://user-images.githubusercontent.com/24658433/156228177-638e4a50-d891-4736-a496-f6ccd1853e6d.png)

#### Imagem Placeholder

Também existe a possibilidade de se exibir um template no componente que está utilizando o componente de imagem com uma imagem placeholder já contida na aplicação:

image: IFileModel = new IFileModel();
noImage = '../../../../../../assets/images/not_found.png';

    <div *ngIf="this.image.id !== undefined; else noItemImage">
       <app-awesome-file-uploader
           [isImage]="true"
           ...
       ></app-awesome-file-uploader>
    </div>
    <ng-template #noItemImage>
       <div class="text-center mt-4">
          <img width="340" height="250" [src]="noImage" alt="No Image">
       </div>
    </ng-template>

![image](https://user-images.githubusercontent.com/24658433/156228228-474f2eed-06a4-4775-86d4-0a7702ff7a8b.png)

---

#### Componente QR Code

Documentação do componente responsável por gerar e renderizar um QR Code relativo a determinado dado.

Para construção deste componente foi utilizado o plugin [angularx-qrcode](https://www.npmjs.com/package/angularx-qrcode)

![image](https://user-images.githubusercontent.com/24658433/156228579-110a2470-dbb9-46cc-a4a0-2c149c0a20a6.png)

### Exemplo de uso

> Apenas nas configurações das Tabelas

    qrcode: {
        title: 'QRCode',
        filter: false,
        type: 'custom',
        renderComponent: QrcodeComponent,                                1
        onComponentInitFunction: (instance: QrcodeComponent) => {        2
            instance.setQrCodeLegend(['asset', 'sector']);               3
            instance.setQrCodeViewerList('Ativo', [                      4
                { key: 'asset', title: 'Ativo' },                        5
                { key: 'sector', title: 'Setor' },
            ]);
            instance.beforeShowQrCode.subscribe(() =>                    6 A*
                instance.setQrCodeData(String(instance.rowData?.id))
            );
        },
    },

#### Propriedades

**1. renderComponent** - Declara na tabela o componente Qrcode para utilização 

**2. onComponentInitFunction** - Função executada na inicialização do componente com uma instância Qrcode

**3. setQrCodeLegend** - Chaves de campos na tabela que terão valores mostrados no modal

**4. setQrCodeViewerList** - Define o título do modal, e os campos relativos as chaves setadas anteriormente

**5. editMode** - Campos renderizados como informação no modal do tipo chave do campo - título do campo

**6. beforeShowQrCode** - Método que seta o que será renderizado no código QR Code

> **A** - *Por padrão está sendo apenas passado o Id daquele respectivo elemento*. Ex: `id de um ativo`, `item`, etc...

---

#### Componente Smart Select

Componente responsável por gerenciar opções de entrada para o usuário.

![image](https://user-images.githubusercontent.com/24658433/156229242-956c3360-a3bc-4930-84f7-1e96e349d34c.png)

> Obrigatório ter a propriedade HTML **`name`** para o correto funcionamento

#### Exemplo de uso

<app-smart-select
  label="Ativo"                                  1
  name="Ativo"                                   2
  [(ngModel)]="layoutId"                         3
  [options]="(layouttModels$ | async)"           4 A*
  descriptor="getLayoutsDescription"             5 B*
  (ngModelChange)="onChangeLayout($event)"       6 C*
  [required]="true"                              7
></app-smart-select>

#### Propriedades

**1. label** - Define o label do select | `text`

**2. name** - Define um name para o select | `text`

**3. ngModel** - Define o id do elemento selecionado | `text/propriedade da classe`

**4. options** - Define um array com os dados a serem renderizados | `array/array observável`

**5. descriptor** - Define o campo a ser renderizado | `string/função`

**6. ngModelChange** - Evento que dispara uma método para obter a opção selecionada | `função`

**7. required** - Define se o select irá ser obrigatório | `string/boolean`

---

> **A** - A propriedade **`options`** pode receber um array com os dados ou um observável com os dados

    // Array comum (Dados Mockados)
    [options]="layouts"

    layouts = ['Ativo 1', 'ativo 2', 'Ativo 3'];

    // Array com dados da API    
    [options]="layouts"

    layouts: IAssetModel[] = [];     // Array do tipo ILayouttModel

    this.layoutService.listLayouts().subscribe((layouts) => {
      this.layouts = layouts.dataArray as ILayoutModel[];    // Preenche o array com os dados da API
    });

    // Array com dados da API resolvido no Template (HTML)
    [options]="(layouts$ | async)"

    layouts$: Observable<IResponseOld<ILayoutModel>>;
    
    this.layouts$ = this.layoutService.listLayouts();     // guarda o resultado na variável

> **B** - Uma string ou uma função que define a propriedade a ser renderizada, geralmente `description`

    // Forma texto - acesso direto
    descriptor="description"

    // Forma texto - acesso aninhado
    [descriptor]="'layout.description'"

    // Forma função
    [descriptior]="getLayoutsDescription"

    getDescriptionAssets(asset: IAssetModel) {
      return `${asset?.assetDetail.description}`;
    }

> **C** - Geralmente utilizado como um filtro quando um componente de seleção depende de outro para obter os dados

    // Através da Área selecionada preenche o select de Setor
    <app-smart-select
       label="Área"
       name="area"
       [(ngModel)]="areaId"
       (ngModelChange)="onAreaChange()"
       [options]="areas"
       descriptor="description"
       [required]="true"
     ></app-smart-select>

    <app-smart-select
       label="Setor"
       name="sector"
       [(ngModel)]="sectorId"
       [options]="sectors"
       descriptor="description"
       [required]="true"
     ></app-smart-select>

     areas: ILayoutModel[] = [];
     sectors: ILayoutModel[] = [];

     this.layoutService.listLayouts(LayoutCategoryEnum.AREA).subscribe((areas) => {
      this.areas = [...areas.dataArray];    // Outra forma de se preencher o array
    })

     onAreaChange() {
      this.layoutService.listLayouts(areaId).subscribe((sector) => {
        this.sectors = sector.data as ILayoutModel[];
      })
    }

---

#### Componente Smart Multi Select

Componente responsável por gerenciar várias opções de entradas para usuário. 

![image](https://user-images.githubusercontent.com/24658433/156228877-da5ff832-9d03-4ed8-9a5b-4ddcaa1a4595.png)

> Obrigatório ter a propriedade HTML `name` para o correto funcionamento

#### Exemplo de uso

    <app-smart-multi-select
       label="Categorias"                                  1
       name="categories"                                   2
       [(ngModel)]="selectedCategories"                    3
       [options]="(categories$ | async)"                   4 A*
       [descriptor]="getLayoutsDescription"                5 B*
       [required]="true"                                   6
    ></app-smart-multi-select>

#### Propriedades

**1. label** - Define o label do select | `text`

**2. name** - Define um name para o select | `text`

**3. ngModel** - Define o id do elemento selecionado | `text/propriedade da classe`

**4. options** - Define um array com os dados a serem renderizados | `array/array observável`

**5. descriptor** - Define o campo a ser renderizado | `string/função`

**6. required** - Ícone referente a ferramenta da página atual | `string/boolean`

> **A** - A propriedade `options` pode receber um array com os dados ou um observável com os dados

    <app-smart-multi-select
       name="categories"
       label="{{ 'items.labels.categories' | translate }}"
       [options]="(categories$ | async)"
       [(ngModel)]="selectedCategories"
       [descriptor]="getCategoryDescriptor"
    ></app-smart-multi-select>

    categories$: Observable<IResponseOld<IItemCategoriesModel>>;

    this.categories$ = this.itemCategoriesService.listItemCategories();

> **B** - Uma string ou uma função que define a propriedade a ser renderizada, geralmente `description`

    getCategoryDescriptor(category: IItemCategoriesModel): string {
      return `${category.description}`;
    }
    
