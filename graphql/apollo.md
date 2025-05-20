# Apollo

## Apollo Client
Apollo Client is a powerful JavaScript library that enables you to manage both local and remote data with GraphQL. It is designed to work seamlessly with React, Angular, Vue, and other JavaScript frameworks. Apollo Client provides a set of tools for fetching, caching, and managing data in your application.

### Installation
To install Apollo Client, you can use npm, yarn, pnpm or bun. Here are the commands for both package managers:

```bash
# Using npm
npm install @apollo/client graphql
# Using yarn
yarn add @apollo/client graphql
# Using pnpm
pnpm add @apollo/client graphql
# Using bun
bun add @apollo/client graphql
```
### Basic Setup
To set up Apollo Client in your application, you need to create an instance of `ApolloClient` and provide it with a GraphQL endpoint. You can also configure caching and other options.

#### HTTP Link Setup
Create a standard HTTP link to your GraphQL server. This will handle queries and mutations.

```javascript
import { createHttpLink } from '@apollo/client';

const authLink = new ApolloLink((operation, forward) => {
  const accessToken = getAccessToken(); // Function to get the access token
  if (accessToken) {
    operation.setContext({
      headers: { 'Authorization': `Bearer ${accessToken}` }, // Set the authorization header
    });
  }
  return forward(operation);
});

const httpLink = concat(authLink, createHttpLink({
  uri: 'http://localhost:9000/graphql'
}));
```
#### WebSocket Link for Subscriptions
To enable subscriptions, use graphql-ws for WebSocket-based connections.

```javascript
import { GraphQLWsLink } from '@apollo/client/link/subscriptions';
import { createClient as createWsClient } from 'graphql-ws';
import { getAccessToken } from '../auth';

const wsLink = new GraphQLWsLink(createWsClient({
  url: 'ws://localhost:9000/graphql', // Replace with your WebSocket URL
  connectionParams: () => ({ accessToken:   () }), // Add your token here
}));
```

#### Linking HTTP and WebSocket Links

Use the split function to determine which link (HTTP or WebSocket) to use based on the operation type.

```javascript
import { ApolloClient, InMemoryCache, split } from '@apollo/client';
import { getMainDefinition } from '@apollo/client/utilities';
import { Kind, OperationTypeNode } from 'graphql';

function isSubscription(operation) {
  const definition = getMainDefinition(operation.query);
  return definition.kind === Kind.OPERATION_DEFINITION
    && definition.operation === OperationTypeNode.SUBSCRIPTION;
}

const apolloClient = new ApolloClient({
  link: split(isSubscription, wsLink, httpLink),
  cache: new InMemoryCache(),
});
```

---

```javascript
// client.js
import { ApolloClient, ApolloLink, concat, createHttpLink, InMemoryCache, split } from '@apollo/client';
import { GraphQLWsLink } from '@apollo/client/link/subscriptions';
import { getMainDefinition } from '@apollo/client/utilities';
import { Kind, OperationTypeNode } from 'graphql';
import { createClient as createWsClient } from 'graphql-ws';
import { getAccessToken } from '../auth';

const authLink = new ApolloLink((operation, forward) => {
  const accessToken = getAccessToken();
  if (accessToken) {
    operation.setContext({
      headers: { 'Authorization': `Bearer ${accessToken}` },
    });
  }
  return forward(operation);
});

const httpLink = concat(authLink, createHttpLink({
  uri: 'http://localhost:9000/graphql',
}));

const wsLink = new GraphQLWsLink(createWsClient({
  url: 'ws://localhost:9000/graphql',
  connectionParams: () => ({ accessToken: getAccessToken() }),
}));

function isSubscription(operation) {
  const definition = getMainDefinition(operation.query);
  return definition.kind === Kind.OPERATION_DEFINITION
    && definition.operation === OperationTypeNode.SUBSCRIPTION;
}

const apolloClient = new ApolloClient({
  link: split(isSubscription, wsLink, httpLink),
  cache: new InMemoryCache(),
});
```

**Example**

```javascript
// Queries & Mutations
import { gql } from '@apollo/client';

const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      name
      email
    }
  }
`;

apolloClient
  .query({ query: GET_USER, variables: { id: '123' } })
  .then(result => console.log(result));
```
```javascript
// Subscriptions
import { gql } from '@apollo/client';

const USER_ADDED = gql`
  subscription OnUserAdded {
    userAdded {
      id
      name
    }
  }
`;

const subscription = apolloClient.subscribe({
  query: USER_ADDED,
}).subscribe({
  next(data) {
    console.log('New user added:', data);
  },
  error(err) {
    console.error('Subscription error:', err);
  },
});
```

