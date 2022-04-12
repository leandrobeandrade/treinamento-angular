## Exemplos Aplicados

Exemplos de uso na aplicação **ekaizen-frontend-redo** referentes a utilização da biblioteca `RXJS` con suas funcionalidades e seus operadores.

> ### forkJoin

    1 - Variável com a referência do service que lista setores
    2 - Variável com a referência do service que lista os treinamentos
    3 - Cria um obersável forkJoin recebendo os 2 services
    4 - Canaliza o fluxo dos dados
    5 - Mapeia cada valor de retorno dos services
    6 - Inscreve no observável para pegar os dados modelados
>

    1  const layouts$ = this.layoutService.listLayoutSectors();
    2  const selectedLayouts$ = this.ghcLayoutsToTrainingsService.listGhcLayoutsToTrainings(layoutId);

    3  forkJoin([layouts$, selectedLayouts$])
    4   .pipe(
    5     map(([layouts, selecteds]) = <[IResponseOld<IGhcLayoutSectorsModel>, IResponseOld<IGhcLayoutsToTrainings>]>[
             layouts.map((layout) => ({
               id: safe('id', layout),
               layout: { id: layout.id },
               description: safe('description', layout),
               descriptionCode: safe('descriptionCode', layout),
               category: safe('description', layout.category),
               isRequired: selecteds.dataArray.some((selected) =>selected.layout.id === layout.id && selected.isRequired),
             })),
             selecteds.map((selected) => ({
               layout: { id: selected.layout.id },
               isRequired: selected.isRequired,
               description: layouts.mapped.find((layout) => layout.id === selected.layout.id).description,
               descriptionCode: layouts.mapped.find((layout) => layout.id === selected.layout.id).descriptionCode,
               category: layouts.mapped.find((layout) => layout.id === selected.layout.id).category,
             })),
           ]
         )
       )
    6  .subscribe(([layouts, selectedLayouts]) => {
        this.layouts = layouts;
        this.selectedLayouts = selectedLayouts.mapped;
      });
      
> ### takeWhile e filter

    1 - Variável de controle;
    2 - Se o evento for true segue o fluxo
    3 - Observa enquanto a vriável de controle for true
    4 - Muda o valor da varável de controle para false se desinscrevendo do observable
>
    1   alive = true;
    
        openDialog(row: IShiftsModel) {
          this.dialogService
            .open(ShiftsFormComponent, {
              context: { row },
              closeOnBackdropClick: false,
            })
            .onClose.pipe(
    2         filter((event) => event === true),
    3         takeWhile(() => this.alive)
            )
            .subscribe(() => {
              this.loadShifts();
            });
        }

        ngOnDestroy() {
    4     this.alive = false;
        }

> ### mergeMap

    1 - Service que lista o status atual
    2 - Mergea o retorno do primeiro service de listagem com o retorno do segundo de update

>

        changeStatus() {
    1     this.emotionalKanbanStatus.getCurrentStatus()
            .pipe(
    2         mergeMap(() =>
                this.emotionalKanbanStatus.updateStatus(
                  this.emotionalKanbanStatusModel
                )
              ),
              catchError(() =>
                this.emotionalKanbanStatus.addStatus(this.emotionalKanbanStatusModel)
              )
            )
            .subscribe();
        }
