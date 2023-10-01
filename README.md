[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/subsquid/squid-evm-template)

# Minimal EVM squid

This is a starter template of a squid indexer for EVM networks (Ethereum, Polygon, BSC, etc.). See [Squid SDK docs](https://docs.subsquid.io/) for a complete reference.

To extract EVM logs and transactions by a topic or a contract address, use `EvmBatchProcessor.addLog()` and `EvmBatchProcessor.addTransaction()` methods of the `EvmBatchProcessor` instance defined in `src/processor.ts`. 

The requested data is transformed in batches by a single handler provided to the `processor.run()` method. 

For a full list of supported networks and config options,
check the [`EvmBatchProcessor` overview](https://docs.subsquid.io/develop-a-squid/evm-processor/) and the [configuration page](https://docs.subsquid.io/develop-a-squid/evm-processor/configuration/).

For a step-by-step migration guide from TheGraph, see [the dedicated docs page](https://docs.subsquid.io/migrate/migrate-subgraph/).


## Quickstart Installation and Dependancies
Install Node.js and Docker. For windows user, Docker can download in https://docs.docker.com/desktop/install/windows-install and Nodejs can download in https://nodejs.org/en/download <br/>
<b>Note: please use your PC, because docker does not support in android anything</b>

```bash
sudo apt-get update
sudo apt-get install node nodejs npm docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
npm i -g @subsquid/cli
```
<br/>
For additional dependency installations, you can install this to ensure your work runs smoothly without obstacles in the future. 

```bash
npm i -g @types/node
npm i -g typescript
npm i -g @subsquid/archive-registry
npm i -g @subsquid/big-decimal
npm i -g @subsquid/bigquery-store
npm i -g @subsquid/bn-typeorm
npm i -g @subsquid/cli
npm i -g @subsquid/commands
npm i -g @subsquid/csv-store
npm i -g @subsquid/duckdb-store
npm i -g @subsquid/eth-ingest
npm i -g @subsquid/evm-ingest
npm i -g @subsquid/evm-processor
npm i -g @subsquid/evm-typegen
npm i -g @subsquid/file-store
npm i -g @subsquid/file-store-csv
npm i -g @subsquid/file-store-json
npm i -g @subsquid/file-store-parquet
npm i -g @subsquid/file-store-s3
npm i -g @subsquid/frontier
npm i -g @subsquid/graphiql-console
npm i -g @subsquid/graphql-server
npm i -g @subsquid/gsheets-store
npm i -g @subsquid/http-client
npm i -g @subsquid/hydra-cli
npm i -g @subsquid/hydra-common
npm i -g @subsquid/hydra-db-utils
npm i -g @subsquid/hydra-processor
npm i -g @subsquid/hydra-typegen
npm i -g @subsquid/ink-abi
npm i -g @subsquid/ink-typegen
npm i -g @subsquid/logger
npm i -g @subsquid/openreader
npm i -g @subsquid/rpc-client
npm i -g @subsquid/scale-codec
npm i -g @subsquid/scale-codec-json
npm i -g @subsquid/scale-type-system
npm i -g @subsquid/squid-gen
npm i -g @subsquid/squid-gen-evm
npm i -g @subsquid/squid-gen-ink
npm i -g @subsquid/squid-gen-targets
npm i -g @subsquid/squid-gen-utils
npm i -g @subsquid/ss58
npm i -g @subsquid/ss58-codec
npm i -g @subsquid/substrate-data
npm i -g @subsquid/substrate-data-raw
npm i -g @subsquid/substrate-dump
npm i -g @subsquid/substrate-evm-processor
npm i -g @subsquid/substrate-frontier-evm
npm i -g @subsquid/substrate-ingest
npm i -g @subsquid/substrate-metadata
npm i -g @subsquid/substrate-metadata-explorer
npm i -g @subsquid/substrate-processor
npm i -g @subsquid/substrate-runtime
npm i -g @subsquid/substrate-typegen
npm i -g @subsquid/typeorm-codegen
npm i -g @subsquid/typeorm-config
npm i -g @subsquid/typeorm-migration
npm i -g @subsquid/typeorm-store
npm i -g @subsquid/util
npm i -g @subsquid/util-internal
npm i -g @subsquid/util-internal-archive-client
npm i -g @subsquid/util-internal-archive-layout
npm i -g @subsquid/util-internal-binary-heap
npm i -g @subsquid/util-internal-code-printer
npm i -g @subsquid/util-internal-commander
npm i -g @subsquid/util-internal-config
npm i -g @subsquid/util-internal-counters
npm i -g @subsquid/util-internal-fs
npm i -g @subsquid/util-internal-gql-request
npm i -g @subsquid/util-internal-hex
npm i -g @subsquid/util-internal-http-client
npm i -g @subsquid/util-internal-http-server
npm i -g @subsquid/util-internal-ingest-tools
npm i -g @subsquid/util-internal-json
npm i -g @subsquid/util-internal-processor-tools
npm i -g @subsquid/util-internal-prometheus-server
npm i -g @subsquid/util-internal-range
npm i -g @subsquid/util-internal-read-lines
npm i -g @subsquid/util-internal-resilient-rpc
npm i -g @subsquid/util-internal-service-manager
npm i -g @subsquid/util-naming
npm i -g @subsquid/util-timeout
npm i -g @subsquid/util-xxhash
npm i -g @subsquid/warthog
```

```bash
# 1. Retrieve the template
sqd init my_squid_name -t evm
cd my_squid_name

# 2. Install dependencies
npm ci

# 3. Start a Postgres database container and detach
sqd up

# 4. Build and start the processor
sqd process

# 5. The command above will block the terminal
#    being busy with fetching the chain data, 
#    transforming and storing it in the target database.
#
#    To start the graphql server open the separate terminal
#    and run
sqd serve
```
A GraphiQL playground will be available at [localhost:4350/graphql](http://localhost:4350/graphql).

## Dev flow

### 1. Define database schema

Start development by defining the schema of the target database via `schema.graphql`.
Schema definition consists of regular graphql type declarations annotated with custom directives.
Full description of `schema.graphql` dialect is available [here](https://docs.subsquid.io/basics/schema-file).

### 2. Generate TypeORM classes

Mapping developers use TypeORM [EntityManager](https://typeorm.io/#/working-with-entity-manager)
to interact with target database during data processing. All necessary entity classes are
generated by the squid framework from `schema.graphql`. This is done by running `sqd codegen`
command.

### 3. Generate database migrations

All database changes are applied through migration files located at `db/migrations`.
`squid-typeorm-migration(1)` tool provides several commands to drive the process.

```bash
## drop create the database
sqd down
sqd up

## replace any old schemas with a new one made from the entities
sqd migration:generate
```
See [docs on database migrations](https://docs.subsquid.io/basics/db-migrations) for more details.

### 4. Import ABI contract and generate interfaces to decode events

It is necessary to import the respective ABI definition to decode EVM logs. One way to generate a type-safe facade class to decode EVM logs is by placing the relevant JSON ABIs to `./abi`, then using `squid-evm-typegen(1)` via an `sqd` script:

```bash
sqd typegen
```

See more details on the [`squid-evm-typegen` doc page](https://docs.subsquid.io/evm-indexing/squid-evm-typegen).

## Project conventions

Squid tools assume a certain [project layout](https://docs.subsquid.io/basics/squid-structure):

* All compiled js files must reside in `lib` and all TypeScript sources in `src`.
The layout of `lib` must reflect `src`.
* All TypeORM classes must be exported by `src/model/index.ts` (`lib/model` module).
* Database schema must be defined in `schema.graphql`.
* Database migrations must reside in `db/migrations` and must be plain js files.
* `sqd(1)` and `squid-*(1)` executables consult `.env` file for environment variables.
