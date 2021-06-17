# JSONA

Read this in other languages: [中文](./README.zh-CN.md)

JSONA = JSON (less grammatical restrictions) + Annotation.

- [JSONA](#jsona)
  - [Introduction](#introduction)
  - [Difference from JSON](#difference-from-json)
    - [Support comments](#support-comments)
    - [Free choose quota on prop key](#free-choose-quota-on-prop-key)
    - [Allow extra trailing commas](#allow-extra-trailing-commas)
    - [Omit part of floating point numbers](#omit-part-of-floating-point-numbers)
    - [Multiple bases support](#multiple-bases-support)
    - [Support single quote and back quote](#support-single-quote-and-back-quote)
    - [Multi-line string](#multi-line-string)
    - [Escape string](#escape-string)
  - [Annotation](#annotation)
    - [Position](#position)
    - [Parameters](#parameters)
  - [Example](#example)
  - [Related Project](#related-project)


## Introduction

JSON is a very friendly format for exchanging and describing data, but it does not carry logic and calculations. Annotation is an elegant way to insert logic into JSON. see [motivation](docs/motivation.md)

Data and logic are splitted, and they coexist in a harmonious way.

Because JSON is a pure data exchange format, its grammatical requirements are very strict. JSONA is used for parsing engines that provide annotations, and the syntax does not need to be too strict.


## Difference from JSON

JSONA is a superset of JSON. JSONA has the following changes based on JSON

### Support comments

```
/**
 multiple lines comment
*/
// single line  comment
{
  @anno /* inline comment */ @anno
}
```

### Free choose quota on prop key

```
{
  "a": 1,
  b: 2,
  'a': 3,
  `a`: 4,
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

### Omit part of floating point numbers

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

Where you can insert comments, you can insert annotations.

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

## Related Project

- [apitest](https://github.com/sigoden/apitest.git) - declarative api testing tool.
- [jsona-openapi](https://github.com/sigoden/jsona-openapi) - write openapi in jsona.
- [vscode-jsona](https://marketplace.visualstudio.com/items?itemName=sigoden.vscode-jsona) - vscode extension for jsona.