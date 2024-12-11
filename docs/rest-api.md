## Boas práticas e padronizações

### Sumário

<ol>
  <li><a href="#nomenclaturas">Nomenclaturas</a>
    <ol>
      <li><a href="#arquivos">Arquivos</a></li>
      <li><a href="#diretorios">Diretórios</a></li>
      <li><a href="#nome-classes">Nome de classes</a></li>
      <li><a href="#nome-metodos">Nome de métodos de classes</a></li>
      <li><a href="#nome-entidades">Nome de entidades</a></li>
      <li><a href="#nome-variaveis">Nome de variáveis</a></li>
    </ol>
  </li>
  <li><a href="#framework">Framework</a></li>
  <li><a href="#arquitetura">Arquitetura</a></li>
  <li><a href="#adr">ADR's</a></li>
  <li><a href="#docker">Docker</a></li>
  <li><a href="#testes">Testes</a></li>
</ol>

---

<h3 id="nomenclaturas">Nomenclaturas</h3>
<h4 id="arquivos">Arquivos</h4>

- Nome do arquivo em inglês
- Sempre no singular e <strong>se aplicavél</strong> com um sufixo representando o tipo de arquivo.
    - file.sufix.ts
- Todas as letras em minúsculo
- Utilizar o hífen (-) como separador das palavras.

> EXAMPLE: **Exemplos**
>
> * Controller: <strong>user.controller.ts</strong>
>   * Com separador: <strong>internal-ticket.controller.ts</strong>
> * Service: <strong>user.service.ts</strong>
> * Módulo: <strong>user.module.ts</strong>
> * Repository: <strong>user.repository.ts</strong>
> * Usecase: <strong>user.usecase.ts</strong>

---

<h4 id="diretorios">Diretórios</h4>

- Nome do arquivo em inglês
- Sempre no plural
- Todas as letras em minúsculo
- Utilizar o hífen (-) como separador das palavras.

> EXAMPLE: **Exemplos**
>
> * Diretório da feature/módulo de usuários: <strong>users</strong>
>   * Com separador: <strong>internal-tickets</strong>
> * <strong>enums</strong>
> * <strong>models</strong>
> * <strong>types</strong>

---

<h4 id="nome-classes">Nome de classes</h4>

- Nome da classe em inglês
- Seguir boas práticas de código limpo para nomeação de classes (escolher nomes claros e significativos para variáveis, funções e classes)
- Padrão <strong>PascalCase</strong>

> EXAMPLE: **Exemplos**
>
> * Classe responsável por gerenciar clientes: <strong>class CustomerManager</strong>
> * Classe responsável por lidar com serviços de pagamentos: <strong>class PaymentService</strong>

---

<h4 id="nome-metodos">Nome de métodos de classes</h4>

- Nome do método em inglês
- Seguir boas práticas de código limpo para nomeação de métodos de classe (escolher nomes claros e significativos para variáveis, funções e classes)
- Padrão <strong>camelCase</strong>

> EXAMPLE: **Exemplos**
>
> * Método responsável por enviar notificação por e-mail: <strong>public sendEmailNotification(email: string, message: string) {} </strong>
> * Método responsável por validar cartão de crédito: <strong>public validateCreditCard(creditCardNumber: string) {}</strong>

---

<h4 id="nome-entidades">Nome de entidades</h4>

- Nome da entidade em inglês
- Sempre no singular
- Padrão <strong>PascalCase</strong>

> EXAMPLE: **Exemplos**
>
> * Entidade de usuário: <strong>UserEntity</strong>
> * Entidade de cliente: <strong>ClientEntity</strong>
> * Entidade de Dados de perfil: <strong>DataProfileEntity</strong>

---

<h4 id="nome-variaveis">Nome de variáveis</h4>

- Nome da variável em inglês
- Seguir boas práticas de código limpo para nomeação de métodos de classe (escolher nomes claros e significativos para variáveis, funções e classes)
- Padrão <strong>camelCase</strong>

> EXAMPLE: **Exemplos**
>
> * Variável para armazenar total de clientes: <strong>const totalClients</strong>
> * Variável para armazenar idade de um usuário: <strong>const userAge</strong>
> * Variável para armazenar total de um pedido incluso taxas: <strong>const totalOrderPriceIncludingTax</strong>
