# gql-generator

Generate queries from graphql schema, used for writing api test.

## Example

```gql
# Sample schema
type Query {
  user(id: Int!): User!
}

type User {
  id: Int!
  username: String!
  email: String!
  createdAt: String!
}
```

```gql
# Sample query generated
query user($id: Int!) {
  user(id: $id) {
    id
    username
    email
    createdAt
  }
}
```

## Usage

```bash
# Install
npm install @williamyin/gql-generator -g

# see the usage
gqlg --help

# Generate sample queries from schema file
gqlg --schemaFilePath ./example/sampleTypeDef.graphql --destDirPath ./example/output --depthLimit 5
```

Now the queries generated from the [`sampleTypeDef.graphql`](./example/sampleTypeDef.graphql) can be found in the destDir: [`./example/output`](./example/output).

This tool generate 3 folders holding the queries: mutations, queries and subscriptions.

The tool will automatically exclude any `@deprecated` schema fields (see more on schema directives [here](https://www.apollographql.com/docs/graphql-tools/schema-directives)). To change this behavior to include deprecated fields you can use the `includeDeprecatedFields` flag when running the tool, e.g. `gqlg --includeDeprecatedFields`.

### Programmatic Access

Alternatively, you can run `gql-generator` directly from your scripts:

```js
const gqlg = require('gql-generator');

gqlg({ schemaFilePath: './example/sampleTypeDef.graphql', destDirPath: './example/output', depthLimit: 5 });
```

## Notes

- As this tool is used for tests, it expands all of the fields in a query. There might be recursive fields in the query, so `gqlg` ignores the types which have been added in the parent queries already by default. This can be disabled using the `--includeCrossReferences` argument.
- Variable names are derived from argument names, so variables generated from multiple occurrences of the same argument name must be deduped. An index is appended to any duplicates e.g. `region(language: $language1)`.
