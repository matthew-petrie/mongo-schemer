# mongo-schemer

Provides clear Mongo schema validation error messages by running all validation errors through `ajv`. Does this by wrapping your Mongo client `db` in order to catch validation errors and compare against your schema.

## Requirements

- Your schema validation has to be in $jsonSchema format. More info here: https://docs.mongodb.com/manual/core/schema-validation/
- Currently only supports Promise-based usage of Mongo. PRs welcome!

## Installation

```
npm install mongo-schemer
```

## Usage

Wrap your `db` with `MongoSchemer.explainSchemaErrors()`:

```
const { MongoClient } = require('mongodb');
const MongoSchemer = require('mongo-schemer');

MongoClient.connect(dbUrl).then((client) => {
  const db = client.db(dbName);
  if (process.env.DEV) {
    db = MongoSchemer.explainSchemaErrors(db);
    MongoSchemer.onError((errors) => {
      console.log(errors);
    });
  }
});
```

Running in prod is not recommended due to the overhead of validating documents against the schema.