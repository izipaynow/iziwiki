## Boas práticas e padronizações

Recomenda-se a criação de um arquivo README.md para todo repositório inicializado de um projeto. Neste arquivo deve conter informações e contextualização sobre o projeto em questão, instruções de instalação, configuração e inicialização do projeto. Além de conter informações sobre especificidades do projeto (algo específico ou diferente do usual que o projeto em questão utiliza e precisa ser documentado).

TIP: **Dica** 
Abaixo segue um modelo de README.md que pode ser utilizado como ponto de partida, e ajustado/adaptado conforme necessidade do projeto.

<h1 align="center" style="font-weight: bold;">My API</h1>

<p align="center">
 <a href="#started">Getting Started</a> • 
  <!-- <a href="#routes">API Endpoints</a> • -->
 <a href="#patterns">Patterns</a> •
</p>

<p align="center">
  <b>My API Back-end Application</b>
</p>

<h5 align="center">Project Information and Context</h5>

<p align="center">Insert here the project context and relevant informations</p>


<h2 id="started">🚀 Getting started</h2>

<h3>Prerequisites</h3>

- [NodeJS](https://nodejs.org/en)
- [Git](https://git-scm.com/)

<h3>Cloning</h3>

How to clone your project

```bash
git clone https://github.com/dev-growdev/my-api.git
```
---

<h3> Environment Variables</h3>

Use the `.env-example` as reference to create your configuration file `.env`.

---

<h3>Starting</h3>

How to install and start your project

```
yarn install && yarn start:dev
```

---

<h3>Using Docker</h3>

To use Docker you will need the Docker Engine (Linux) or Docker Desktop (Win and MacOs) installed on your machine.

The project uses [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) VSCode Extension to build and run the app inside a Docker container.

---

<!-- <h2 id="routes">📍 API Endpoints</h2>

The API endpoints documentation could be found after running the project and accessing [http://localhost:8080/api/docs](http://localhost:8080/api/docs).

--- -->

<h3 id="patterns"> 💾 Commit Patterns</h3>

[Semantic Commit Messages](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716)

---


<h3> 📝 Project Patterns</h3>

<h5> Class name: PascalCase Pattern </h5>

- Example: CreateUser

---

<h5> Entity name: PascalCase Pattern and always in singular </h5>

- Example: UserEntity

---

<h5> File name: kebab-case Pattern </h5>

- Example: new-feature.service.ts

---

<h5> Class methods name: camelCase Pattern </h5>

- Example: getUserByUid

---

<h5> variable name: camelCase Pattern </h5>

- Example: const dataProfile

---

<h5> Folder/Features name: kebab-case Pattern and always in plural </h5>

- Example: users ou generic-features

- Service: A single file containing all feature methods
- Controller: A single file containing all feature methods

---

