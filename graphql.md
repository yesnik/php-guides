# GraphQL

[GraphQL](https://graphql.org/) is a query language for APIs and a runtime for fulfilling those queries with your existing data. 
It gives clients the power to ask for exactly what they need.

## Implementations on PHP

- https://github.com/thecodingmachine/graphqlite
- https://github.com/webonyx/graphql-php

## Query examples

```php
query {
  product(id: "2000") {
    name
    price
  }
}
```

## Mutation example

```php
mutation {  
  saveProduct(name: "Symfony book", price: 123.2) {
    name
    price
  }
}
```
