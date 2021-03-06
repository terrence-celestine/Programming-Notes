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

### Multiple Root Query Types

1. A graphQL root query can have multiple entry points.

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
    company: {
      type: CompanyType,
      args: { id: { type: GraphQLString } },
      resolve(parentValue, args) {
        return axios
          .get(`http://localhost:3000/companies/${args.id}`)
          .then((res) => res.data);
      },
    },
  },
});
```

### Two-way relations

1. Queries in graphQL can have nested fields of other object types. A company can have a field called users which has the type of a GraphQLObject. 
2. Use a list if there are more then 1 object that can be matched to an object.

```js
const CompanyType = new GraphQLObjectType({
  name: "Company",
  fields: {
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString },
    users: {
      type: new GraphQLList(UserType),
      resolve(parentValue, args) {
        return axios
          .get(`http://localhost:3000/companies/${parentValue.id}/users`)
          .then((res) => res.data);
      },
    },
  },
});
```

### Query Fragments

1. Query fragments are useful when you want to re-use a query in multiple places. You define the fragment by first naming it and passing the type of Object the query should be applied to. GraphQL checks if the fields the fragment's need are apart of the object before returning them.

```js
{
  company(id: "1") {
    ...companyDetails
  }
}

fragment companyDetails on Company{
  id
  name
  description
}
```

### Mutations

1. Mutations are used to modify data from our data store.
2. Mutations are a different type of query that takes a field with the name of the mutation, along with data needed to complete the mutation.

```js
const mutation = new GraphQLObjectType({
  name: "Mutation",
  fields: {
    addUser: {
      type: UserType, // the type of data we will return from the resolve
      args: {
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
        companyId: { type: GraphQLString },
      },
      resolve() {},
    },
  },
});
```

### GraphQLNonNull

1. Can be used to go around a field type. It makes sure that the value passed to a mutation is valid and not empty.

### Delete Mutation

### Edit Mutation

1. Patch requests only update parts of the object.
2. Put request updates the entire object.
3. Args are passed from the query into the patch request.

```js
// query syntax
mutation {
  editUser(id:"23", age: 10){
    id
    firstName
    age
  }
}
// query syntax
editUser: {
      type: UserType,
      args: {
        id: { type: new GraphQLNonNull(GraphQLString) },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
        companyId: { type: GraphQLString },
      },
      resolve(parentValue, args) {
        return axios
          .patch(`http://localhost:3000/users/${args.id}`, args)
          .then((res) => res.data);
      },
}
```

### Apollo vs Relay

1. Apollo 

   1.  produced by the same guy as Meteor JS.
   2. Good balance between features and complexity.
2. Relay

   1. Amazing performance for mobile.
   2. Insanely complex 
   3. Used by Facebook


### Apollo Client Setup

1. Apollo Store - communicates directly with graphQL server. It's agnostic to the front-end.
2. Apollo Provider - is a react component and accepts a reference to the datastore.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import ApolloClient from "apollo-client";
import { ApolloProvider } from "react-apollo";

const client = new ApolloClient();

const Root = () => {
  return (
    <ApolloProvider client={client}>
      <div>Lyrical</div>
    </ApolloProvider>
  );
};
```

### Getting data from graphQL to React

1. Identify data required
2. Write query in GraphiQL and in component file.
3. Bond query + component
4. Access data.

Notes: graphql-tag allows you to write a graphQL query using back ticks.

To pass the data from graphQL to a component use react-apollo and export graphql and the component

```jsx
import React, { Component } from "react";
import gql from "graphql-tag";
import { graphql } from "react-apollo";

class SongList extends Component {
  render() {
    console.log(this.props);
    return <div>this is the song list</div>;
  }
}

const query = gql`
  {
    songs {
      id
      title
      lyrics {
        content
      }
    }
  }
`;

export default graphql(query)(SongList);
```

5. Trying to get data before it loads throws an error in react.
   1. Use the "props.loading" boolean to check if graphQL returned the data before rendering.

```jsx
import React, { Component } from "react";
import gql from "graphql-tag";
import { graphql } from "react-apollo";

class SongList extends Component {
  renderSongs() {
    return this.props.data.songs.map((song) => {
      return <li>{song.title}</li>;
    });
  }
  render() {
    if (this.props.data.loading) {
      return <div>Loading</div>;
    }
    return <div>{this.renderSongs()}</div>;
  }
}

const query = gql`
  {
    songs {
      title
    }
  }
`;

export default graphql(query)(SongList);

```

### Mutations in GraphQL

1. Mutations look like functions and take query parameters.

```js
mutation AddSong($title: String){
    addSong(title: $title){
        id
        title
    }
}
```

### Mutations in React

1. When we wrap a mutation on a component it gives the component access to "props.mutate"
   1. this.props.mutate invokes the mutation wrapped on the component when it was exported.

```js
import _ from "lodash";
import React, { Component } from "react";
import gql from "graphql-tag";

const mutation = gql`
  mutation AddSong($title: String) {
    addSong(title: $title) {
      title
    }
  }
`;

class SongCreate extends Component {
  constructor(props) {
    super(props);
    this.state = {
      title: "",
    };
  }

  onSubmit(e) {
    e.preventDefault();
    console.log("submitting");
    this.props.mutate({
      variables: {
        title: this.state.title,
      },
    });
  }

  render() {
    return (
      <div>
        <h3>Create a New Song</h3>
        <form onSubmit={this.onSubmit.bind(this)}>
          <label>Song title:</label>
          <input
            type="text"
            onChange={(e) => this.setState({ title: e.target.value })}
            value={this.state.title}
          />
        </form>
      </div>
    );
  }
}

export default graphql(mutation)(SongCreate);
```

### Refetching data 

1. If a component uses a graphql query to fetch data, sometimes apollo client will not re-run the query after doing a mutation.
2. To fix this, the mutate function takes a "refetchQueries" argument, this is an array of query objects that can also take variables.

```jsx
  onSubmit(e) {
    e.preventDefault();
    this.props
      .mutate({
        variables: {
          title: this.state.title,
        },
        refetchQueries: [{ query: query }],
      })
      .then(() => hashHistory.push("/"))
      .catch((err) => console.log(err));
  }
```

3. The mutate function also has access to "props.data.refetch", this will re-run any queries tied to the component.

```jsx
  onSongDelete(id) {
    this.props
      .mutate({
        variables: {
          id: id,
        },
      })
      .then(() => this.props.data.refetch());
  }
```

4. Queries that are bound to a component can receive props using the options key and passing the varialbes that the query needs.

```js
export default graphql(query, {
  options: (props) => {
    return { variables: { id: props.params.id } };
  },
})(SongDetail);
```

