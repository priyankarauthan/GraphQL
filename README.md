# GraphQL


## 1. What is GraphQL?

GraphQL is a query language and runtime for APIs that allows clients to request exactly the data they need.

### Main advantages:

Flexible queries  
Reduced over-fetching  
Single endpoint  
Strong typing  
## 2. Why use GraphQL instead of REST?

Advantages over REST:

Exact data fetching
Fewer network calls
Strongly typed schema
Better frontend flexibility
Easier API evolution
## 3. What are the main components of GraphQL?

Main components:

Schema
Types
Queries
Mutations
Subscriptions
Resolvers

## 4. What is Query and Mutation in GraphQL?

In GraphQL:

READ → Query
CREATE/UPDATE/DELETE → Mutation

```

type User {
    id: ID!
    name: String!
    age: Int!
}

type Query {
    getUser(id: ID!): User
    getAllUsers: [User]
}

type Mutation {
    createUser(input: UserInput!): User
    updateUser(id: ID!, input: UserInput!): User
    deleteUser(id: ID!): String
}

input UserInput {
    name: String!
    age: Int!
}
```
## 5. What is Query? How do we fetch or get data in graphQL?
Query Section (READ)

```
type Query {
    getUser(id: ID!): User
    getAllUsers: [User]
}
```
## 6. What is Mutation ?

```
type Mutation {
    createUser(input: UserInput!): User
    updateUser(id: ID!, input: UserInput!): User
    deleteUser(id: ID!): String
}
```

Used for:

create
update
delete

## 7. Show an example of GraphQL create operation/mutation?

```
mutation {
  createUser(input: {
    name: "Priyanka",
    age: 25
  }) {
    id
    name
    age
  }
}
```

Response
```
{
  "data": {
    "createUser": {
      "id": "1",
      "name": "Priyanka",
      "age": 25
    }
  }
}
```

## 8. What are Resolvers?

Resolvers are methods/functions that fetch the actual data for GraphQL fields.

In Spring Boot:
```
@QueryMapping
public List<Employee> employees() {
    return service.getAllEmployees();
}
```

```
@MutationMapping
public User createUser(@Argument UserInput input) {

    User user = new User();
    user.setName(input.getName());
    user.setAge(input.getAge());

    return userRepository.save(user);
}
```

## 9. What is Subscription in GraphQL?

Subscriptions provide real-time updates using WebSockets.

Example use cases:

Chat applications
Live stock prices
Notifications

## 10. What problem does GraphQL solve?
Over-fetching
Under-fetching
Multiple API calls
API evolution issues

## 11.What are GraphQL Fragments?

Fragments allow reuse of query fields.

Example:
```
fragment EmployeeFields on Employee {
   id
   name
}

query {
   employees {
      ...EmployeeFields
   }
}
```

## 12. What are Variables in GraphQL?

Variables allow dynamic values in queries.

Example:
```
query GetEmployee($id: ID!) {
   employee(id: $id) {
      name
   }
}
```

## Flow
```
Client Mutation
↓
Resolver
↓
Database Insert
↓
Response Returned
```

## Step 3: READ Operation

Fetch user data.

Get Single User
Query

```
query {
  getUser(id: 1) {
    id
    name
    age
  }
}
```
Response:-

```
{
  "data": {
    "getUser": {
      "id": "1",
      "name": "Priyanka",
      "age": 25
    }
  }
}
```
## Resolver 
```
@QueryMapping
public User getUser(@Argument Long id) {
    return userRepository.findById(id).orElse(null);
}
```
## Get All Users
Query
```
query {
  getAllUsers {
    id
    name
    age
  }
}
```
## Resolver
```
@QueryMapping
public List<User> getAllUsers() {
    return userRepository.findAll();
}
```

## Step 4: UPDATE Operation

Modify existing user.

Mutation
```
mutation {
  updateUser(
    id: 1,
    input: {
      name: "Priya",
      age: 26
    }
  ) {
    id
    name
    age
  }
}
```
Response
```
{
  "data": {
    "updateUser": {
      "id": "1",
      "name": "Priya",
      "age": 26
    }
  }
}
```
## Resolver

```
@MutationMapping
public User updateUser(@Argument Long id,
                       @Argument UserInput input) {

    User user = userRepository.findById(id)
                    .orElseThrow();

    user.setName(input.getName());
    user.setAge(input.getAge());

    return userRepository.save(user);
}
```
## Step 5: DELETE Operation

Remove user.

Mutation
```
mutation {
  deleteUser(id: 1)
}
```

Response  
```
{
  "data": {
    "deleteUser": "User deleted successfully"
  }
}
```
## Resolver
```
@MutationMapping
public String deleteUser(@Argument Long id) {

    userRepository.deleteById(id);

    return "User deleted successfully";
}
```
## Complete Spring Boot Flow
```
Frontend / Postman / GraphiQL
↓
GraphQL Query
↓
Spring GraphQL Controller
↓
Resolver Method
↓
Service Layer
↓
Repository
↓
Database
```
## Explain all GraphQL Annotations

| Annotation             | Purpose                      |
| ---------------------- | ---------------------------- |
| `@Controller`          | Marks GraphQL controller     |
| `@QueryMapping`        | Handles query operations     |
| `@MutationMapping`     | Handles mutation operations  |
| `@SubscriptionMapping` | Handles subscriptions        |
| `@SchemaMapping`       | Resolves nested fields       |
| `@Argument`            | Reads GraphQL arguments      |
| `@BatchMapping`        | Batch field resolution       |
| `@ContextValue`        | Access GraphQL context       |
| `@ProjectedPayload`    | Projection interface support |


## What is a GraphQL Resolver?

In GraphQL, a resolver is:

A Java method/function that fetches data for a GraphQL query, mutation, or field.

Simple meaning:

Resolvers connect:

GraphQL schema
to
actual Java code/business logic



