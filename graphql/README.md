# GraphQL

## Table of Contents
- [GraphQL](#graphql)
  - [Table of Contents](#table-of-contents)
  - [What Is GraphQL?](#what-is-graphql)
  - [Differences between GraphQL and REST](#differences-between-graphql-and-rest)
  - [Schemas and Types](#schemas-and-types)
    - [Query](#query)
    - [Mutation](#mutation)
    - [Subscription](#subscription)
    - [Types](#types)
  - [Query](#query-1)
  - [Mutation](#mutation-1)
  - [Subscription](#subscription-1)
  - [Autharization In GraphQL](#autharization-in-graphql)


---

## What Is GraphQL?
GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. It provides a more efficient, powerful, and flexible alternative to the REST API. GraphQL is often used in conjunction with React, but it can be used with any programming language or framework.

## Differences between GraphQL and REST
1. **Data Fetching**: In REST, you have to make multiple requests to different endpoints to fetch related data. In GraphQL, you can fetch all the data you need in a single request.
    eg: In REST, you might have to make separate requests to `/users` and `/posts` to get user data and their posts. In GraphQL, you can fetch both in a single request `/graphql`.
2. **Over-fetching and Under-fetching**: In REST, you often get more data than you need (over-fetching) or not enough data (under-fetching). In GraphQL, you can specify exactly what data you need, so you only get the data you want.
3. **Versioning**: In REST, you often have to create new versions of your API when you make changes. In GraphQL, you can add new fields and types without affecting existing queries, so you don't need to version your API.
4. **Strongly Typed**: GraphQL is strongly typed, meaning that the API schema is defined in a way that allows clients to know exactly what data they can expect. This makes it easier to work with and reduces the chances of errors.
5. **Real-time Data**: GraphQL supports real-time data through subscriptions, allowing clients to receive updates when data changes. REST typically requires polling or webhooks for real-time updates.
6. **Introspection**: GraphQL APIs are self-documenting. You can query the API for its schema and get information about the types, queries, and mutations available. This makes it easier to understand and work with the API.
7. **Single Endpoint**: In REST, you often have multiple endpoints for different resources. In GraphQL, you typically have a single endpoint (e.g., `/graphql`) that handles all queries and mutations. This simplifies the API structure and reduces the number of requests needed to fetch data.
8. **Batching and Caching**: GraphQL allows for batching multiple requests into a single request, reducing the number of network calls. It also supports caching at the field level, allowing for more efficient data retrieval.
9. **Tooling and Ecosystem**: GraphQL has a rich ecosystem of tools and libraries that make it easier to work with, including client libraries (e.g., Apollo Client, Relay) and server libraries (e.g., Apollo Server, GraphQL.js). These tools provide features like caching, optimistic UI updates, and more.
10. **Error Handling**: In REST, error handling is often done through HTTP status codes. In GraphQL, errors are returned in the response body, allowing for more detailed error messages and better handling of partial success.
11. **Documentation**: GraphQL APIs are self-documenting, meaning that the schema can be introspected to generate documentation automatically. This makes it easier for developers to understand the API and its capabilities.


---

## Schemas and Types
GraphQL APIs are defined by a schema that specifies the types of data that can be queried and the relationships between those types. The schema is written in a language called GraphQL Schema Definition Language (SDL). The schema defines the types, queries, mutations, and subscriptions that are available in the API.

### Query
A query is a read-only operation that retrieves data from the API. Queries are defined in the schema and can take arguments to filter or modify the data returned. Queries can also return nested data, allowing you to fetch related data in a single request.
### Mutation
A mutation is a write operation that modifies data in the API. Mutations are defined in the schema and can take arguments to specify the data to be modified. Mutations can also return data, allowing you to get the updated data after the mutation is performed.
### Subscription
A subscription is a real-time operation that allows clients to receive updates when data changes. Subscriptions are defined in the schema and can take arguments to filter the data received. Subscriptions are typically used for real-time applications, such as chat applications or live updates.

### Types
Types are the building blocks of a GraphQL schema. They define the shape of the data that can be queried or mutated. There are several built-in types in GraphQL, including:
- **Scalar Types**: These are the basic types in GraphQL, including `Int`, `Float`, `String`, `Boolean`, and `ID`. Scalar types represent a single value and cannot be further divided.
- **Nullable Types**: These are types that can be null. Nullable types are defined without the `!` symbol in the schema. For example, `String` is a nullable string type that can be null.
- **Non-nullable Types**: These are types that cannot be null. Non-nullable types are defined using the `!` symbol in the schema. For example, `String!` is a non-nullable string type that cannot be null.
- **Object Types**: These are complex types that represent a collection of fields. Object types can contain other object types, scalar types, or lists of types. Object types are defined using the `type` keyword in the schema.
- **Array**: These are special types that represent a list of values. Array types can contain any type, including scalar types, object types, or other array types. Array types are defined using square brackets `[]` in the schema.
- **Input Types**: These are special types that are used as arguments for queries and mutations. Input types are similar to object types but cannot contain fields that return other object types. Input types are defined using the `input` keyword in the schema.
- **Enum Types**: These are special types that represent a fixed set of values. Enum types are defined using the `enum` keyword in the schema and can be used as fields or arguments in queries and mutations.
- **Interface Types**: These are abstract types that define a set of fields that must be implemented by other object types. Interface types are defined using the `interface` keyword in the schema and can be used to create polymorphic queries.
- **Union Types**: These are special types that represent a value that can be one of several different object types. Union types are defined using the `union` keyword in the schema and can be used to create flexible queries.


**Example**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  content: String!
  user: User!
}
type Query {
  users: [User!]!
  user(id: ID!): User
  posts: [Post!]!
  post(id: ID!): Post
}
```

---

## Query
A query is a read-only operation that retrieves data from the API. Queries are defined in the schema and can take arguments to filter or modify the data returned. Queries can also return nested data, allowing you to fetch related data in a single request.
**Example**
```graphql
query {
  users {
    id
    name
    email
    posts {
      id
      title
      content
    }
  }
}
```
**Query for List**

```graphql
type User {
  id: ID!
  name: String!
  email: String!
}

type Query {
  users: [User!]!
}

resolver {
  Query: {
    users: () => {
      return [
        { id: 1, name: "John Doe", email: "johndoe@gmail.com" }
      ]
    }
  }
}
```

```graphql
query {
  users {
    id
    name
    email
  }
}
```


**Query for Single Item**
```graphql
type User {
	id: ID!
	name: String!
	email: String!
}
type Query {
	user(id: ID!): User
}
resolver {
	Query: {
		user: (parent, args) => {
			return { id: args.id, name: "John Doe", email: "johndoe@gmail.com" }
		}
	}
}
```

```graphql
query {
	user(id: 1) {
		id
		name
		email
	}
}
```

*With Variables*
```graphql
query getUser($id: ID!) {
	user(id: $id) {
		id
		name
		email
	}
}
```
```json
{
  "query": "query getUser($id: ID!) { user(id: $id) { id name email } }",
  "variables": {
	"id": 1
  }
}
```
---

## Mutation
A mutation is a write operation that modifies data in the API. Mutations are defined in the schema and can take arguments to specify the data to be modified. Mutations can also return data, allowing you to get the updated data after the mutation is performed.

**Example**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
}
type Mutation {
  createUser(name: String!, email: String!): User!
  updateUser(id: ID!, name: String, email: String): User!
  deleteUser(id: ID!): User!
}
resolver {
  Mutation: {
	createUser: (parent, args) => {
	  return { id: 1, name: args.name, email: args.email }
	},
	updateUser: (parent, args) => {
	  return { id: args.id, name: args.name, email: args.email }
	}
}
  deleteUser: (parent, args) => {
    return { id: args.id }
  }
}
```

**Input Types**
Input types are special types that are used as arguments for queries and mutations. Input types are similar to object types but cannot contain fields that return other object types. Input types are defined using the `input` keyword in the schema.
```graphql
input UserInput {
  name: String!
  email: String!
}
type Mutation {
  createUser(input: UserInput!): User!
  updateUser(id: ID!, input: UserInput!): User!
}
```
```graphql
mutation {
  createUser(input: { name: "John Doe", email: "johndoe@gmail.com" }) {
    id
    name
    email
  }
}
```

*With Variables*
```graphql
mutation createUser($input: UserInput!) {
  createUser(input: $input) {
    id
    name
    email
  }
}
```
```json
{
  "query": "mutation createUser($input: UserInput!) { createUser(input: $input) { id name email } }",
  "variables": {
  "input": {
    "name": "John Doe",
    "email": "johndoe@gmail.com"
  }
  }
}
```
---

## Subscription

A subscription is a real-time operation that allows clients to receive updates when data changes. Subscriptions are defined in the schema and can take arguments to filter the data received. Subscriptions are typically used for real-time applications, such as chat applications or live updates.

**Example**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
}
type Mutation {
  createUser(name: String!, email: String!): User!
}
type Subscription {
  userCreated: User!
}
resolver {
  Subscription: {
    userCreated: {
      subscribe: () => pubsub.asyncIterator("USER_CREATED")
    }
  },
  Mutation: {
    createUser: (parent, args) => {
      const user = { id: 1, name: args.name, email: args.email }
      pubsub.publish("USER_CREATED", { userCreated: user })
      return user
    }
  }
}
```
```graphql
subscription {
  userCreated {
    id
    name
    email
  }
}
```

## Autharization In GraphQL
Authorization in GraphQL is the process of determining whether a user has permission to access a specific resource or perform a specific action. This is typically done by checking the user's authentication token and verifying their roles and permissions.
**Example**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
}
type Query {
  users: [User!]!
  user(id: ID!): User
  posts: [Post!]!
  post(id: ID!): Post
}

type Mutation {
  createUser(name: String!, email: String!): User!
  updateUser(id: ID!, name: String, email: String): User!
  deleteUser(id: ID!): User!
}
type Subscription {
  userCreated: User!
}
resolver {
  Query: {
    users: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return [
        { id: 1, name: "John Doe", email: "johndoe@gmail.com" }
      ]
    },
    user: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return { id: args.id, name: "John Doe", email: "johndoe@gmail.com" }
    },
    posts: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return [
        { id: 1, title: "Post 1", content: "Content 1" },
        { id: 2, title: "Post 2", content: "Content 2" }
      ]
    },
    post: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return { id: args.id, title: "Post 1", content: "Content 1" }
    }
  },
  Mutation: {
    createUser: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return { id: 1, name: args.name, email: args.email }
    },
    updateUser: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return { id: args.id, name: args.name, email: args.email }
    },
    deleteUser: (parent, args, context) => {
      if (!context.user) {
        throw new Error("Unauthorized")
      }
      return { id: args.id }
    }
  },
  Subscription: {
    userCreated: {
      subscribe: (_root, _args, context) => {
        if (!context.user) {
          throw new Error("Unauthorized")
        }
        return pubsub.asyncIterator("USER_CREATED")
      }
    }
  }
}
```