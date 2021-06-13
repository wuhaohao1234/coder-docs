---
title: "Ast语法树"
date: 2021-05-01T23:09:38+08:00
---

# ast语法树

几个关键库

1. parser

讲code代码转换为语法树

const ast = parser.parse(code)

2. traverse

语法树遍历

const visitor = {
  CallExpression(path) {
    const { callee } = path.node
  }
}

traverse.default(ast, visitor)

3. generate

将语法树转换为代码

generage.default(ast, {}, code)