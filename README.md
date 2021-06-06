# JSONA

JSONA = JSON (less grammatical restrictions) + Annotation.

- [JSONA](#jsona)
  - [Motivation](#motivation)
  - [与JSON区别](#与json区别)
    - [支持注释](#支持注释)
    - [属性名可省略双引号](#属性名可省略双引号)
    - [允许多余尾逗号](#允许多余尾逗号)
    - [浮点数允许只有整数部分或小数部分](#浮点数允许只有整数部分或小数部分)
    - [多种进制支持](#多种进制支持)
    - [支持单引号和反引号](#支持单引号和反引号)
    - [多行字符串](#多行字符串)
    - [转义字符串](#转义字符串)
    - [注解](#注解)
  - [示例](#示例)

## Motivation

JSON is a very friendly format for exchanging and describing data, but it does not carry logic and calculations. Annotation is an elegant way to insert logic into JSON.

See the following example
```
{
  "login": {
    "route": "POST /login",
    "req": {
      "body": {
        "user": "Alice",
        "pass": "a123456"
      }
    },
    "res": {
      "200": {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6Ikp...",
      }
    }
  }
}
```

The data is rest api.

If we want to turn it into a Swagger/OpenAPI specification interface document, we can add the following annotations

```
{
  "login": { @endpoint({summary:"登录",security:[]})
    "route": "POST /login",
    "req": {
      "body": {
        "user": "Alice", @description("用户名")
        "pass": "a123456" @description("密码")
      }
    },
    "res": {
      "200": {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6Ikp..." @description("JWT令牌")
    }
  }
}
```

> [jsona-openapi](https://github.com/sigoden/jsona-openapi)　The above functions are implemented, and annotations are provided to convert an interface data description into an OpenApi document.


If I want to turn it into a test case, we can add the following annotation

```
{
  "login": { @describe("测试登录") @client("http")
    "route": "POST /login",
    "req": {
      "body": {
        "user": "Alice", 
        "pass": "a123456"
      }
    },
    "res": {
      "200": { @partial
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6Ikp...", @type
      }
    }
  }
}
```

> [apitest](https://github.com/sigoden/apitest) The above functions are implemented, and annotations are provided to treat an interface description as a test case.


The data is same. We have different understandings and purposes of the data, so we provide and implement different annotations.

Data and logic are splitted, and they coexist in a harmonious way.

Because JSON is a pure data exchange format, its grammatical requirements are very strict. JSONA is used for parsing engines that provide annotations, and the syntax does not need to be too strict.


## Difference from JSON

JSONA is a superset of JSON. JSONA has the following changes based on JSON

### Support comments

```
/**
 多行
*/
// 单行注释
{
  @anno /* 内链注释 */ @anno
}
```

### Omit double quotes on prop key

```
{
  "a": 1,
  b: 2,
}
```

### Allow extra trailing commas

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

### Floating point numbers allow only integer part or fractional part

```
{
  a: 3.,
  b: .3,
}
```

### Multiple bases support

```
{
  integer: 3,
  hex: 0x1a,
  binary: 0b01,
  otcal: 0o12,
}
```

### Support single quote and back quote

```
{
  x: 'abc "def" ghi',
  y: "abc 'def' ghi",
  z: `abc "def", 'ghi'`,
}
```


### Multi-line string

```
{
  x: `abc
  def`
}
```

### Escape string

```
{
  x: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}', // 单引号
  y: '\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}', // 反引号
  z: "\0\b\f\n\r\t\u000b\'\\\xA9\u00A9\u{2F804}", // 双引号
}
```


## Annotation

The annotation is marked with `@`, followed by a variable name.

### Position

- Except for `key: value,` and `value,` between colons and commas, comments can appear anywhere

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

### Parameters

Annotations can take parameters, and the parameters are JSONA without annotations. That is, annotations cannot be nested annotations.

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

## Example

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