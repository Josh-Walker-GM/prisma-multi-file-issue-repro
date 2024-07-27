# prismaSchemaFolder relative path issue reproduction

This example setup is based on the "Simple TypeScript Script Example" and you can find the original README.md file for that example setup [here](https://github.com/prisma/prisma-examples/blob/latest/typescript/script/README.md).

## Description
The purpose of this example is to show an issue around database file location when you have enabled the `prismaSchemaFolder` preview feature.

You will notice a collection of schema files within the `prisma/schema` directory. We're using a SQLite database with the location coming from an environment variable. Given the recent changes in versions `5.15-1.17` we know that the database location, when given as a relative path, should now be determined relative to the `schema.prisma` file. The value is given in a `.env` file and is `../dev.db` so that it is created in `prisma/dev.db`.

The issue is that some commands such as generate and migrate work as expected. However, once you go to run the example script here it creates an empty database on directory above where would expect and fails because that database is empty - not migrated.

## Steps
1. Run `npx prisma migrate dev --name init` - this will succeed and produce a migrated database in the correct location.
2. Run `npm run dev` - this will fail and a new empty database file will be created.

## Notes
### `env` function in schema.prisma
The documentation (vscode pop-up) for env states that:

> Note that the file must be in the same directory as your schema.prisma file to automatically be picked up by the Prisma CLI.

However, this does not seem to be the case when you enable the `prismaSchemaFolder` preview feature. It has to be one directory up in that case.