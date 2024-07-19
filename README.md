# solana_geyser_client

*Solana Geyser Plugin streaming account Data to a PostgREST Server*

## How does it work ?
At a high level, plugin allows you to specify a set of programs, from which you want the accounts (PDA's) to be streamed to your Postgres database.
It is divided into two plugins, one called "default" and the other "atlantic".
The difference between the two is that atlantic is also built to stream deserialized data to the database, while the default one only contains the raw data buffer.

### Use-cases

- Speed: Making a SQL Query to a Postgres database can be up to 10 times faster than using gPa (getProgramAccounts) calls.
- Realtime: We can benefit from Supabase's realtime infrastructure to stream Realtime account updates to dApp frontends.
- Filtering: With the atlantic plugin, we can directly filter on chain data with a SQL Query, instead of having to use gPa Filters.



## Getting started

### Clone Waverider

```
git clone https://github.com/aditya-dom/solana_geyser_client.git
cd solana_geyser_client
```

### Setting up the default Plugin

1. __Build the Plugin__

```
yarn build-default
```

2. __Set up your database__

This plugin works with every PostgREST Server, although i recommend you set up a Supabase instance. They have a generous free tier, but can also be self hosted for completely free.

Take the SQL Script at [config/default.sql](https://github.com/nautilus-project/waverider/blob/main/config/default.sql) and run it in your database, either via the CLI or a GUI(Supabase web GUI).

If you are using Supabase, also make sure to turn off Row Level Security for the accounts table, you can do this via the UI.

3. __Fill the configuration file__

If you are a mac user, open `config/config.default.mac.json`. If you are on Windows or Linux, open `config/config.default.json`.
Fill out these fields:

```json
{
  // Your supabase REST API URL (starting with https//). Don't forget the /rest/v1 at the end
  "supabase_url": "<url>/rest/v1",
  // Your Supabase Annon Key
  "supabase_key": "<key>",
  // The programs you want to index accounts from
  "programs": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"]
}
```

4. __Launch your validator with the plugin__

We'll use a local validator for this example, but this can be attached to a main/test/devnet validator.


:::Important
You HAVE to run the Solana Toolsuite (CLI) with the same version as this scripts Plugin Interface(view in Cargo.toml), and need to run the Plugin with the same Rust version you have built your Solana Toolsuite.
:::

For mac users, run: `solana-test-validator --geyser-plugin-config config/config.default.mac.json`.

For Linux/Windows users, run: `solana-test-validator --geyser-plugin-config config/config.default.json`.

