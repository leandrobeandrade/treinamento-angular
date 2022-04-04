## Serviços

Serviços em aplicações Angular servem para gerenciamento e controle de dados dos mais diversos tipos, estruturas e modelos, sendo estes dados internos ou externos. Pode ter estes compartilhados em uma intância apenas `(padrão singleton)` ou ter várias instâncias sobre o mesmo serviço.

### Dados Internos

Serve como um fornecedor e compartilhador de dados internos da aplicação para os mais diversos fins, como verificação de valores atualizados de propriedades e validações.

#### Exemplo de uso

    // Classe Service
    import { Injectable } from '@angular/core';
    import { BehaviorSubject } from 'rxjs';

    @Injectable({
      providedIn: 'root',
    })
    export class DadosInternosService {
      readonly control: BehaviorSubject<boolean> = new BehaviorSubject(false);  // valor iniciado como false
      public readonly control$ = this.control.asObservable();

      constructor() {}

      setControlStatus(val: boolean) {
        this.control.next(val);
      }
    }
    
    // Classe que seta um valor para ser atualizado
    constructor(private dadosInternosService: DadosInternosService) {}
    
    setControl() {
      this.dadosInternosService.setControlStatus(true);   // seta o valor para true
    }
    
    // Classe que recebe o dado atualizado
    @Component({
      selector: 'servico-dados-internos',
      template: `
        <p>Valor Atualizado: <b>{{ value }}</b></p>     // true
      `,
    })
    export class ServicoDadosInternosComponent implements OnInit {
      value: boolean;

      constructor(private dadosInternosService: DadosInternosService) {}

      ngOnInit() {
        this.dadosInternosService.control$.subscribe((value) => {
          this.value = value;
        });
      }
    }

### Dados Externos

Serve como uma ponte entre a aplicação e e uma fonte de dados externa a aplicação, podendo receber, cadastrar, atualizar e deletar este dados através dos métodos disponibilizados pelo módulo `HTTPClientModule`.

#### Exemplo de uso

    // Classe Serviço
    import { Injectable } from '@angular/core';
    import { HttpClient, HttpResponse } from '@angular/common/http';
    import { Observable } from 'rxjs';

    @Injectable({
      providedIn: 'root',
    })
    export class DadosExternosService {
      readonly url = 'https://opentdb.com/api.php?amount=10';

      constructor(private http: HttpClient) {}

      getData(): Observable<any> {
        return this.http.get<HttpResponse<any>>(this.url);
      }
    }
    
    // Classe de Funcionalidade
    @Component({
      selector: 'servico-dados-externos',
      template: `
        <h3>Comunicação de dados externos entre Componentes através de Serviço</h3>
        <div class="question" *ngFor="let q of data?.results; let i = index">
          <h4>Questão {{ i + 1 }} [Type - {{ q.type }}]</h4>
          <h5>{{ q.question }}</h5>
          <p>{{ q.category }}</p>
          <span>Dificuldade : {{ q.difficulty }}</span>
        </div>
      `,
      providers: [DadosExternosService],
    })
    export class ServicoDadosExternosComponent implements OnInit {
      public data: any;

      constructor(private dadosInternosService: DadosExternosService) {}

      ngOnInit() {
        this.dadosInternosService.getData().subscribe((res) => {
          this.data = res;
          console.log(this.data);
        });
      }
    }
    
|Referências|
|-|

- [httpClient](https://angular.io/api/common/http/HttpClient)
- [métodos http](https://angular.io/guide/http)
