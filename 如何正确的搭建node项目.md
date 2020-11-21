---
title: "如何正确的搭建node项目"
date: 2020-10-05T11:46:57+08:00
---

##  环境配置

https://kulshekhar.github.io/ts-jest/

 `yarn init -y`

### typescript-jest

安装所需的依赖
 `yarn add --dev jest typescript ts-jest @types/jest`
初始化
 `yarn ts-jest config:init`

配置
  [配置](https://kulshekhar.github.io/ts-jest/user/config/)

package.json中的scripts中添加

`"eslint": "eslint --ext .tsx,.ts --fix ./src"`

### typescript-eslint-Prettier

`yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --dev`

`yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev`

编辑.eslintrc.js

```javascript
module.exports = {
    parser: '@typescript-eslint/parser', // Specifies the ESLint parser
    extends: [
        'plugin:jsx-control-statements/recommended',
        'prettier/@typescript-eslint', // Uses eslint-config-prettier to disable ESLint rules from @typescript-eslint/eslint-plugin that would conflict with prettier
        'plugin:prettier/recommended', // Enables eslint-plugin-prettier and displays prettier errors as ESLint errors. Make sure this is always the last configuration in the extends array.
    ],
    plugins: ['@typescript-eslint', 'jsx-control-statements', 'prettier'],
    env: {
        browser: true,
        node: true,
        es6: true,
        mocha: true,
        'jsx-control-statements/jsx-control-statements': true
    },
    globals: {
        $: true
    },
    rules: {
        'prettier/prettier': 1,
        // 'no-console': ['warn', { allow: ['warn', 'error'] }],
        "eqeqeq": ['warn', 'always'],
        "prefer-const": ['error', {"destructuring": "all", "ignoreReadBeforeAssign": true}],
        '@typescript-eslint/indent': ['error', 4, { VariableDeclarator: 4, SwitchCase: 1 }],
        '@typescript-eslint/no-unused-vars': 0,
        "@typescript-eslint/interface-name-prefix": 0,
        "@typescript-eslint/explicit-member-accessibility": 0,
        "@typescript-eslint/no-triple-slash-reference": 0,
        "@typescript-eslint/ban-ts-ignore": 0,
        "@typescript-eslint/no-this-alias": 0,
        "@typescript-eslint/triple-slash-reference": ['error', { "path": "always", "types": "never", "lib": "never" }],
    }
};

```

.prettierrrc.js

```javascript
module.exports = {
    trailingComma: "all",
    printWidth: 120,
    tabWidth: 4,
    singleQuote: true,
    semi: true,
    trailingComma: 'es5',
    bracketSpacing: true,
    jsxBracketSameLine: true,
    arrowParens: 'always',
    parser: 'typescript'
};
```
package.json中的lock文件添加

`"lint": "eslint --ext .tsx,.ts --fix ./src"`

### 使用husky

`yarn add husk --dev`

package.json添加

```json
{
  "husky": {
      "hooks": {
          "pre-commit": "lint-staged"
      }
  },
  "lint-staged": {
      "*.{js,ts,tsx}": [
          "eslint --fix"
      ]
  }
}
```

### 开发

`npx tsc --init`

编辑tsconfig.json
```json
{
  "compilerOptions": {
    /* Basic Options */
    "target": "es6",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    // "lib": [],                             /* Specify library files to be included in the compilation. */
    // "allowJs": true,                       /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "./dist",                        /* Redirect output structure to the directory. */
    // "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "incremental": true,                   /* Enable incremental compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                   /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "dist",
    "node_modules"
  ],
  "compileOnSave": true
}
```

安装concurrently 

`yarn add concurrently --dev`

package.json改为

```json
{
    "name": "string-to-json",
    "version": "1.0.0",
    "main": "dist/index.js",
    "scripts": {
        "start": "npx concurrently \"npm run dev\" \"npm run jest:watch\"",
        "dev": "npx tsc -w",
        "jest:watch": "jest --watch",
        "test": "jest",
        "lint": "eslint --ext .tsx,.ts --fix ./src",
        "build": "npx tsc"
    },
    "repository": "git@github.com:wuhaohao1234/string-to-json.git",
    "author": "wuhaohao1234 <1611499758@qq.com>",
    "license": "MIT",
    "husky": {
        "hooks": {
            "pre-commit": "lint-staged"
        }
    },
    "lint-staged": {
        "*.{js,ts,tsx}": [
            "eslint --fix"
        ]
    },
    "devDependencies": {
        "@types/jest": "^26.0.14",
        "@typescript-eslint/eslint-plugin": "^4.3.0",
        "@typescript-eslint/parser": "^4.3.0",
        "babel-eslint": "^10.1.0",
        "concurrently": "^5.3.0",
        "eslint": "^7.10.0",
        "eslint-config-prettier": "^6.12.0",
        "eslint-plugin-jsx-control-statements": "^2.2.1",
        "eslint-plugin-prettier": "^3.1.4",
        "eslint-plugin-react": "^7.21.3",
        "jest": "^26.4.2",
        "prettier": "^2.1.2",
        "ts-jest": "^26.4.1",
        "typescript": "^4.0.3"
    }
}

```

开发执行`yarn start`

### 使用commitlint约束commit 字段

`yarn add @commitlint/config-conventional @commitlint/cli`

编辑commitlint.config.js

```javascript
module.exports = {extends: ['@commitlint/config-conventional']}
```
添加到husky中hooks

```
 "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
```

参考: https://github.com/wuhaohao1234/learncommitizen-

### 后期

1. 这里包括开源项目发布到npm

https://my.oschina.net/fenying/blog/1607571

2. 使用持续集成

jekins 

travis-ci 

circle-ci 

github 

actions