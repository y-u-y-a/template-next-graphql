# Next.js + GraphQL + Hasura Application

- Node 14.17.0
- TypeScript 4.5.4
- Next.js 12.0.7
- TailwindCSS 3.0.12
- GraphQL 16.2.0
- ApollClient 3.5.6

## Create Project

### Install

```shell
# Create project
$ npx create next-app next-graphql-app

# Apollo Client
$ yarn add @apollo/client graphql @apollo/react-hooks cross-fetch @heroicons/react

# React-Testing-Library + MSW + next-page-tester
$ yarn add -D msw@0.35.0 next-page-tester jest @testing-library/react @types/jest @testing-library/jest-dom @testing-library/dom babel-jest @babel/core @testing-library/user-event jest-css-modules prettier

# TypeScript
$ yarn add -D typescript @types/react @types/node

# TailwindCSS
$ yarn add tailwindcss@latest postcss@latest autoprefixer@latest

# GraphQL
$ yarn add -D @graphql-codegen/cli @graphql-codegen/typescript
```

### TypeScript

- yarn dev 実行で自動的に設定が記述される

```shell
$ touch tsconfig.json
```

### TailwindCSS(v3.0.0〜)

```shell
# tailwind.config.js, postcss.config.js生成
$ npx tailwindcss init -p
```

```js:tailwind.config.js
module.exports = {
  // 追加
   content: [
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
}
```

```css:global.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### .babelrc

```shell
$ touch .babelrc
```

```json
{
  "presets": ["next/babel"]
}
```

### Package.json

- jest の記述
- prettier の記述

```json
"scripts": {
  ...
  "test": "jest --env=jsdom --verbose"
},
"jest": {
  "testPathIgnorePatterns": [
    "<rootDir>/.next/",
    "<rootDir>/node_modules/"
  ],
  "moduleNameMapper": {
    "\\.(css)$": "<rootDir>/node_modules/jest-css-modules"
  }
},
"prettier": {
  "tabWidth": 2,
  "printWidth": 150,
  "semi": false,
  "singleQuote": true,
  "bracketSpacing": true,
  "trailingComma": "es5",
  "arrowParens": "always",
  "jsxBracketSameLine": false
},
```

## Test 動作確認

```shell
$ mkdir __tests__
$ cd __tests__ && touch Home.test.tsx && cd ..
```

```ts:__test__/Home.test.tsx
import { render, screen } from '@testing-library/react'
import '@testing-library/jest-dom/extend-expect'
import Home from '../pages/index'

it('Should render title text', () => {
  render(<Home />)
  expect(screen.getByText('Next.js!')).toBeInTheDocument()
})
```

## Generate GraphQL schema

```shell
$ yarn graphql-codegen init && yarn
$ mkdir queries && cd queries && touch queries.ts && cd ..
# queries.tsに定義を記述後、下記を実行
$ yarn gqlgen
```

```shell:init
? What type of application are you building? Application built with React
? Where is your schema?: (path or url) https://next-graphql-app.hasura.app/v1/graphql
? Where are your operations and fragments?: queries/**/*.ts
? Pick plugins: TypeScript (required by other typescript plugins), TypeScript Operations (operations and fragments), TypeScript Reac
t Apollo (typed components and HOCs)
? Where to write the output: types/generated/graphql.tsx
? Do you want to generate an introspection file? No
? How to name the config file? codegen.yml
? What script in package.json should run the codegen? gen-types
```
