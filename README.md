# boilerplate-react-typescript-webpack-babel
React, TypeScript, Webpack, Babel 개발 환경을 구축해둔다. (without CRA)

NPM 대신 Yarn 을 사용한다 
- Yarn : https://yarnpkg.com/

# 환경 구축 
## 1. 프로젝트 생성 
- 일반적으로 repository 가 생성되어 있을 테니 그 폴더에서 작업을 한다.
- 만약 그렇지 않다면 새 프로젝트 폴더를 만든다 
- 명령어 : `mkdir <project_folder>`

## 2. 설치 과정 

### Yarn
Install
```command
yarn init -y
```

### Typescript
Install
```command
yarn add -D typescript @types/react @types/react-dom
```

`tsconfig.json`
```command
touch .tsconfig.json
```
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "umd",
    "lib": [
      "ESNext",
      "dom"
    ],
    "jsx": "react",
    "noEmit": true,    
    "sourceMap": true,
    
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
       
    "moduleResolution": "node",
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true
  },
  "include": ["src"]
}
```

### Webpack
Install
```command
yarn add -D webpack webpack-cli webpack-dev-server html-webpack-plugin
```

`webpack.config.js`
```command
touch webpack.config.js
```

```js
const path = require('path')
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: './src/index.tsx',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.(js|ts)x?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.jsx', '.js'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./src/index.html"),
    }),
  ],
  devServer: {
    static: {
      directory: path.resolve(__dirname, './dist')
    }
  }
}
```

### Babel
Install
```command
yarn add -D @babel/core babel-loader @babel/preset-env @babel/preset-react @babel/preset-typescript core-js  regenerator-runtime
```

`babel.config.json`
```command
touch babel.config.json
```

```json
{
  "presets": [
    "@babel/preset-typescript",
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "corejs": 3,
        "targets": "> 0.25%, not dead"
      }
    ],
    "@babel/preset-react"
  ],
}
```

### CSS
Install
```command
yarn add -D css-loader style-loader
```


### Git
Create
```command
touch .gitignore
```

.gitignore
```
.dist
.history
node_modules
```

### React
Install
```command
yarn add react react-dom
```

Create
```
mkdir src ; touch src/index.tsx src/App.tsx src/index.html
```

`src/index.tsx`
```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const rootNode = document.getElementById('app');
if (rootNode) {
  createRoot(rootNode).render(<App />);
}
```

`src/App.tsx`
```tsx
import React from 'react';

const App: React.FC = () => {
  return (
    <h1>Hello React</h1>
  )
}

export default App
```

`src/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React World</title>
  <script src="./bundle.js"></script>
</head>
<body>
  <div id="app" />  
</body>
</html>
```

## 3. 기타 설정
`package.json` 수정
```json
  "scripts": {
    "dev": "webpack serve --open --mode development",
    "build": "webpack --mode production --progress",
    "tscheck": "tsc"
  },
```

## 4. 실행

### 설치
```
yarn
```

### 개발 Development
```
yarn dev
```

### 빌드 Production
```
yarn build
```

### Type Error Check
```
yarn tscheck -w
```

## 기타 
수정
- 2022.0811 제작

참고 사이트 
- 기초 골대 
  - [Create React v18 TypeScript Project with webpack and Babel](https://itnext.io/create-react-typescript-project-with-webpack-and-babel-2431cac8cf5b)
- core-js 및 regenerator-runtime 에 대해 서칭 
  - [FE개발자의 성장 스토리 02 : Babel7과 corejs3 설정으로 전역 오염 없는 폴리필 사용하기](https://tech.kakao.com/2020/12/01/frontend-growth-02/)
