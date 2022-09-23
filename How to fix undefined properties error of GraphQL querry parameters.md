# Problem
How to fix undefined properties error of GraphQL querry parameters


# Environment
N/A


# How you fix it
Make sure at server end GrapQL schema is updated, then update schema.json file content in the project.

# Solution
- First re-check **API.swift** file, all the updated parameters should be part of that file.
- Then re-check **ApolloClientQuery.graphql** file, the query should be updated (with updated parameters).
- If problem still exists, get the GraphQL schema updated at the server end.
- Fetch updated schema file using command: "apollo schema:download --endpoint=http://localhost:8080/graphql schema.json" (replace http://localhost:8080/ with your server url).
- Update the **schema.json** file in the project.

Greate now build your project successfully without any errors/issues, cheers!

