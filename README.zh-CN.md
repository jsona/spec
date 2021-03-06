# JSONA

其他语言版本: [English](./README.md)


- [JSONA](#jsona)
  - [介绍](#介绍)
  - [与JSON区别](#与json区别)
    - [支持注释](#支持注释)
    - [属性名随意使用引号](#属性名随意使用引号)
    - [允许多余尾逗号](#允许多余尾逗号)
    - [浮点数允许只有整数部分或小数部分](#浮点数允许只有整数部分或小数部分)
    - [多种进制支持](#多种进制支持)
    - [支持单引号和反引号](#支持单引号和反引号)
    - [多行字符串](#多行字符串)
    - [转义字符串](#转义字符串)
  - [注解](#注解)
    - [位置](#位置)
    - [参数](#参数)
  - [示例](#示例)
  - [相关项目](#相关项目)

## 介绍

JSONA = JSON(更少语法限制) + Annotation(注解)。

JSON是一种很友好的描述数据的格式，但它不承载逻辑和计算。 而Annotation就是一种优雅插入逻辑到JSON的方式。

数据和逻辑是对立的，有以一种和谐的方式共存。

因为JSON是一种纯数据交换的格式，所以它的语法要求很严厉。JSONA是给提供注解的解析引擎使用的，语法不需要太严厉。


## 与JSON区别

JSONA 是JSON的超集。JSONA 在JSON的基础上，有如下改动

### 支持注释

```
/**
 多行
*/
// 单行注释
{
  @anno /* 内链注释 */ @anno
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
  b: .3,
}
```

### 多种进制支持

```
{
  integer: 3,
  hex: 0x1a,
  binary: 0b01,
  otcal: 0o12,
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
  y: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}', // 反引号
  z: "\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}", // 双引号
}
```


## 注解

注解有`@`标记，接一个变量名。

### 位置

能插入注释的地方，都能插入注解

```
@anno
{ @anno 
  @anno
  v1: 1, @anno
  v2: {}, @anno
  v3: [], @anno
  v4: [ @anno
  ],
  v5: [
    @anno
  ],
  v6: [
  ], @anno
} @anno
@anno
```

### 参数

注解可以带参数，其参数是不带注解的JSONA。即注解不能嵌套注解。

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

## 示例

```
// single line comment

{
    @foo /* abc */ @optional
    @null(null) // single line comment
    @bool(true)
    @float(3.14)
    @number(-3)
    @string('abc "def" ghi')
    @array([3,4])
    @object({k: "v"})

    nullValue: null,
    boolTrue: true,
    boolFale: false,
    float: 3.14,
    floatNegative: -3.14,
    floatNegativeWithoutInteger: -.14,
    floatNegativeWithoutDecimal: -3.,
    integer: 3,
    hex: 0x1a,
    binary: 0b01,
    otcal: 0o12,
    integerNegative: -3,
    stringSingleQuota: 'abc "def" ghi',
    stringDoubleQuota: "abc 'def' ghi",
    stringBacktick: `abc
def \`
xyz`,
    stringEscaple1: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}',
    stringEscaple2: "\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}",
    stringEscaple3: `\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}`,
    arrayEmpty: [], 
    arrayEmptyMultiLine: [ @array
    ],
    arrayEmptyWithAnnotation: [],  // @array
    arraySimple: [ @array
        "a", @upper
        "b",
    ],
    arrayOneline: ["a", "b"], @array
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

## 相关项目

- [apitest](https://github.com/sigoden/apitest.git) - 自动化测试工具
- [jsona-openapi](https://github.com/sigoden/jsona-openapi) - openapi dsl
- [vscode-jsona](https://marketplace.visualstudio.com/items?itemName=sigoden.vscode-jsona) - vscode扩展