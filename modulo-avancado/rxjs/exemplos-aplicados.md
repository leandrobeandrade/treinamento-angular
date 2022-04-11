## Exemplos Aplicados

Exemplos de uso na aplicação **ekaizen-frontend-redo** referentes a utilização da biblioteca `RXJS` con suas funcionalidades e seus operadores.

### `forkJoin`

    const layouts$ = this.layoutService.listLayoutSectors();
    const selectedLayouts$ = this.ghcLayoutsToTrainingsService.listGhcLayoutsToTrainings(layoutId);

    forkJoin([layouts$, selectedLayouts$])
      .pipe(
        map(([layouts, selecteds]) = <[IResponseOld<IGhcLayoutSectorsModel>, IResponseOld<IGhcLayoutsToTrainings>]>[
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
      .subscribe(([layouts, selectedLayouts]) => {
        this.layouts = layouts;
        this.selectedLayouts = selectedLayouts.mapped;
      });
