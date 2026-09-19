# server-hibernate

`ServerHibernate` is a small Java socket-based chat server backed by Hibernate and PostgreSQL.

The project listens for TCP client connections, parses simple text commands, and persists chat-related entities such as users, rooms, messages, and user profile details through Hibernate.

## What The Project Does

The server starts on port `7878` and accepts client connections through a `ServerSocket`.

Connected clients are handled on separate threads. Incoming text commands are passed to a command parser, which delegates to repository-backed command executors. The persistence layer uses Hibernate sessions and annotated JPA entities to work with:

- `User`
- `UserDetails`
- `Room`
- `Message`

## Main Components

- `ChatServerApplication`: application entry point
- `ChatServer`: starts and stops the TCP server
- `ClientListener`: accepts incoming socket connections
- `CommandWorker`: handles one connected client
- `command/*`: command parsing and execution
- `repository/*`: Hibernate-backed repository implementations
- `database/SessionFactoryInitializer`: Hibernate session factory bootstrap
- `domain/*`: entity model

## Tech Stack

- Java 8 target
- Hibernate ORM
- PostgreSQL JDBC driver
- Lombok
- Maven
- JUnit 4

## Build And Run

### Requirements

- JDK 17 or newer to build
- Maven 3.9+
- PostgreSQL if you want to run the server against a real database

The project compiles to Java 8 bytecode, but it builds cleanly on a modern JDK.

### Run The Build

```bash
mvn test
```

This project currently has no committed automated tests, so this command mainly validates compilation and the Maven lifecycle.

### Run The Server

```bash
mvn exec:java -Dexec.mainClass=org.levelup.server.chat.ChatServerApplication
```

You can also run `ChatServerApplication` directly from your IDE.

## Database Configuration

Database settings are currently hard-coded in `SessionFactoryInitializer`:

- host: `localhost`
- port: `5432`
- database: `coto-chat`
- username: `postgres`
- password: `root`

Hibernate is configured with `hbm2ddl.auto=create`, which recreates the schema on startup. That is convenient for local experiments, but risky for persistent environments.

## Current Limitations

- no automated tests are present in the repository
- database credentials are hard-coded
- schema generation is destructive on startup
- command protocol is only lightly documented in code

## Recent Maintenance

This repository was updated to:

- refresh outdated build dependencies
- upgrade Lombok so the project builds on modern JDKs
- replace the obsolete PostgreSQL JDBC coordinates with the current driver artifact
- remove unused dependencies from the build
- configure UTF-8 explicitly for Maven compilation and test execution
- add an English README for onboarding and maintenance
