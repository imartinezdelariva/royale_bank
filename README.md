RoyaleBank

RoyaleBank is a full-stack online banking application developed as a university team project. It simulates common digital banking workflows, including customer registration, account management, transfers, phone-number-based payments and transaction history.

The project combines a web interface with a Spring-based REST API, relational data persistence, cookie-based authentication, automated testing and continuous deployment.

Project status: Academic prototype. It is not intended to process real financial data or credentials.

View the live demo

Key features

Customer registration, login and logout

Management of multiple bank accounts per customer

Account balance and transaction history views

Transfers between accounts using an IBAN

Phone-number-based payments between registered customers

Purchase and payment records

Automated unit, integration and end-to-end tests

Continuous integration with GitHub Actions

Continuous deployment with Render

Technology overview

Backend: Java, Spring Framework, Maven

API: REST, JSON and HTTP cookies

Persistence: Relational database with repository-based access

Frontend: HTML, CSS and JavaScript

Testing: Unit, integration and end-to-end tests with TestRestTemplate

Delivery: GitHub Actions and Render

Application walkthrough

Sign in



Existing customers can access their accounts using their email address and password. New users can navigate to the registration form from this screen.

Registration



New customers can create a profile by providing their personal information. Input is validated in both the browser and backend before being processed.

Account dashboard



The dashboard displays the accounts associated with the authenticated customer. From here, users can review balances and access account operations.

Create an account



Customers can create an additional bank account, which is automatically associated with their profile.

Account operations



For each account, customers can send a phone-number-based payment, make a transfer or review previous transactions.

Transaction history



The transaction view shows the operation type, description, source account, destination account and amount.

Phone-number-based payment



Customers can send money to another registered user using their phone number. The backend verifies that the recipient exists before processing the operation.

Bank transfer



Customers can transfer funds between their own accounts or to another account registered in the system.

Backend design

The domain model is built around four main entities:

Entity

Responsibility

Cliente

Stores the customer's identity and contact details

Cuenta

Represents a bank account owned by one customer

Pago

Records transfers, payments and purchases

Token

Associates an authentication token with a customer

Main relationships

One customer can own multiple accounts.

An account can be the source or destination of multiple payments.

An authentication token is associated with a customer session.

Authentication

Authenticated sessions use a token exchanged through an HTTP cookie. The cookie uses HttpOnly to prevent direct JavaScript access and SameSite=Lax to limit cross-site requests.

As this is an academic prototype, the authentication and authorisation design should be independently reviewed before any production use.

API overview

Method

Endpoint

Purpose

POST

/api/royale

Register a customer

POST

/api/royale/users

Authenticate and create a session

DELETE

/api/royale

Log out

GET

/api/royale

Retrieve the authenticated customer profile

POST

/api/royale/cuentas

Create a bank account

POST

/api/royale/bizum

Send a phone-number-based payment

POST

/api/royale/transferencia

Make a bank transfer

PUT

/api/royale/cuenta/saldo

Update an account balance

POST

/api/royale/compra

Record a purchase

GET

/api/royale/cuentas/operaciones/{iban}

List account transactions

DELETE

/api/royale/cuentas/{iban}

Delete a bank account

DELETE

/api/royale/cliente

Delete a customer profile

Testing

The automated test suite covers several layers of the application:

Unit tests: request validation and business rules

Integration tests: persistence and relationships between customers, accounts and tokens

End-to-end tests: registration, duplicate detection, authentication, authenticated profile retrieval, account creation and customer deletion

CI/CD

The workflow in .github/workflows/ci.yml runs on changes to the main branch. It builds the Maven project and executes the automated test suite. Render handles deployment of successful changes.

Project background

RoyaleBank was originally developed collaboratively as a university project. This repository preserves that shared origin and is being maintained and extended as a software engineering portfolio project.

Individual contributions and subsequent improvements should be documented through the repository's commit history and pull requests.

Roadmap

Add reproducible local setup instructions and environment templates

Strengthen authentication and server-side authorisation controls

Add static and dynamic application security testing

Expand negative and security-focused test coverage

Document the threat model and security findings

Improve API documentation and error responses

Disclaimer

RoyaleBank is an educational project. Do not use real personal, banking or authentication data in the application.
