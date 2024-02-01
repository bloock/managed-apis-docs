---
stoplight-id: 1ykgb7ldotkim
---

# Configuration

The Bloock Managed API leverages Viper, a powerful configuration management library, currently supporting environment variables and a YAML configuration file.

## Variables

Here are the configuration variables used by the Bloock Managed API:

- **BLOOCK_API_KEY** (**REQUIRED**)
  - **Description**: Your unique [BLOOCK API key](https://docs.bloock.com/libraries/authentication/create-an-api-key).
  - **Purpose**: This [API key](https://docs.bloock.com/libraries/authentication/create-an-api-key) is required for authentication and authorization when interacting with the Bloock Identity Managed API. It allows you to securely access and use the API's features.
  - **[Create API Key](https://docs.bloock.com/libraries/authentication/create-an-api-key)**
- **BLOOCK_DB_CONNECTION_STRING** (**_OPTIONAL_**)
  - **Description**: Your custom database connection URL.
  - **Default**: "file:bloock?mode=memory&cache=shared&\_fk=1"
  - **Purpose**: This variable allows you to specify your own [database](#database-support) connection string. You can use it to connect the API to your existing database infrastructure. The format depends on the [database](#database-support) type you choose.
  - **Required**: When docker database container or your existing database infrastructure provided.
- **BLOOCK_WEBHOOK_SECRET_KEY** (**_OPTIONAL_**)
  - **Description**: Your [BLOOCK webhook secret key](https://docs.bloock.com/webhooks/overview).
  - **Purpose**: The [webhook secret key](https://docs.bloock.com/webhooks/overview) is used to secure and verify incoming webhook requests. It ensures that webhook data is received from a trusted source and has not been tampered with during transmission.
  - **Required**: When you want to certificate data using integrity Bloock product.
  - **[Create webhook](https://docs.bloock.com/webhooks/overview)**
- **BLOOCK_CLIENT_ENDPOINT_URL** (**_OPTIONAL_**)
  - **Description**: An endpoint URL where you want to send processed files.
  - **Purpose**: This URL specifies the destination where processed files will be sent after successful verification. It can be configured to integrate with other systems or services that require the processed data.
- **BLOOCK_AUTHENTICITY_KEY** (**_OPTIONAL_**)
  - **Description**: Private key for signing data.
  - **Purpose**: If you want to sign data using your own local private key, you can specify it here. This private key is used for cryptographic operations to ensure data integrity and authenticity.
- **BLOOCK_ENCRYPTION_KEY** (**_OPTIONAL_**)
  - **Description**: Private key for encrypting data.
  - **Purpose**: If you want to encrypt data using your own local key, you can specify it here.
- **BLOOCK_API_HOST** (**_OPTIONAL_**)
  - **Description**: The API host IP address.
  - **Default**: 0.0.0.0
  - **Purpose**: This variable allows you to specify the IP address on which the Bloock Managed API should listen for incoming requests. You can customize it based on your network configuration.
- **BLOOCK_API_PORT** (**_OPTIONAL_**)
  - **Description**: The API port number.
  - **Default**: 8080
  - **Purpose**: The API listens on this port for incoming HTTP requests. You can adjust it to match your preferred port configuration.
- **BLOOCK_API_DEBUG_MODE** (**_OPTIONAL_**)
  - **Description**: Enable or disable debug mode.
  - **Default**: false
  - **Purpose**: When set to true, debug mode provides more detailed log information, which can be useful for troubleshooting and debugging. Set it to false for normal operation.
- **BLOOCK_TMP_DIR** (**_OPTIONAL_**)
  - **Description**: The temporary directory path for storing processed files.
  - **Default**: ./tmp
  - **Purpose**: Processed files can be temporarily stored in this directory while waiting for integrity confirmation. You can configure it to a specific directory path that suits your storage needs.
- **BLOOCK_STORAGE_LOCAL_PATH** (**_OPTIONAL_**)
  - **Description**: The local directory where files will be stored when using `LOCAL` availability type.
  - **Default**: ./data
  - **Purpose**: Processed files will be stored in this directory only when `LOCAL` availability type is specified when invoking the process endpoint.
- **BLOOCK_STORAGE_LOCAL_STRATEGY** (**_OPTIONAL_**)
  - **Description**: The filename strategy to follow when using `LOCAL` availability type.
  - **Default**: HASH
  - **Purpose**: Currently it supports two possible values: `HASH` (default) will set the file name to the content's hash and `FILENAME` will try to compute the file name based on the URL or the file provided (WARNING: this strategy may overwrite files if two of them compute to the same filename, i.e. uploading to files with the same name).

These configuration variables provide fine-grained control over the behavior of the Bloock Managed API. You can adjust them to match your specific requirements and deployment environment.

## Configuration file

The configuration file should be named `config.yaml`. The service will try to locate this file in the root directory unless the BLOOCK_CONFIG_PATH is defined (i.e. `BLOOCK_CONFIG_PATH="app/conf/"`).

Sample content of `config.yaml`:

```yaml
BLOOCK_API_HOST: "0.0.0.0"
BLOOCK_API_PORT: "8080"
BLOOCK_API_DEBUG_MODE: "false"

BLOOCK_DB_CONNECTION_STRING: "file:bloock?mode=memory&cache=shared&_fk=1"

BLOOCK_API_KEY: ""
BLOOCK_WEBHOOK_SECRET_KEY: ""
BLOOCK_CLIENT_ENDPOINT_URL: ""

BLOOCK_AUTHENTICITY_KEY: ""

BLOOCK_ENCRYPTION_KEY: ""

BLOOCK_TMP_DIR: "./tmp"
BLOOCK_STORAGE_LOCAL_PATH: "./data"
```

## Database Support

The Bloock Managed API is designed to be flexible when it comes to database integration. It supports three types of relational databases: **MemDB (SQLite)**, **MySQL**, and **Postgres**. The choice of database type depends on your specific requirements and infrastructure.

Here are the supported database types and how to configure them:

- **MySQL**: To connect to a MySQL database, you can use the following connection string format
  ```
  mysql://user:password@tcp(host:port)/database
  ```

Replace `user`, `password`, `host`, `port`, and `database` with your MySQL database credentials and configuration. This format allows you to specify the MySQL database you want to connect to.

- **Postgres**: For PostgreSQL database integration, use the following connection string format:

  ```
  postgresql://user:password@host/database?sslmode=disable
  ```

Similar to MySQL, replace `user`, `password`, `host`, and `database` with your PostgreSQL database details. Additionally, you can set the `sslmode` as needed. The `sslmode=disable` option is used in the example, but you can adjust it according to your PostgreSQL server's SSL requirements.

- **MemDB (SQLite)**: The API also supports in-memory SQLite databases. To use SQLite, you can specify the connection string as follows:

  ```
  file:dbname?mode=memory&cache=shared&_fk=1
  ```

In this format, `dbname` represents the name of your SQLite database. The API will create an in-memory SQLite database with this name.

If you already have an existing database infrastructure and want to use it with the Bloock Managed API, you have the flexibility to provide your custom database connection string.

`Variable: BLOOCK_DB_CONNECTION_STRING`

The API provides a configuration variable called `BLOOCK_DB_CONNECTION_STRING` that allows you to specify your own database connection string independently of the way you run the API. Whether you run the API as a Docker container or as a standalone application, you can always set this variable to point to your existing database server.