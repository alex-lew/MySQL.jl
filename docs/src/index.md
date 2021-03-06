# MySQL.jl Documentation

```@contents
```

## Getting started

The MySQL.jl package provides a client library for Julia to interface with a mysql server. The package can be installed by doing:

```julia
] add MySQL
```

Once installed, you start using the package by making a connection to the mysql database by doing something like:

```julia
conn = DBInterface.connect(MySQL.Connection, host, user, passwd)
```

This utilizes the DBInterface.jl package method `connect` and passes in `MySQL.Connection` as the first argument to signal the type of database we're connecting to. `DBInterface.connect` also supports a host of options like the port to connect to, whether to use a socket, where an options file is located etc. To see the full list of supported keyword arguments, see the help for [`DBInterface.connect`](@ref).

Once connected, there are two ways to submit queries to the server:

  * `stmt = DBInterface.prepare(conn, sql); DBInterface.execute(stmt, params)`: first prepare a SQL statement against the database, then execute it with optional params. This allows re-executing the same statement repeatedly in an efficient way.
  * `DBInterface.execute(conn, sql, params)`: directly execute a SQL statement against the database, optionally passing params to be bound to markers.

Both execution methods return a `Cursor` object that supports the [Tables.jl](https://juliadata.github.io/Tables.jl/stable/) interface, which allows materializing a query resultset in a number of ways, like `DataFrame(x)`, `CSV.write("results.csv", x)`, etc.

MySQL.jl attempts to provide a convenient `MySQL.load(table, conn, table_name)` function for generically loading Tables.jl-compatible sources into database tables. While the mysql API has some utilities for even making this possible, just note that it can be tricky to do generically in practice due to specific modifications needed in `CREATE TABLE` and column type statements.

## API reference
```@docs
DBInterface.connect
DBInterface.close!
MySQL.escape
DBInterface.execute
DBInterface.prepare
DBInterface.lastrowid
```
