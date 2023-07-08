# Vault Management and Protection of Sensitive Data

Vault is a powerful tool for managing secrets and safeguarding sensitive data. It offers a user-friendly interface, a command-line interface (CLI), and an HTTP API to securely store and control access to tokens, passwords, certificates, encryption keys, and other confidential information.

## Hashicorp Ecosystem

Vault is part of the Hashicorp ecosystem, which includes other essential tools like Terraform, Consul, Nomad, Boundary, Packer, Vagrant, and Waypoint.

## Introduction to Vault

Vault serves as an identity-based secret and encryption management system. Its primary function is to authenticate and authorize users within your organization, enabling controlled access to secrets within a traceable and tightly regulated environment.

## Four Stages of Vault

Vault operates through four stages:

1. **Authentication**: Users are authenticated with the system, resulting in the creation of a token associated with a policy.
2. **Validation**: User credentials are verified with a third-party system set up in your organization.
3. **Authorization**: Tokens are associated with a set of rules that govern controlled user access.
4. **Access**: Users gain access to the resources based on their authenticated and authorized token.

## Problem Statement

In many organizations, secrets, tokens, keys, and passwords are scattered across various locations such as text files, config files, databases, and environment variables. It becomes challenging to determine which copies of these credentials are valid, leading to several issues:

- Lack of a single source of truth for secrets.
- Absence of access control on files, including project repositories and confluence.
- Absence of an audit trail to track access and modifications.

## Installing Vault

To install Vault, you can download the binary ZIP file and store it in your `bin` directory. Alternatively, you can use the following command to install it on your system:

```bash
sudo apt update && sudo apt install gpg
sudo apt-get install vault
```

## Defaults for Dev Server

When using Vault in a `dev` environment, you can export the following values:

```bash
export VAULT_DEV_LISTEN_ADDRESS=127.0.0.1:8200
export VAULT_DEV_ROOT_TOKEN_ID="LearnVault"
```

To start the Vault server, execute the following command:

```
vault server -dev
```

## Connecting to the Server using the Client

To communicate with the server, open a new terminal and export the Vault address and token:

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN="LearnVault"
```

Next, log in to Vault by running the following command:

```bash
vault login
```

Upon successful login, the output will confirm the authentication and display relevant token information.

## KV Secrets Engine

By default, the KV version 2 secrets engine is enabled at the `secret/` path when using the dev server. To interact with the KV secrets engine, use the `kv` subcommand. The following are the available subcommands for the KV secrets engine:

- `delete`: Deletes versions of secrets stored in the Key-Value store (K/V).
- `destroy`: Permanently removes one or more versions of secrets.
- `enable-versioning`: Enables versioning for an existing K/V v1 store.
- `get`: Retrieves data from the K/V store.
- `list`: Lists data or secrets in the K/V store.
- `metadata`: Interacts with Vault's Key-Value storage metadata.
- `patch`: Updates secrets without overwriting existing ones.
- `put`: Sets or updates secrets, replacing existing ones.
- `rollback`: Reverts to a previous version of secrets.
- `undelete`: Restores a deleted version of secrets.

## CRUD Operations with Secrets

Let's explore how to perform CRUD (Create, Read, Update, Delete) operations with the secret engine.

### Creating Your First Secret

To store a secret, you need to provide the following information:
1. Mount location (default: `secret`)
2. Path
3. Key
4. Secret value

For example, to store the password `VaultIsSecure` with the key `about` in the mount location `foo`, execute the following command:

```bash
vault kv put -mount=secret foo about=VaultIsSecure
```

Upon successful write, you will receive output displaying metadata and secret path information.

### Updating Your Secret

Vault allows you to store multiple secrets. To update the secret we created earlier, run the following command:

```bash
vault kv put -mount=secret foo about=VaultIsSecure isItGreat=yes
```

Notice that the version for the secret has been incremented to 2.

### Reading Your Secret

Vault provides flexibility in receiving the secret output in various formats. Let's retrieve the secret in JSON format using the following command:

```bash
vault kv get -mount=secret -format=json foo | jq -r
```

Executing the above command will produce the secret output in JSON format.

### Deleting the Secret

To delete the secret, use the following command:

```bash
vault kv delete -mount=secret foo
```

Upon successful deletion, you will receive a confirmation message. Note that you can recover deleted secrets using the `undelete` command.

```bash
vault kv undelete -mount=secret -versions=2 foo
```

In upcoming tutorials, we will continue exploring secret mount paths and additional commands available in Vault.
