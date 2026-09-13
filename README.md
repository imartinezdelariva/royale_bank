# 🏦 RoyaleBank

RoyaleBank is a full-stack online banking application developed as a university team project. It simulates common banking operations, including customer registration, account management, transfers, phone-number-based payments and transaction history.

The project combines a web interface with a Spring-based REST API, relational data persistence, cookie-based authentication, automated testing and continuous deployment.

> **Project status:** Academic prototype. It is not intended to process real financial data or credentials.

## Key features

- Customer registration, login and logout
- Management of multiple bank accounts
- Account balance and transaction history
- Bank transfers using an IBAN
- Phone-number-based payments
- Purchase and payment records
- Unit, integration and end-to-end tests
- Continuous integration with GitHub Actions
- Continuous deployment with Render

## Technology stack

- **Backend:** Java, Spring Framework and Maven
- **Frontend:** HTML, CSS and JavaScript
- **API:** REST and JSON
- **Database:** Relational database
- **Authentication:** Token-based sessions using HTTP cookies
- **Testing:** Unit, integration and E2E tests
- **CI/CD:** GitHub Actions and Render

## Application walkthrough

### Sign in

![Sign-in screen](./fotos/iniciosesion.png)

Existing customers can access their accounts using their email address and password. New users can navigate to the registration form from this screen.

### Registration

![Registration screen](./fotos/registro.png)

New customers can create a profile by providing their personal information. Input is validated in both the browser and backend before being processed.

### Account dashboard

![Account dashboard](./fotos/principal.png)

The dashboard displays all accounts associated with the authenticated customer. Users can review their balances and access the available account operations.

### Create an account

![Create-account screen](./fotos/crearcuenta.png)

Customers can create additional bank accounts. Each new account is automatically associated with the authenticated customer.

### Account operations

![Account actions](./fotos/acciones.png)

For each account, customers can:

- Send a phone-number-based payment
- Make a bank transfer
- Review the transaction history

### Transaction history

![Transaction history](./fotos/historial.png)

The transaction view displays:

- Operation description
- Source account
- Destination account
- Amount
- Transaction type

### Phone-number-based payment

![Phone-number-based payment](./fotos/bizum.png)

Customers can send money to another registered user using their phone number. The backend verifies that the recipient exists before processing the operation.

### Bank transfer

![Bank transfer](./fotos/transferencia.png)

Customers can transfer funds between their own accounts or to another account registered in the system.

## Backend design

The backend is implemented using Spring Framework and follows a REST-based architecture.

### Customer entity

| Attribute | Description | Example | Constraint |
| --- | --- | --- | --- |
| `cliente_id` | Unique customer identifier | `123` | Primary key |
| `dni` | Spanish identity document | `12345678A` | Unique and required |
| `nombre` | Customer's first name | `Juan` | Required |
| `apellido` | Customer's surname | `Pérez` | Optional |
| `email` | Customer's email address | `juan.perez@mail.com` | Unique and required |
| `telefono` | Customer's phone number | `600111222` | Required |
| `password` | Customer's password | — | Required |

### Account entity

| Attribute | Description | Example | Constraint |
| --- | --- | --- | --- |
| `cuenta_id` | Unique account identifier | `456` | Primary key |
| `iban` | Unique account IBAN | `ES9121000418450200051332` | Unique and required |
| `saldo` | Current account balance | `1500.75` | Required |
| `sucursal` | Bank branch | `Bilbao` | Required |
| `cliente_id` | Owner of the account | `123` | Foreign key |

### Payment entity

| Attribute | Description | Example | Constraint |
| --- | --- | --- | --- |
| `id` | Unique payment identifier | `789` | Primary key |
| `tipo` | Payment type | `transferencia` | Required and validated |
| `importe` | Amount transferred | `100.00` | Required |
| `cuenta_origen_id` | Source account | `456` | Foreign key |
| `cuenta_destino_id` | Destination account | `457` | Optional foreign key |
| `concepto` | Payment description | `Electricity bill` | Required |

### Token entity

| Attribute | Description | Constraint |
| --- | --- | --- |
| `id` | Unique token identifier | Primary key |
| `cliente_id` | Customer associated with the token | Foreign key |

### Entity relationships

| Relationship | Description |
| --- | --- |
| Customer → Account | One customer can own multiple accounts |
| Account → Payment | One account can be the source of multiple payments |
| Account → Payment | One account can receive multiple payments |
| Customer → Token | A token is associated with a customer session |

## Authentication

Authenticated sessions use a token exchanged through an HTTP cookie.

The cookie uses:

- `HttpOnly` to prevent direct access from JavaScript
- `SameSite=Lax` to limit cross-site requests
- An application-wide path to maintain the session across routes

As this is an academic prototype, the authentication and authorisation design should be independently reviewed before production use.

## API overview

| Method | Endpoint | Purpose | Main responses |
| --- | --- | --- | --- |
| `POST` | `/api/royale` | Register a customer | `201`, `409` |
| `POST` | `/api/royale/users` | Authenticate and create a session | `201`, `401` |
| `DELETE` | `/api/royale` | Log out | `204`, `401` |
| `GET` | `/api/royale` | Retrieve the customer profile | `200`, `401` |
| `POST` | `/api/royale/cuentas` | Create a bank account | `201`, `401`, `409` |
| `POST` | `/api/royale/bizum` | Send a phone-number-based payment | `201`, `401`, `404`, `409` |
| `POST` | `/api/royale/transferencia` | Make a bank transfer | `200`, `401`, `404`, `409` |
| `PUT` | `/api/royale/cuenta/saldo` | Update an account balance | `200`, `401`, `404`, `409` |
| `POST` | `/api/royale/compra` | Record a purchase | `200`, `401`, `404`, `409` |
| `GET` | `/api/royale/cuentas/operaciones/{iban}` | List account transactions | `200`, `401`, `404` |
| `DELETE` | `/api/royale/cuentas/{iban}` | Delete a bank account | `204`, `401`, `404` |
| `DELETE` | `/api/royale/cliente` | Delete a customer profile | `204`, `401`, `404` |

## Testing

The project includes several levels of automated testing.

### Unit tests

Unit tests validate business logic and request constraints, including:

- DNI format
- Email format
- Phone-number format
- Password requirements
- Multiple simultaneous validation errors

### Integration tests

Integration tests verify the interaction between application components and the persistence layer:

- Saving customers with associated accounts
- Creating and retrieving authentication tokens
- Persisting entity relationships
- Querying customer, account and token repositories

### End-to-end tests

E2E tests use `TestRestTemplate` to send HTTP requests to the application:

- Customer registration
- Duplicate customer detection
- Authentication and session-cookie creation
- Authenticated profile retrieval
- Bank-account creation
- Customer deletion

## CI/CD

The GitHub Actions workflow located at [`.github/workflows/ci.yml`](./.github/workflows/ci.yml) runs automatically when changes are pushed to the main branch.

The workflow:

- Builds the project with Maven
- Runs unit, integration and end-to-end tests
- Verifies that the application passes the automated checks

Render is used for continuous deployment after successful changes.

## Project background

RoyaleBank was originally developed collaboratively as a university project. This repository preserves that shared origin and is being maintained and extended as a software engineering portfolio project.

Individual contributions and subsequent improvements can be reviewed through the repository's commit history.

## Roadmap

- Strengthen authentication and server-side authorisation
- Add static application security testing
- Add dynamic application security testing
- Expand security-focused test coverage
- Document the application's threat model
- Improve API documentation
- Add reproducible local installation instructions

## Disclaimer

RoyaleBank is an educational project. Do not use real personal, banking or authentication data in the application.
