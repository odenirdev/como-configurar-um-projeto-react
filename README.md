# Como configurar um projeto React

Olá dev, neste artigo vamos aprender a como configurar um projeto profissional e pronto para ser escalado utilizando o framework javascript React.

Vamos configurar os seguintes objetos:

- O próprio React
- Babel e Webpack
- Estrutura do projeto

## Iniciando o projeto

Primeiro antes de iniciarmos precisamos configurar nosso
ambiente de desenvolvimento, instalando as seguintes ferramentas em nossa máquina:

- Nodejs
- Yarn
- Visual Studio Code

### Instalando o Nodejs

O Nodejs é o motor responsável por compilar nosso código em React.

Para conhecer como instalar o Nodejs em sua máquina, segue o [link](https://nodejs.dev/learn/how-to-install-nodejs).

### Instalando o Yarn

O Yarn é um gerenciador de pacotes, é ele o responsável por instalar, desinstalar e gerenciar as dependências do projeto.

Poderiamos o [npm](https://nodejs.org/en/knowledge/getting-started/npm/what-is-npm/) gerenciador de pacotes nativo do nodejs, porém o yarn é mais otimizado.

Para aprender como instalar o Yarn, segue o [link](https://classic.yarnpkg.com/lang/en/docs/install/#debian-stable).

### Visual Studio Code

O Visual Studio Code também conhecido como VS Code, é o nosso editor de texto opensource desenvolvido pela microsoft e toda comunidade, nele que iremos escrever nossos códigos e executar nossos projetos.

Para instalar o VS Code em sua maquina, segue o [link](https://code.visualstudio.com/download).

### Iniciar projeto

Maravilha! Agora com o ambiente de desenvolvimento configurado, podemos iniciar nosso projeto, para isso precisamos criar uma pasta para o projeto, basta usar o seguinte código no Terminal do MAC, Linux ou Powershell:

```bash
mkdir project-name && cd project-name
```

Depois de criar e abrir a pasta precisamos iniciar o projeto, rodando o seguinte código:

```bash
yarn init -y
```

Com isso nosso projeto deve ser iniciado criando o arquivo package.json na raiz.

O arquivo package.json é resposável por conter as informações gerais sobre o projeto. por exemplo: nome, versão, arquivo principal, tipo de licença, dependências de projeto, dependências de desenvolvimento e entre outros.

```json
{
  "name": "project-name",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

## React

Depois de inicar o projeto, podemos instalar nossa primeira dependência, o próprio React.

O React é um ecosistema de desenvolvimento de aplicações, utilizado para diversos tipos de dispositivos e que abstrai todo o desenvolvimento de app de uma forma intuitiva e divertida, utilizando os conceitos de componentização, props, hooks, styled-components e muitas outras funcionalidades.

Caso queria saber mais sobre o React, segue o link para [documentação](https://pt-br.reactjs.org/).

Para instalar o react e o react-dom que traz ferramentas para utilizar o react junto com a arvore DOM do HTML, basta rodar o código abaixo no terminal:

```bash
yarn add react react-dom
```

## Babel

O Babel é responsável pela transpilação do código js para que todos os navegadores entendam.

Para o funcionamento correto e integração com o react precisamos instalar as seguintes dependências:

- O próprio Babel como @babel/core;
- @babel/cli, que traz as linhas de comando do babel
- @babel/preset-env, que traz a identificação do navegador que está executando a aplicação e converte o código adequado para esse ambiente
- @babel/preset-react /, que traz toda a adequação para que o babel entenda um código react, tudo instalado como dependência de desenvolvimento.

E conseguimos instalar todas essas dependências, utilizando o código abaixo:

```bash
yarn add @babel/core @babel/cli @babel/preset-env @babel/preset-react -D
```

Depois de instalar todas as dependências nessas para a integração do babel com o react, precisamos configurá-la criando um arquivo chamado **babel.config.js** na raiz do projeto e configurando da seguinte forma:

```js
module.exports = {
  presets: [
    "@babel/preset-env",
    ["@babel/preset-react", { runtime: "automatic" }],
  ],
};
```

## Webpack

O Webpack é responsável pelo gerenciamento de importações de arquivos no js, por exemplo arquivos .sass, .png e .json, convertendo as importações de arquivos no js para tipos de arquivos estáticos entendíveis pelo navegador.

Ele também trabalha junto com o Babel, para que todos os navegadores entendam as importações.

Primeiro precisamos instalar o próprio webpack, webpack-cli que traz os comandos de linhas de comando que rodam no terminal.

```bash
yarn add webpack webpack-cli -D
```

Depois precisamos instalar os loaders, que segundo a própria [documentação do webpack](https://webpack.js.org/concepts/) são pré-processadores de arquivos, que traz todo o entendimento de qualquer tipo de arquivo para dentro do webpack.

Primeiro vamos instalar o babel-loader que traz a integração do babel com o webpack e o pré-processamento dos arquivos Javascript.

```bash
yarn add babel-loader -D
```

Também vamos instalar os pré-processadores de estilos e de arquivos css, que trabalham de forma conjunta para pré-processar arquivos css.

```bash
yarn add style-loader css-loader -D
```

Caso queria utilizar algum framework css para melhorar a experiência de estilização, por exemplo o [Sass](https://sass-lang.com/), podemos instalar o próprio Sass para Nodejs e o seu loader para o pré-processamento feito pelo Webpack.

```bash
yarn add node-sass sass-loader -D
```

E pronto finalizamos a parte dos loaders, agora podemos seguir instalando a lib html-webpack-plugin que tem como objetivo configurar a injetação do arquivo bundle.js gerado pelo webpack em um arquivo desejado, neste caso no public/index.html.

```bash
yarn add html-webpack-plugin -D
```

Para finalizar as instalações para configuração do webpack, vamos instalar a lib webpack-dev-server que traz a função de assistir(watch) os arquivos do projeto em ambiente de desenvolvimento e cross-env que auxilia na definição de variaveis de ambiente independente do sistema operacional que irá rodar a aplicação.

```bash
yarn add webpack-dev-server cross-env -D
```

Agora podemos criar o arquivo de configuração do webpack, para isso criamos um arquivo na raiz do projeto chamado **webpack.config.js**.

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const isDevelopment = process.env.NODE_ENV !== "production"; // Seleciona qual ambiente a aplicação está

module.exports = {
  mode: isDevelopment ? "development" : "production", // Aplica qual ambiente que a aplicação está e como o código bundle deve ser gerado
  devtool: isDevelopment ? "eval-source-map" : "source-map", // Ferramenta que gera a visualização do código no inspecionar do browser
  entry: path.resolve(__dirname, "src", "index.jsx"), // Caminho para o arquivo de entrada do projeto
  output: {
    // Arquivo de saída do processamento do webpack.
    path: path.resolve(__dirname, "dist"), // Caminho no qual o arquivo de saída deve ficar
    filename: "bundle.js", // Nome do arquivo de saída.
  },
  resolve: {
    extensions: [".js", ".jsx"], // Extenções dos arquivos que o webpack deve gerenciar
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "public", "index.html"), // Plugin para injetar o arquivo bundle.js gerado pelo webpack no public/index.html
    }),
  ],
  devServer: {
    // Configração do webpack-dev-server.
    static: {
      directory: path.resolve(__dirname, "public"), // Aponta para a pasta aonde está o arquivo index.html
    },
  },
  module: {
    // Regras de tratamento do webpack
    rules: [
      // Regra para entender e converter arquivos jsx
      {
        test: /\.jsx$/, // Buscar todos os arquivo que terminam em .jsx
        exclude: /node_modules/, // Menos os arquivo que estão na pasta node_modules
        use: "babel-loader", // E use o babel-loader para converter o arquivo em uma versão que qualquer outro navegador conheça
      },
      // Regra para entender e converter arquivos css
      {
        test: /\.scss$/,
        exclude: /node_modules/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
};
```

## Projeto

Depois de baixado as dependências e configurado os primeiros arquivos(webpack.config.js e babel.config.js) teremos um package.json parecido com o apresentado abaixo:

```json
{
  "name": "setup-react-project", // Nome do projeto
  "version": "1.0.0", // Versão do projeto
  "main": "index.jsx", // Arquivo principal
  "license": "MIT", // Licença do projeto
  "dependencies": {
    // Lista de dependências
    "node-sass": "^7.0.1",
    "react": "^17.0.2",
    "react-dom": "^17.0.2"
  },
  "devDependencies": {
    // Lista de dependências de desenvolvimento
    "@babel/cli": "^7.17.0",
    "@babel/core": "^7.17.2",
    "@babel/preset-env": "^7.16.11",
    "@babel/preset-react": "^7.16.7",
    "babel-loader": "^8.2.3",
    "cross-env": "^7.0.3",
    "css-loader": "^6.6.0",
    "html-webpack-plugin": "^5.5.0",
    "sass-loader": "^12.6.0",
    "style-loader": "^3.3.1",
    "webpack": "^5.68.0",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^4.7.4"
  }
}
```

E podemos criar nossos **scripts**, que são atalhos para comandos de terminal, vamos criar o script para o comando que tem como objetivo rodar nosso projeto em ambiente de desenvolvimento e ambiente de produção.

```json
{
  "name": "setup-react-project",
  "version": "1.0.0",
  "main": "index.jsx",
  "license": "MIT",
  "scripts": {
    "dev": "webpack serve",
    "build": "cross-env NODE_ENV=production webpack"
  },
  "dependencies": {
    "node-sass": "^7.0.1",
    "react": "^17.0.2",
    "react-dom": "^17.0.2"
  },
  "devDependencies": {
    "@babel/cli": "^7.17.0",
    "@babel/core": "^7.17.2",
    "@babel/preset-env": "^7.16.11",
    "@babel/preset-react": "^7.16.7",
    "babel-loader": "^8.2.3",
    "cross-env": "^7.0.3",
    "css-loader": "^6.6.0",
    "html-webpack-plugin": "^5.5.0",
    "sass-loader": "^12.6.0",
    "style-loader": "^3.3.1",
    "webpack": "^5.68.0",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^4.7.4"
  }
}
```

Agora podemos finalizar a configuração do React, criando o HTML raiz do projeto, para isso criamos uma pasta chamada **public/** na raiz do projeto, essa pasta é reponsável pro apresentar os arquivos de forma pública, após isso criamos o arquivo **index.html** dentro da pasta public, com o seguinte conteúdo:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App</title>
  </head>

  <body>
    <!-- Elemento do html no qual será renderizado a aplicação React -->
    <div id="root"></div>

    <!-- Código fonte do projeto convertido pelo babel e webpack -->
    <script src="../dist/bundle.js"></script>
  </body>
</html>
```

Agora podemos criar, nossa pasta **src/** que é responsável por conter todos os arquivos e códigos referente a aplicação.

Primeiro criamos nosso arquivo **index.jsx**, que tem como objetivo fazer a ponte entre o arquivo App.jsx, que vamos criar em seguida, com o arquivo index.html, renderizando nosso JSX dentro da DOM do HTML, da forma apresentado abaixo:

```jsx
import React from "react"; // Importação do react
import { render } from "react-dom"; // Importação de render(), função responsável por renderizar componentes React como filho de um elemento HTML.
import { App } from "./App"; // Componente App

render(<App />, document.getElementById("root")); // Renderizando o componente App como filho do elemento HTML com o id root.
```

Agora podemos criar o arquivo **App.jsx**, que nada mais é o ponto de partida da nossa aplicação React.

```jsx
import React from "react";

import "./styles/global.scss";

export const App = () => {
  return <h1>Hello React.js</h1>;
};
```

E por fim teremos uma estrutura de pastas parecida com a apresentada abaixo:

📦project-name
┣ 📂node_modules -- Pasta das dependências

┣ 📂public -- Pasta dos arquivos públicos

┃ ┗ 📜index.html -- Arquivo HTML raiz do projeto
┣ 📂src -- Pasta de desenvolvimento do App
┃ ┣ 📜App.jsx -- Arquivo JS raiz do App
┃ ┗ 📜index.jsx -- Arquivo JS raiz do projeto
┣ 📜babel.config.js -- Arquivo de configuração do babel
┣ 📜webpack.config.js -- Arquivo de configuração do webpack
┣ 📜package.json -- Arquivo de configuração do projeto
┗ 📜README.md -- Arquivo de documentação do projeto

Parabens dev!! Você aprendeu como funciona a configuração de uma aplicação profissional e escalável, utilizando React, Babel, Webpack, Sass e entre outras ferramentas.

Você também pode gerar todo o projeto configurado utilizando o [create-react-app](https://github.com/facebook/create-react-app) ou utilizar uma ferramenta mais moderna, por exemplo o [vite](https://vitejs.dev/).
