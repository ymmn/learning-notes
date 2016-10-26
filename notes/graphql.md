# GraphQL

## Overview

#### What is it?
It's a [specification](https://facebook.github.io/graphql/) for a **Query Language** (hence the QL), and an **execution engine**. So GraphQL isn't a piece of code itself, but there are multiple implementation of it in [Javascript](https://github.com/graphql/graphql-js) and other languages.

#### Why was it built?
Facebook built it so mobile clients could fetch all the required data in one network roundtrip. So mainly performance.

## Terminology

### Type
This is "type" as in "statically typed language". Everything in GraphQL has a `type`.

Something can be a built-in `type`, like `Int` or `String`, or a custom `type`, like `User` or `Delivery`.

### [Object Type](http://graphql.org/learn/schema/#object-types-and-fields)
Following the Java analogy above, it's the `type` of all custom defined `object`s in GraphQL.

basically has a `name`, `fields`, and `description`. defined like this:
```
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

### [`Query` and `Mutation` types](http://graphql.org/learn/schema/#the-query-and-mutation-types)
These types are plain old objects.

> they define the entry point of every GraphQL query

So basically, they're like a `main` function, in that their name makes them special. They're used to define the **schema** (see definition below).

Note that these names are just convention. A schema can be defined with an object of any `type`, not necessarily `Query` and `Mutations`.

### [Field](http://graphql.org/learn/queries/#fields)
It's basically like a function. Its normal usage is without arguments, e.g. while selecting fields from an object, but it can also have `arguments`.

When you define mutations on your schema, it looks something like:
```
type Mutations {
  CreateUser(name: String): User
}
```

Note that your mutation `CreateUser` is simply a **`field` with arguments** (this actually isn't very obvious if you use [graphene](http://graphene-python.org/)).

### Schema
This is what defines the kinds of queries and mutations that are possible on your endpoint. Carries the `Query` and `Mutations` types, and can be queried thru the [introspection API](http://graphql.org/learn/introspection/)


### Kind (`__TypeKind`)
This is used to describe implementation details of a `type`. It's easiest for me to understand with a Java analogy:

What's an `int`? It's a primitive. What's a `float`? It's a primitive. What's a `User`? It's an object.

Just like how Java has "primitives" and "objects", a GraphQL `type` is one of the following:
- `SCALAR`, `ENUM`, `NON_NULL`, `LIST`, `OBJECT`, `UNION`, `INPUT_OBJECT`, and `INTERFACE`

Side note: `LIST` and `NON_NULL` are special type modifiers, in that, they wrap other types. You can see it in the syntax of the language too:
```
name: String!           # NON_NULL is just a "!"
appearsIn: [Episode]    # LIST is just the wrapping braces

# what it looks like in introspection:
{
  "name": "name",
  "type": {
    "name": null,
    "kind": "NON_NULL",
    "ofType": {
      "name": "String",
      "kind": "SCALAR"
    }
  }
}
```

### [Operation](https://facebook.github.io/graphql/#sec-Language.Operations)

> There are two types of operations that GraphQL models:
> * query – a read‐only fetch.
> * mutation – a write followed by a fetch.

This is basically the executable unit. A **query document** (which is often times a request string) can have multiple operations.

Note that a query / mutation can be parametrized:
```
query Hero($episode: Episode) {
  hero(episode: $episode) {
    name
  }
}
```

So in this case, the query is *not* an operation, at least not until it's provided the parameters.

One interesting thing is, a parametrized query is essentially like defining your own custom API endpoint client side!

## Resources
- [GraphQL specification](https://facebook.github.io/graphql), it's actually super approachable!
- [Lee Byron on the history of GraphQL](https://www.youtube.com/watch?v=Oh5oC98ztvI)
