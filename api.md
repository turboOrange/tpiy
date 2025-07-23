# API calls

This page document all the API calls for a tpiy implementation. tpiy uses graphQL to comunicate to comunicate. There is then only one endpoint `/graphql`. And all those calls can be made from there:

Note that for registration and login, other methondes might be added to the specification but we decided to go with token based email regitstration for now.

## query examples

### register
query: 
```
mutation {
  loginUser(input: {
    email: String,
    password: String
  }) {
    token
    user {
      name
    }
  }
}
```

Output:
```
{
  "data": {
    "loginUser": {
      "token": String
      "user": {
        "name": String
      }
    }
  }
}
```

### login

query:
```
mutation {
  loginUser(input: {
    email: String,
    password: String
  }) {
    token
    user {
      name
    }
  }
}
```

Output:
```
{
  "data": {
    "loginUser": {
      "token": String
      "user": {
        "name": String
      }
    }
  }
}
```

### get list of passwords with simple info

query:
```
query {
  getPasswordEntryList(page: 1, pageSize: 10) {
    items {
      name
      icon
      description
    }
    totalCount
    page
    pageSize
    hasNextPage
  }
}

```

Output:
```
{
  "data": {
    "getPasswordEntry": {
      "items": [
        {
          "name": String,
          "icon": String,
          "description": String
          "id": number
        }
      ],
      "totalCount": Int,
      "page": Int,
      "pageSize": Int,
      "hasNextPage": Bool
    }
  }
}

```

### get password information by ID

```
query {
  getPasswordById(id: Int) {
    name
    username
    password
    description
    tfa
  }
}
```

## object types

```
type PasswordEntry {
  id: ID!
  name: String!
  username: String!
  password: String!
  description: String
  twoFA: String
  icon: String
}

type PaginatedPasswordEntries {
  items: [PasswordEntry!]!
  totalCount: Int!
  page: Int!
  pageSize: Int!
  hasNextPage: Boolean!
}

type AuthPayload {
  token: String!
  user: User!
}

type User {
  id: ID!
  name: String!
  email: String!
}

input RegisterInput {
  name: String!
  email: String!
  password: String!
}

input LoginInput {
  email: String!
  password: String!
}

type Query {
  getPasswordEntry(page: Int = 1, pageSize: Int = 20): PaginatedPasswordEntries!
  getPasswordById(id: ID!): PasswordEntry
}

type Mutation {
  registerUser(input: RegisterInput!): AuthPayload!
  loginUser(input: LoginInput!): AuthPayload!
}

```
