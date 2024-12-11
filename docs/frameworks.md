# Framework Frontend BankIzi (Work in progress)

A Bankizi utiliza em seus projetos internos e em alguns projetos de clientes um framework de front-end chamado [FUSE React](https://react-material.fusetheme.com/sign-in).

Fuse é um framework que já possui toda estrutura de autenticação e autorização pré-preparada à nível de front-end. Além de possuir roteamento e navegação embutidas, conta ainda com componentes próprios. Por baixo dos panos, o FUSE utiliza diversas libs consolidadas no ecossistema Javascript como React, Material-UI, TailwindCSS e ViteJS, ReduxToolKit, React-Hook-Form, etc. E está em constante atualização e evolução.

Ao iniciar um novo projeto, deve-se verificar a última versão estável disponível do FUSE e procurar iniciar o projeto sempre com a versão estável mais atual. Caso a versão estável atual seja muito recente (por exemplo, foi lançada há menos de duas semanas) pode ser interessante iniciar o projeto com a versão estável anterior, pois a versão mais recente pode conter bugs. ([FUSE Changelog](https://react-material.fusetheme.com/documentation/changelog))

<h3> Arquitetura para novas features </h3>

A organização de diretórios deve ser seguida conforme padrão do framework.

Exemplo:

```
|- users - (Funcionalidade)
   |- detail (Página de detalhe)
      |- components (se necessário)
      |- tabs (se necessário)
   |- list (Página de listagem)
   |- store (Em caso de uso de Redux)
      |- usersSlice.ts
      |- userSlice.ts
   UserRoute.tsx (Rotas das funcionalidades)
```

---

<h3> Componentes Genéricos/Reutilizáveis </h3>

- No diretório `shared-components` é onde deve ser armazenado componentes que serão reutilizados globalmente na aplicação.

<h3> Regras de Permissionamento/Autorização do Fuse </h3>

- No arquivo `configs/navigationConfig.ts` o auth é utilizado para visualização no sidebar.

- No arquivo `configs/routesConfig.ts` é o agregador das rotas da aplicação, onde cada [feat]Route.tsx é importado.

- Nos arquivos `main/[feat]/[feat]Route.tsx` agrupa as rotas da funcionalidade e permite a autorização através da propriedade `auth`.
   - Para ser possível utilizar o `LazyLoad` a página da funcionalidade deve ser exportada como `default`.

```

```


<h3> API Service </h3>

- No diretório `app/services` o arquivo `api.service.ts` é reponsável por realizar a instância do Axios, definir a BaseURL default apra chamadas de API e por conter os métodos HTTP que serão reutilizados na aplicação.