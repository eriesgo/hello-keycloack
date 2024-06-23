# README

## Docker guide

<https://www.keycloak.org/getting-started/getting-started-docker>

### Running in dev mode (first time)

Start Keycloack (with Docker)

`create-start-keycloack-dev.sh`

This command starts Keycloak exposed on the local port 8080 and creates an initial admin user with the username admin and password admin.

> Warning: This script creates a new container every time erasing the realms and users that you may have created before.

### Admin Console

This is the console for managing realms and users

- Visit local admin console: `http://localhost:8080/admin`
- Create a new realm (`localhost-hello`)
- Create a new user

### Account Console

This is the console for managing individual user account

- Login with the new user `http://localhost:8080/realms/localhost-hello/account`

## How to create admins for realms

https://stackoverflow.com/questions/56743109/keycloak-create-admin-user-in-a-realm

## CIAM and multitenancy

https://www.keycloak.org/2024/06/announcement-keycloak-organizations

It is a technology preview feature.

Start Keycloak with `docker run --name kc-orgs -d -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin -p 8080:8080 quay.io/keycloak/keycloak start-dev --features organization`
This needs to be enabled at start or when building optimized version.

- Create a realm: `localhost-orgdemo`
- Create users in this realm
  - `mjane` with `orgdemo.com` domain
  - `alice` with `orga.com` domain
- Authentication works as normal with user and password

- Enable organizations (`Realm Settings`) section
- Authentication works now with two steps: first asking for user or email, and the second for the password.

- Authentication of a user that does not exist using an email domain that matches an organization - not working yet, but multiple options possible

- Authenticating as an existing user using an email domain that matches the domain set to an identity provider associated with an organization
- Create `orga` realm
  - Create an OpenID Connect client for `localhost-orgdemo` - with name `localhost-orgdemo-broker` secret `WrLQ0Im5add7XKq4wM0t62x6DwSTN9Tp`
    - Discovery endpoint is `http://localhost:8080/realms/orga/.well-known/openid-configuration`
  - Create a user `jdoe`
- In the other realm, `localhost-orgdemo`
  - Create an OpenID Connect Identity Provider to `orga`
- Finally link the IdP created in `localhost-orgdemo` realm with the `orga` organization

## Database

Created a docker-compose file to run keycloack in development mode with a PostreSQL as data storage: `docker-compose up`