# Migrate Command Line Tool

Migrate is the command-line tool supporting the https://github.com/sbowman/migrations package.  It
provides both a usable tool to run database migrations without embedding the `migrations` package
into your own app, but also example code of how to configure and use migrations if you do want to
embed the library your application (recommended, so you can run migrations on deployment).

## Migrations Integration and CLI

The `migrations` package may be included in your product binary, or run separately as a standalone
application. When embedded in the application, typically you configure your application to apply the
database migrations on startup.

It may also be run as a standalone tool, either from the command line or a Docker container.

### Installing the Command-Line Tool

To install the command-line tool, run:

    $ go install github.com/sbowman/migrations/v2/migrate

This will install the `migrate` binary into your `$GOPATH/bin` directory. If that directory is on
your PATH, you should be able to run migrations from anywhere.

If you need help using the command-line tool, run `migrate help` for information.

## Available Options

To apply migrations to the database schema, you need to supply two pieces of information:  the path
to the SQL migrations, and the database URI to connect to the database. To supply that information
to the command-line application, use the `migrations` and `uri` parameters:

    $ migrate apply --uri='postgres://postgres@127.0.0.1/fourier?sslmode=disable&connect_timeout=5'
    --migrations=./sql

By default, the `migrations` package looks for migrations in the `sql` directory relative to
where you run the binary, so you can leave `--migrations` off:

    $ migrate apply --uri='postgres://postgres@127.0.0.1/fourier?sslmode=disable&connect_timeout=5'

You may additionally apply a revision to migrate the database to the specific version of the schema
using the `--revision` parameter:

    $ migrate apply --uri='postgres://postgres@127.0.0.1/fourier?sslmode=disable&connect_timeout=5'
    --revision=23

This will either apply the migrations to reach SQL migration file `23-<name>.sql`, or rollback the
database migrations to step back down to revision 23.

### Supported Environment Variables

The `migrations` package supports supplying configuration options via environment variables.

* To supply an alternative migrations directory, you can use the `MIGRATIONS` environment variable.
* To supply the PostgreSQL URI in the command-line application, you can use the `DB_URI` or `DB`
  environment variables.

