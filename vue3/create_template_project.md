# 创建Vite + Vue3 + TypeScript模板项目

## 技术清单

// TODO

## 使用vite搭建项目

使用npm:
  
```
npm create vite@latest
```

使用yarn:
  
```
yarn create vite
```

使用pnpm:
```
pnpm create vite
```


## 代码规范

使用 EditorConfig + Prettier + ESLint 组合来实现代码规范化

### 集成 EditorConfig 配置

> VSCode 使用 EditorConfig 需要去插件市场下载插件 EditorConfig for VS Code

在项目根目录下增加 .editorconfig 文件：

```
# Editor configuration, see http://editorconfig.org

# 表示是最顶层的 EditorConfig 配置文件
root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行
```


### 集成 Prettier 配置

> VSCode 编辑器使用 Prettier 配置需要下载插件 Prettier - Code formatter

1. 安装 Prettier

```
npm i prettier -D
```


2. 创建并配置 Prettier 配置文件

在项目根目录下创建 .prettierrc 文件，并添加 Prettier 代码风格规则

```
# .prettierrc

{
  "semi": false,
  "singleQuote": true,
  "printWidth": 80,
  "trailingComma": "none",
  "arrowParens": "avoid",
  "proseWrap": "never"
}
```


3. 使用 Prettier 命令格式化代码

```
# 格式化所有文件（. 表示所有文件）
npx prettier --write .
```


### 集成 ESLint 配置

> VSCode 使用 ESLint 配置文件需要去插件市场下载插件 ESLint

1. 安装 ESLint 

```
npm i eslint -D
```


2. 创建 ESLint 配置文件

执行 `npx eslint --init` 命令创建配置文件

```
npx eslint --init
```

- How would you like to use ESLint?

√ To check syntax, find problems, and enforce code style

-  How would you like to use ESLint?

√ JavaScript modules (import/export)

- Which framework does your project use?

√ Vue.js

- Does your project use TypeScript?
  
√ Yes

- Where does your code run?
  
√ Browser
√ Node

- How would you like to define a style for your project?

√ Use a popular style guide

- Which style guide do you want to follow?

√ Airbnb: https://github.com/airbnb/javascript

- What format do you want your config file to be in?

√ JavaScript

- Would you like to install them now?

√ Yes

- Which package manager do you want to use?

√ npm


命令执行完后会在根目录下生成 .eslintrc.js 文件

3. 配置 ESLint 配置文件

安装插件

```
npm i eslint-plugin-prettier eslint-config-prettier -D
```

配置 .eslintrc.js 

```
# .eslintrc.js

module.exports = {
  ...
  rules: {
    'import/no-extraneous-dependencies': ['error', { devDependencies: true }],
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off'
  },
  ...
  extends: [
    ...
    // vue 插件默认为 vue2 ，替换为 vue3 的插件
    // plugin:vue/essential
    'plugin:vue/vue3-recommended',
    // 添加 import 插件
    'plugin:import/recommended',
    // 添加 prettier 插件，解决 Prettier 和 ESLint 的冲突
    'plugin:prettier/recommended',
  ],
  ...
}
```


4. 使用 ESLint 命令格式化代码

```
# 格式化所有文件（. 表示所有文件）
npx eslint --fix .
```


### 配置vite.config.ts

1. 配置路径别名

修改vite.config.ts文件

```
# vite.config.ts

...
import path from 'path'
...

export default defineConfig({
  ...
  resolve: {
    alias: {
      // 设置 `@` 指向 `src` 目录
      '@': path.resolve('./src')
    },
    // 使用路径别名时想要省略的后缀名
    extensions: ['.ts']
  }
  ...
})
```

遇到的问题及解决方案

- import path from 'path' 警告：找不到模块"path"或其相应的类型声明

解决方案：安装@types/node

```
npm i @types/node -D
```

- import path from 'path' 警告：模块 ""path"" 只能在使用"allowSyntheticDefaultImports" 标志时进行默认导入

解决方案：

在 tsconfig.node.json 添加 compilerOptions 对象属性添加"allowSyntheticDefaultImports": true即可

```
# tsconfig.node.json

{
  ...
  "compilerOptions": {
    ...
    "allowSyntheticDefaultImports": true
    ...
  }
  ...
}
```

- 使用别名导入后 eslint 提示找不到文件警告：unable to resove path to module '@/xxx'. eslint(import/no-unresolved)

解决方案：

安装 eslint-import-resolver-alias 插件

```
npm install -D eslint-import-resolver-alias
```

在 .eslintrc.js 添加以下配置

```
module.exports = {
  ...
  settings: {
    'import/resolver': {
      alias: [['@', './src']]
    }
  }
  ...
}

```
