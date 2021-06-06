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
  "login": { @endpoint({summary:"login",security:[]})
    "route": "POST /login",
    "req": {
      "body": {
        "user": "Alice", @description("user name")
        "pass": "a123456" @description("user pass")
      }
    },
    "res": {
      "200": {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6Ikp..." @description("JWT token")
    }
  }
}
```

> [jsona-openapi](https://github.com/sigoden/jsona-openapi)　The above functions are implemented, and annotations are provided to convert an interface data description into an OpenApi document.


If I want to turn it into a test case, we can add the following annotation

```
{
  "login": { @describe("test login") @client("http")
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