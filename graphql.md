### What is GraphQL?



### Queries

#### Root Queries

1. Is a entry point into our data. 
2. The most important part of the query is the resolve part. It returns a node value of the object type.

```js
const RootQuery = new GraphQLObjectType({
  name: "RootQueryType",
  fields: {
    user: {
      type: UserType,
      args: { id: { type: GraphQLString } },
      resolve(parentValue, args){
            // go into datastore and return it
            
      }
    },
  },
});
```

**GraphQLSchema** - takes a root query and returns a graphql schema instance

```js
module.exports = new GraphQLSchema({
  query: RootQuery
});
```



3. The resolve function within our root query returns a raw javascript object. GraphQL takes the JSON object and only returns the key's that match the fields in the schema. So in the example above we are requesting a user object. If the data we resolve has an other key/values besides id, firstName, and age it will be ignored.

Note: The resolve function uses a Promise to return data to the server.

```js
const RootQuery = new GraphQLObjectType({
  name: "RootQueryType",
  fields: {
    user: {
      type: UserType,
      args: { id: { type: GraphQLString } },
      resolve(parentValue, args) {
        // go into the data store and return value
        return axios
          .get(`http://localhost:3000/users/${args.id}`)
          .then((res) => res.data);
      },
    },
  },
});
```



4. Schema's tell GraphQL what our data looks like. It's like a blue print.

```js
const graphql = require("graphql");
const { GraphQLObjectType, GraphQLString, GraphQLInt } = graphql;

const UserType = new GraphQLObjectType({
  name: "User",
  fields: {
    id: GraphQLString,
    firstName: GraphQLString,
    age: GraphQLInt,
  },
});

```

### Associating Types

1. Different types of data can be associated together by passing one object type as the field inside of another object type. In the example below each User has a Company which is another graphQL object.

```js

const CompanyType = new GraphQLObjectType({
  name: "Company",
  fields: {
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString },
  },
});

const UserType = new GraphQLObjectType({
  name: "User",
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt },
    company: {
      type: CompanyType,
    },
  },
});
```

