# Front-end React JS

Babel: Converter (transpilar) código do React para um código que o browser entende.

Webpack: Para cada tipo de arquivo (.js, .css, .png) eu vou converter o código de uma maneira diferente.

Loaders: (dentro do Webpack) babel-loader, css-loader, image-loader, file-loader

## React

```
mkdir frontend
cd frontend
yarn init -y
```

Adiciona pasta `src` e pasta `public` com um arquivo `index.html` dentro.

No `index.html` criar a estrutura de HTML5
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React JS</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

Instalar pacotes:
```
yarn add react react-dom
```

## Babel
```
yarn add @babel/core @babel/preset-env @babel/preset-react webpack webpack-cli
```

Criar o arquivo `babel.config.js` na raiz do projeto
```js
module.exports = {
  presets: [
    '@babel/preset-env',
    '@babel/preset-react',
  ]
}
```

Instalar pacote:
```
yarn add @babel/cli
```

Criar um arquivo `index.js` dentro da pasta `src` e escrever sintaxe recente do JS
```js
const soma = (a, b) => {
  return a + b
}

console.log(soma(1, 3))
```

No terminal, rodar o babel para transpilar o arquivo `src/index.js` para um JS mais antigo e alocar no arquivo `public/bundle.js` para que o browser consiga entender
```
yarn babel src/index.js --out-file public/bundle.js
```

## Webpack
Criar um arquivo `webpack.config.js` na raiz do projeto
```js
const path = require('path')

module.exports = {
  entry: path.resolve(__dirname, 'src', 'index.js'),
  output: {
    path.resolve(__dirname, 'public'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      {}
    ]
  }
}
```
Onde:
`entry` é o arquivo de entrada
`output` é o arquivo de saída

Instalar pacote:
```
yarn add babel-loader
```
Adicionar a rule para loader de arquivos com extensão `.js` dentro das `rules`
```js
...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
...
```

Rodar no terminal:
```
yarn webpack --mode development
```
No arquivo `bundle.js` já aparece muito mais código.
Agora também é possível fazer o import e export de funções entre arquivos `.js`.
Jogando o conteúdo da função soma para o arquivo `src/soma.js`:
```js
export const soma = (a, b) => {
  return a + b
}
```
E importando a função no arquivo `src/index.js` o código vai continuar funcionando
```
import { soma } from './soma'

console.log(soma(1, 3))
```

Instalar pacote:
```
yarn add webpack-dev-server -D
```
No arquivo de `webpack.config.js` adicionar o seguinte propriedade
```

```
`contentBase` é o caminho onde contém o diretório público da nossa aplicação
Agora rodar no terminal:
```
yarn webpack-dev-server --mode development
```
Agora o projeto já está rodando em (http://localhost:8080)[http://localhost:8080] e está _Live reloading enabled_, ou seja, já está atualizando o browser assim que as alterações no código são salvas.

## Componentização
Limpamos os arquivos dentro da pasta `src`
Agora é vamos criar os componentes `html` dentro do arquivo `js`.
Usaremos a `div` com `id="app"` que está dentro do `index.html`. É dentro dessa div que vão todos os componentes gerados pelo React.
No arquivo `src/index.js` importamos o `React` de `react` e o `render` do `react-dom` e podemos criar `html` dentro do `js` nesse método `render` desde que esteja configurado o `preset` `'@babel/preset-react'` dentro do `babel.config.js`.

JSX: HTML dentro de Javascript (Javascript XML)

```js
import React from 'react'
import { render } from 'react-dom'

render(<h1>Hello World</h1>, document.getElementById('app'))
```

Olhar no browser e inspecionar elementos. A estrutura já estará assim:
```html
<div id="app">
  <h1>Hello World</h1>
</div>
```

Criar o arquivo `src/App.js` e toda vez que for criar componente no React é convencionado que o nome do arquivo comece com letra maiúscula.
Um componente no React nada mais é que uma função que retorna um HTML.
Todo compontente precisa ter o React importado.
```js
import React from 'react';

function App() {
  return <h1>Hello World</h1>
}

export default App
```

Agora dentro do arquivo `src/index.js` é possível importar nosso componente App e consegue escrever como uma tag de HTML:
```js
import React from 'react'
import { render } from 'react-dom'

import App from './App'

render(<App />, document.getElementById('app'))
```
Como não tem nenhum conteúdo, pode-se auto-fechar a tag nesse formato `<App />`

Isolar os componentes da aplicação numa pasta `src/components` e criar o componente `Header.js`
```js
import React from 'react'

export default function Header() {
  return (
    <header>
      <h1>React JS</h1>
    </header>
  )
}
```

Importa o `Header` dentro do `App` e se tiver mais que um componente, é necessário englobar com uma Fragment
```js
import React from 'react';

import Header from './components/Header'

function App() {
  return (
    <>
      <Header />
      <Header />
    </>
  )
}

export default App
```

## Propriedade
Quando precisa passar informação do componente pai para o componente filho, passa-se uma propriedade (como se fosse um atributo do HTML) dentro da tag React. E já na função do componente, precisa capturar a propriedade no componente como parâmetro.
No arquivo `src/App.js` então, pode-se adicionar a propriedade `title` no componente `Header` dessa forma:
```js
function App() {
  return (
    <>
      <Header title="Homepage" />
      <Header title="Projects" />
    </>
  )
}
```

E no arquivo `src/components/Header.js` fazer a seguinte alteração. Lembrando que quando se quer colocar uma variável Javascript dentro de tag HTML, é necessário colocar {} em volta.
```js
export default function Header(props) {
  return (
    <header>
      <h1>{props.title}</h1>
    </header>
  )
}
```

Ou pode-se pegar a propriedade pela desestruturação da propriedade:
```js
import React from 'react'

export default function Header({ title }) {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  )
}
```

Outra propriedade é o `children` que é o conteúdo colocado dentro da tag. Então, o `App.js` ficaria:
```js
function App() {
  return (
    <>
      <Header title="Homepage">
        <ul>
          <li>Homepage</li>
          <li>Projects</li>
        </ul>
      </Header>
      <Header title="Projects">
        <ul>
          <li>Homepage</li>
          <li>Projects</li>
          <li>Loogin</li>
        </ul>
      </Header>
    </>
  )
}
```

E o componente `Header` ficaria assim:
```js
export default function Header({ title, children }) {
  return (
    <header>
      <h1>{title}</h1>

      {children}
    </header>
  )
}
```

## Estado e Imutabilidade
Manipular e armazenar alguma informação dentro do componente.
O React consegue renderizar novamente a inteface quando alguma variável sofre alteração.
Mas não dá para usar variáveis tradicionais. Precisa usar o conceito de Estado.
Da maneira antiga, a tela fica sem exibir as alterações sofridas.

O `Header.js` foi limpo:
```js
export default function Header({ title, children }) {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  )
}
```

E o `App.js` foi alterado para:
```js
function App() {
  const projects = ['Desenvolvimento de app', 'Front-end web']

  function handleAddProject() {
    projects.push(`Novo projeto ${Date.now()}`)
    console.log(projects)
  }
  
  return (
    <>
      <Header title="Projects" />
      <ul>
        {projects.map(project => <li key={project}>{project}</li>)}
      </ul>

      <button type="button" onClick={handleAddProject}>Adicionar projeto</button>
    </>
  )
}

export default App
```

O click no botão altera o array project porém a tela não está refletindo essas atualizações.
O poder do React é justamente atualizar os componentes da tela quando as variáveis forem alteradas.
Mas nesse caso, ao invés de variáveis, utiliza-se useState do react.
useState retorna um array com 2 posições: na primeira posição retorna a variável com seu valor inicial, e a segunda posição é uma função que vai atualizar esse valor.
A função `setProjects` deve respeitar a imutabilidade da variável, então o `projects.push('Novo projeto')` já não serve, pois o método `push` altera o valor do array inicial. Por isso, uma das formas para respeitar a imutabilidade é fazendo o spread operator no array inicial e adicionar o novo valor.
Alterando o `App.js` então:
```js
import React, { useState } from 'react';

import Header from './components/Header'

function App() {
  const [projects, setProjects] = useState(['Desenvolvimento de app', 'Front-end web'])

  function handleAddProject() {
    setProjects([...projects, `Novo projeto ${Date.now()}`])
  }
  
  return (
    <>
      <Header title="Projects" />
      <ul>
        {projects.map(project => <li key={project}>{project}</li>)}
      </ul>

      <button type="button" onClick={handleAddProject}>Adicionar projeto</button>
    </>
  )
}

export default App
```

Instalando novo loader de arquivos `.css`
```
yarn add style-loader css-loader
```

E adicionar no webpack. O css-loader vai lidar com importações de imagens dentro do css enquanto que o style-loader vai pegar esse css e colocar dentro do html.
```js
const path = require('path')

module.exports = {
  entry: path.resolve(__dirname, 'src', 'index.js'),
  output: {
    path: path.resolve(__dirname, 'public'),
    filename: 'bundle.js',
  },
  devServer: {
    contentBase: path.resolve(__dirname, 'public'),
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: [
          { loader: 'style-loader' },
          { loader: 'css-loader' },
        ]
      }
    ]
  }
}
```

Roda o projeto novamente
```
yarn webpack-dev-server --mode development
```

Adiciona um arquivo `src/App.css`
```css
* {
  margin: 0;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
}

body {
  background: #f5f5f5;
  font: 14px sans-serif;
  color: #333;
}
```

E importa esse arquivo no `App.js`:
```js
import './App.css'
```

O Webpack já vai compilar corretamente.
O `style-loader` já vai injetar o `.css` dentro da minha página `html`

Para facilitar na hora de rodar o projeto, foram adicionados os scripts no `package.json`:
```json
  "scripts": {
    "dev": "webpack-dev-server --mode development",
    "build": "webpack --mode production"
  },
```

E para rodar o projeto agora basta colocar no terminal:
```
yarn dev
```

Adicionar o package `file-loader` para carregar arquivos para dentro da nossa aplicação:
```
yarn add file-loader
```

Salvar uma imagem `background.jpeg` dentro de `src/assets` e importar dentro do componente:
```js
import React, { useState } from 'react';

import './App.css'
import backgroundImage from './assets/background.jpeg'

import Header from './components/Header'

function App() {
  const [projects, setProjects] = useState(['Desenvolvimento de app', 'Front-end web'])

  function handleAddProject() {
    setProjects([...projects, `Novo projeto ${Date.now()}`])
  }
  
  return (
    <>
      <Header title="Projects" />
      <img src={backgroundImage} alt="background"/>
      <ul>
        {projects.map(project => <li key={project}>{project}</li>)}
      </ul>

      <button type="button" onClick={handleAddProject}>Adicionar projeto</button>
    </>
  )
}

export default App
```

Também é necessário configurar o `webpack` e incluir o `file-loader`:
```js
      {
        test: /.*\.(gif|png|jpe?g)$/i,
        use: { loader: 'file-loader' }
      }
```

Toda vez que faz alteração no webpack, tem que reiniciar o servidor:
```
yarn dev
```

## Conectando o Front-end (React JS) com o Back-end (Node JS)
Fazer requisições HTTP do React para trazer os dados da API.

Abrir o projecto `backend` e iniciar
```
yarn dev
```

Por enquanto, criar 2 projetos pelo Insomnia.

Instalar o pacote `axios`
```
yarn add axios
```

Criar arquivo `api.js` dentro da pasta `src/services`. Essa pasta armazena qualquer arquivo que vai fornecer comunicação com algum serviço externo.

Criando uma instância do `axios` com a `baseURL` do nosso backend
```js
import axios from 'axios'

const api = axios.create({
  baseURL: 'http://localhost:3333'
})

export default api
```

Importanto `api` agora dentro do `App` e `useEffect` para disparar uma função quando tiver uma informação alterada ou disparar uma função quando o componente for exibido em tela.
O `useEffect` recebe 2 parâmetros. O primeiro é uma função que vai ser disparada, e o segundo parâmetro é quando eu quero disparar essa função. Deixa o 2º parâmetro como um array vazio `[]` quando quer que a função seja disparada apenas 1x assim que o componente for mostrado na tela. Ou deixa como `[projects]` quando o `projects` sofrer alguma alteração. Este array é o array de dependências e vamos incluir nele as variáveis que utilizamos dentro da função no 1º parâmetro do useEffect. Porque provavelmente iremos querer que a função seja executada quando alguma variável dela seja alterada.
Como queremos que a função seja executada somente quando a tela for montada, vamos deixar o array vazio.
Utilizamos o método `get` para fazer a requisição de listar nossos projetos do backend.
```js
import React, { useState, useEffect } from 'react';
import api from './services/api'

import './App.css'

import Header from './components/Header'

function App() {
  const [projects, setProjects] = useState(['Desenvolvimento de app', 'Front-end web'])

  useEffect(() => {
    api.get('projects').then(response => {
      console.log(response)
    })
  }, [])

  function handleAddProject() {
    setProjects([...projects, `Novo projeto ${Date.now()}`])
  }
  
  return (
    <>
      <Header title="Projects" />
      <ul>
        {projects.map(project => <li key={project}>{project}</li>)}
      </ul>

      <button type="button" onClick={handleAddProject}>Adicionar projeto</button>
    </>
  )
}

export default App
```

Deu problema de CORS.
Abrir o projeto do backend, instalar o pacote cors
```
yarn add cors
```
E adiciona o `cors` dentro do `src/index.js` do backend:
```js
const express = require('express')
const cors = require('cors')
const { uuid, isUuid } = require('uuidv4')

const app = express()

app.use(cors())
app.use(express.json())
```
Com somente `cors()` deixamos aberto para qualquer frontend acessar nosso backend. Futuramente, podemos limitar nosso backend para receber requisição somente do nosso frontend, por exemplo:
```js
app.use(cors({
  origin: 'http://localhost:3000'
}))
```

Como o backend foi alterado, temos que criar novamente nossos projetos pelo Insomnia.

Ao atualizar o front, já vemos que o `console.log(response)` retorna com os projetos num array de `data`.
Para preencher os `projects` com esse `data`:
```js
  useEffect(() => {
    api.get('projects').then(response => {
      setProjects(response.data)
    })
  }, [])
```

E como os projetos que vêm da API tem outro formato, vamos inicializar `projects` com um array vazio:
```js
const [projects, setProjects] = useState([])
```

E também alterar dentro do componente, pois agora cada projeto contém um `title` e um `id`
```js
      <ul>
        {projects.map(project => <li key={project.id}>{project.title}</li>)}
      </ul>
```

## Cadastrando um projeto na API
Ao fazer uma requição `post` para a api, é esperado um body contendo `owner` e `title`. Então, dentro da função `handleAddProject` fazemos a seguinte alteração:
```js
    api.post('projects', {
      title: `Novo projeto ${Date.now()}`,
      owner: "Cintia Yamamoto"
    })
```
Por enquanto, estamos com post estático que envia sempre a mesma informação.

E os projetos não são atualizados em tela.
Dessa forma, aproveitamos o retorno da requisição para fazer a atualização da tela.

Observação é que não pode colocar `async await` dentro do `useEffect`, mas no `handleAddProject` pode.
```js
  async function handleAddProject() {
    const response = await api.post('projects', {
      title: `Novo projeto ${Date.now()}`,
      owner: "Cintia Yamamoto"
    })
    const project = response.data
    setProjects([...projects, project])
  }
```

Mas deu um erro pois o babel não consegue transpilar o `async await`. Então, vamos adicionar mais um plugin:
```
yarn add @babel/plugin-transform-runtime -D
```

E configurar o `babel.config.js`
```js
module.exports = {
  presets: [
    '@babel/preset-env',
    '@babel/preset-react',
  ],
  plugins: [
    '@babel/plugin-transform-runtime',
  ]
}
```
