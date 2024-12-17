# Sistema de Checkout - AngularJS

Este projeto é um sistema de **checkout de produtos** utilizando **AngularJS (1.x)**, implementando um fluxo completo de compras com funcionalidades como ofertas, carrinho, cupons de desconto, validações de campos, página de agradecimento e integração com chat.

## Funcionalidades

### 1. **Página de Ofertas**
- **Exibição de Detalhes do Produto**: A página exibe informações dinâmicas sobre os produtos a partir de um objeto `shop.offers`.
- **Destaques e Descontos**: Oferece a possibilidade de exibir o nome do produto, descontos e preços com a formatação correta de moeda brasileira (R$).
- Exibe uma lista de produtos em promoção ou com desconto.
- Permite ao usuário adicionar produtos ao carrinho diretamente da página de ofertas.

### 2. **Carrinho de Compras**
- **Interação com o Carrinho**: Ao clicar no botão de adicionar ao carrinho, o usuário é redirecionado para a página do carrinho. O clique também aciona um evento de rastreamento personalizado para o Facebook Pixel.
- O usuário pode visualizar os produtos que adicionou ao carrinho, com as opções de **remover** ou **alterar quantidade**.
- Exibe o **valor total** da compra, incluindo qualquer desconto aplicado.

### 3. **Cupons de Desconto**
- Permite que o usuário insira cupons de desconto para aplicar um valor ou percentual de desconto no total da compra.
- Realiza validações para garantir que o cupom seja válido.

### 4. **Checkout**
- Formulário completo para coletar os dados do usuário, incluindo:
- **Formatação Dinâmica**: A interface ajusta dinamicamente os valores e conteúdos, como o preço, desconto e parcelamento, usando as variáveis do AngularJS.
  - **Nome**, **E-mail**, **Telefone**, **CPF**, **Data de Nascimento**, **Endereço** e **Sexo**.
  - **Validações de dados** em tempo real, como campos obrigatórios, formato de e-mail, CPF, telefone e CEP.
  - **Máscaras de entrada** para CPF, telefone, data de nascimento e CEP.
  - **Preenchimento automático de endereço** baseado no **CEP** informado.

  
### 5. **Página de Obrigado**
- Após o envio do formulário de checkout, o usuário é redirecionado para uma página de **agradecimento** com a confirmação do pedido.

### 6. **Chat**
- Integração com uma funcionalidade de chat, permitindo que o usuário entre em contato com o suporte ao cliente durante o processo de compra.

### 7. **Validações**
- **Validação de campos obrigatórios**: Todos os campos essenciais como nome, e-mail, telefone, CPF, endereço, etc.
- **Validação de CPF, e-mail e telefone**: Verificação de formatos corretos usando expressões regulares.
- **Validação de CEP**: Busca automática de endereço com base no CEP.
- **Validação em tempo real**: Mensagens de erro aparecem enquanto o usuário interage com o formulário.


## Tecnologias Utilizadas

- **AngularJS (1.x)**: Framework utilizado para criar a estrutura interativa do sistema.
- **Angular Forms (ngModel e ngMessages)**: Utilizado para controlar a validação e o estado dos campos do formulário.
- **Text Mask**: Para mascarar campos como CPF, telefone, CEP, e data de nascimento.
- **Bootstrap**: Framework CSS utilizado para criar um layout responsivo e amigável.
- **jQuery**: Para algumas interações e manipulação de DOM.
- **Chat Integration**: Biblioteca para integração com um sistema de chat (pode ser um serviço de terceiros como Intercom ou Tawk.to).
- **Interpolação de Dados**: Utilizando as expressões `{{ }}` para mostrar dinamicamente os valores de um modelo Angular.
- **Diretivas**: Usando `ng-click` para tratar eventos de clique.
- **Filtros**: Usando o filtro `currency` para formatar os valores como moeda em Reais.
- **Binding de Imagens**: Utilizando a diretiva `src` para carregar dinamicamente as imagens dos produtos a partir de URLs externas.
## Tecnologias

## Exemplo de Código

```html
<div id="produtos" class="clearfix fundo_h2">
  <a class="destaque">
    <h3>{{shop.offers[2]?.freeField1}}</h3>
  </a>
  <h2 class="info">{{shop.offers[2]?.kit.name}}</h2>
  <div class="desconto">
    Desconto<br>
    <strong>{{shop.offers[2]?.freeField2}}</strong>
  </div>
  <div class="cont_left">
    <div class="recomendado">{{shop.offers[2]?.option}}</div>
    <img [src]="shop.offers[2]?.kit.photoUrl" alt="" border="0">
  </div>
  <div class="cont_direita">
    <p class="preco_destaque">
      <span>{{shop.offers[2]?.installment}}x</span> 
      {{shop.offers[2]?.price | currency:'BRL':true}}
    </p>
    <form (click)="goToCart(shop.offers[2])" (ng-click)="fbq('trackCustom', 'SacheOferta6')">
      <h4>Apenas R${{shop.offers[2]?.freeField3}} por dia!</h4>
      <button>  
        <img width="100%" src="images/bt_adicionar.jpg" alt="">
      </button>
      <p class="preco_bottom">
        De: <span> R${{shop.offers[2]?.freeField4}}</span> 
        &nbsp;&nbsp;&nbsp;&nbsp; 
        por: {{shop.offers[2]?.total | currency:'BRL':true}}
      </p>
    </form>
  </div>
</div>
```
```html
        <!--Formulario inicio-->
          <div class="row">
            <div class="col-sm-12 col-lg-8" track-scroll>
              <section id="register-customer-form">
                <form [formGroup]="checkoutForm">
                  <br>
                  <br>
                  <h2 style="color:#000;">
                    <img src="images/2_c.png" width="25" height="25" alt="">&nbsp;Valide seus dados e preencha o endereço de entrega.</h2>
                  <br>
                  <!--campo nome / FONE -->
                  <div class="row">
                    <!-- <AVISOS DO CAMPO NOME-->
                    <div class="col-sm-4">
                      <input id="firstName" name="firstName" placeholder="Digite seu nome" class="form-control" formControlName="firstName" [(ngModel)]="name"
                      minlength="3">
                      <div *ngIf="checkoutForm.controls.firstName.invalid && (checkoutForm.controls.firstName.dirty || checkoutForm.controls.firstName.touched)">
                        <div *ngIf="checkoutForm.controls.firstName.errors.required" class="erro">O campo Nome é obrigatório</div>
                        <div *ngIf="!checkoutForm.controls.firstName.errors.required && checkoutForm.controls.firstName.errors.minlength"
                        class="erro2">O campo Nome precisa ter pelo menos 3 caracteres.</div>
                      </div>
                      <!-- <AVISOS DO CAMPO NOME-->
                    </div>
                    <!-- <AVISOS DO CAMPO SOBRENOME-->
                    <div class="col-sm-4">
                      <input id="txtlastName" name="txtlastName" placeholder="Digite seu sobrenome" class="form-control" formControlName="lastName"
                      minlength="3">
                      <div *ngIf="checkoutForm.controls.lastName.invalid && (checkoutForm.controls.lastName.dirty || checkoutForm.controls.lastName.touched)">
                        <div *ngIf="checkoutForm.controls.lastName.errors.required" class="erro">O campo sobrenome é obrigatório</div>
                        <div *ngIf="!checkoutForm.controls.lastName.errors.required && checkoutForm.controls.lastName.errors.minlength"
                        class="erro2">O campo sobrenome precisa ter pelo menos 3 caracteres.</div>
                      </div>
                      <!-- <AVISOS DO CAMPO SOBRENOME-->
                    </div>
                    <div class="col-sm-4">
                      <input class="form-control" placeholder="Digite seu telefone" formControlName="phone" [(ngModel)]="phone" [textMask]="{mask: phoneOrCellMask}"
                      required>
                      <!-- <AVISOS DO CAMPO FONE-->
                      <div *ngIf="checkoutForm.controls.phone.invalid && (checkoutForm.controls.phone.dirty || checkoutForm.controls.phone.touched)">
                        <div *ngIf="checkoutForm.controls.phone.errors.required" class="erro">O campo telefone é obrigatório.</div>
                        <div *ngIf="!checkoutForm.controls.phone.errors.required && checkoutForm.controls.phone.errors"
                        class="erro2">telefone inválido.</div>
                      </div>
                      <!-- <AVISOS DO CAMPO FONE-->
                    </div>
                    <div class="col-sm-2"></div>
                  </div>
                  <!--CPF / EMAIL-->
                  <div class="row">
                    <div class="col-sm-6">
                      <input id="txtEmail" name="txtEmail" placeholder="Digite seu E-Mail" class="form-control" formControlName="email" [(ngModel)]="email">
                      <!-- <AVISOS DO CAMPO EMAIL-->
                      <div *ngIf="checkoutForm.controls.email.invalid && (checkoutForm.controls.email.dirty || checkoutForm.controls.email.touched)">
                        <div *ngIf="checkoutForm.controls.email.errors.required" class="erro">O campo E-mail é obrigatório.</div>
                        <div *ngIf="!checkoutForm.controls.email.errors.required && checkoutForm.controls.email.errors"
                        class="erro2">E-mail inválido.</div>
                      </div>
                      <!-- <AVISOS DO CAMPO EMAIL-->
                    </div>
                    <div class="col-sm-6">
                      <input id="txtCPF" name="txtCPF" placeholder="Digite seu CPF" class="form-control" formControlName="identity" [textMask]="{mask: cpfMask}">
                      <!-- <AVISOS DO CAMPO CPF -->
                      <div *ngIf="checkoutForm.controls.identity.invalid && (checkoutForm.controls.identity.dirty || checkoutForm.controls.identity.touched)">
                        <div *ngIf="checkoutForm.controls.identity.errors.required" class="erro">O campo CPF é obrigatório.</div>
                        <div *ngIf="!checkoutForm.controls.identity.errors.required && checkoutForm.controls.identity.errors"
                        class="erro2">Digite um CPF válido.</div>
                      </div>
                      <!-- CPF / EMAIL -->
```



## Estrutura do Projeto

```plaintext
/src
  /app
    /offers
      - offers.component.js         # Componente que exibe as ofertas de produtos
    /cart
      - cart.component.js           # Componente para visualização e manipulação do carrinho
    /checkout
      - checkout.component.js       # Componente principal do formulário de checkout
    /thank-you
      - thank-you.component.js      # Página de agradecimento após a compra
    /chat
      - chat.component.js           # Integração com sistema de chat
  /assets
    /images
      - logo.png                    # Imagem de logo ou ícone
  /styles
    - style.css                     # Estilos globais do projeto
```

Desenvolvido com ❤️ por Nilson Junior
