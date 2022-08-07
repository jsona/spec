# JSONA

其他语言版本: [English](./README.md)

- [JSONA](#jsona)
  - [介绍](#介绍)
  - [示例](#示例)
  - [JSON](#json)
    - [支持注释](#支持注释)
    - [属性名随意使用引号](#属性名随意使用引号)
    - [允许多余尾逗号](#允许多余尾逗号)
    - [浮点数允许只有整数部分或小数部分](#浮点数允许只有整数部分或小数部分)
    - [多种进制支持](#多种进制支持)
    - [支持单引号和反引号](#支持单引号和反引号)
    - [多行字符串](#多行字符串)
    - [转义字符串](#转义字符串)
  - [注解](#注解)
    - [插入位置](#插入位置)
    - [注解值](#注解值)
  - [相关项目](#相关项目)

## 介绍

JSONA = JSON + Annotation。JSON 描述数据，Annotation 承载逻辑。

## 示例

下面的示例涵盖了 JSONA 的所有特性。

```
/*
 multiple line comment
*/

// single line comment

{
    @foo /* abc */ @optional
    @null(null) // single line comment
    @bool(true)
    @float(3.14)
    @number(-3)
    @string('abc "def" ghi')
    @array([3,4])
    @object({
        k1: "v1",
        k2: "v2",
    })

    nullValue: /* xyz */ null,
    boolTrue: true,
    boolFalse: false,
    float: 3.14,
    floatNegative: -3.14,
    floatNegativeWithoutInteger: -.14,
    floatNegativeWithoutDecimal: -3.,
    integer: 3,
    hex: 0x1a,
    binary: 0b01,
    octal: 0o12,
    integerNegative: -3,
    stringSingleQuota: 'abc "def" ghi',
    stringDoubleQuota: "abc 'def' ghi",
    stringBacktick: `abc
def \`
xyz`,
    stringEscape1: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}',
    stringEscape2: "\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}",
    stringEscape3: `\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}`,
    arrayEmpty: [], 
    arrayEmptyMultiLine: [ @array
    ],
    arrayEmptyWithAnnotation: [],  // @array
    arraySimple: [ @array
        "a", @upper
        "b",
    ],
    arrayOnline: ["a", "b"], @array
    arrayExtraComma: ["a", "b",],
    objectEmpty: {},
    objectEmptyMultiLine: { @object
    },
    objectEmptyWithAnnotation: {}, @use("Object4")
    objectSimple: { @save("Object4")
        k1: "v1", @upper
        k2: "v2",
    },
    objectOneLine: { k1: "v1", k2: "v2" }, @object
    objectExtraComma: { k1: "v1", k2: "v2", },
}
```

## JSON

JSONA 是 JSON 的超集，借鉴 ECMAScript 的语法缓解 JSON 的一些限制。

### 支持注释

```
/**
 多行
*/
// 单行注释
{
  @anno /* 内联注释 */ @anno
}
```

### 属性名随意使用引号

```
{
  "a": 1,
  b: 2,
  'a': 3,
  `a`: 4,
}
```

### 允许多余尾逗号

```
{
  a: 3,
  b: 4,
  c: [
    'x',
    'y',
  ],
}
```

### 浮点数允许只有整数部分或小数部分

```
{
  a: 3.,
  b: .1,
  c: 3.1,
}
```

### 多种进制支持

```
{
  integer: 3,
  hex: 0x1a,
  binary: 0b01,
  octal: 0o12,
}
```

### 支持单引号和反引号

```
{
  x: 'abc "def" ghi',
  y: "abc 'def' ghi",
  z: `abc "def", 'ghi'`,
}
```


### 多行字符串

```
{
  x: `abc
  def`
}
```

### 转义字符串

```
{
  x: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}', // 单引号
  z: "\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}", // 双引号
  y: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}', // 反引号
}
```

## 注解

注解由 `@` 标记，接一个变量名。注解可以带值，也可以不带。

### 插入位置

下面列出了 JSONA 中所有注解的地方：

```
{ @anno 
  @anno
  v1: 1, @anno
  v2: {}, @anno
  v3: [], @anno
  v4: [ @anno
  ],
  v5: [
  ], @anno
  v6: { @anno
  },
  v7: {
  }, @anno
} @anno
@anno
```

### 注解值

注解值必须用圆括号包起来，但可以省略。

注解值必须是有效的但不带注解的JSONA，注解值不能嵌套注解值。

```
@anno
@anno(null)
@anno(true)
@anno('a')
@anno(3)
@anno(0.3)
@anno([])
@anno(['a'])
@anno({})
@anno({a:3})
```

## 相关项目

- [jsona](https://github.com/jsona/jsona.git) - JSONA 工具箱, 包括解析器、cli、lsp。
- [apitest](https://github.com/sigoden/apitest.git) - 自动化接口测试
- [jsona-openapi](https://github.com/sigoden/jsona-openapi) - openapi dsl
- [vscode-jsona-syntax](https://marketplace.visualstudio.com/items?itemName=sigoden.vscode-jsona) - vscode语法扩展