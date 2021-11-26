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

3. The resolve function within our root query returns a raw javascript object. GraphQL takes the JSON object and only returns the key's that match the fields in the schema. So in the example above we are requesting a user object. If the data we resolve has an other key/values besides id, firstName, and age it will be ignored.

