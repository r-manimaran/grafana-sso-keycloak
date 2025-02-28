# Perform KeyCloak OIDC Single-Sign on for Grafana Application using Docker

## Overview
This repository demonstrates how to implement Single Sign-On (SSO) for Grafana using Keycloak as the Identity Provider (IdP) through OpenID Connect (OIDC) protocol. The setup uses Docker containers to run both Keycloak and Grafana services, making it easy to deploy and test the SSO integration.

## Summary
The implementation covers:
- Setting up Keycloak and Grafana using Docker Compose
- Configuring a new realm and client in Keycloak
- Creating and managing users in Keycloak
- Setting up OIDC authentication between Keycloak and Grafana
- Managing user roles and permissions
- Testing the integration using Postman
- Verifying JWT tokens and user information
- Implementing role-based access control (RBAC)

### Key Features
- Docker-based deployment
- OIDC protocol implementation
- Role-based access management
- JWT token verification
- User profile management
- Admin role configuration
- Client scope and mapper setup

### Prerequisites
- Docker and Docker Compose installed
- Basic understanding of OAuth2/OIDC concepts
- Postman (for testing API endpoints)



Use the below command to up and run the containers
```bash
docker-compose up -d
```
- If the images are not already present, it will download and up the services.

![alt text](images/image.png)

## Testing the Keycloak interface

- Access the Keycloak application by launching the below URL
```Url
http://localhost:8080
```
- Provide the admin password mentioned in the docker-compose keycloak service
![alt text](images/image-1.png)

### Testing Grafana interface
- Access the Grafana application by launching the below URL.

```Url
http://localhost:3000
```
![alt text](images/image-2.png)

- You can notice the button `Sign in with Keycloak` added in the login screen.

## Create Realm and user information in Keycloak

## Steps involved
- Create the realm named `grafana`
- Create a client named `grafana` inside the realm

![alt text](images/image-3.png)

![alt text](images/image-4.png)

![alt text](images/image-5.png)

![alt text](images/image-6.png)

- Provide the URL associated to the grafana.

![alt text](images/image-7.png)

- Turn-on client authentication on the new client

![alt text](images/image-8.png)

- Get the client secret and map it in the grafana.ini file

![alt text](images/image-9.png)

## Add new User in Keycloak realm

- Click the users menu and click create new user
- provide the user information 

![alt text](images/image-10.png)

- Go to credentials and set the password.
- Set the Temporary flag to off

![alt text](images/image-11.png)

- Now the password is set for the user.

![alt text](images/image-12.png)

## Testing Keycloak user authentication using Postman

- Download and import the postman collection from the postman folder.
- Update the client_id, client_sercret, username and password values.
- Test the Get Access Token endpoint.

![alt text](images/image-13.png)

- Use `jwt.io` to check the Access Token and ID token
### Access Token:
![alt text](images/image-14.png)

### ID Token:
![alt text](images/image-15.png)

## User Info Get endpoint
![alt text](images/image-16.png)

## Test SSO from Grafana
- Launch grafana
- Click the Sign-in using Keycloak button. You will be re-directed to keycloak authentication page.

![alt text](images/image-17.png)

- On clicking sign-in, it will authenticate in keycloak and redirect to Grafana

![alt text](images/image-18.png)

## Check logged in user details in Grafana
- Click the profile icon and check the user name and displayname.

![alt text](images/image-19.png)

- This information are pass from Keycloak to grafana. Grafana will use the userinfo endpoint in keycloak to get the details.

- Click the profile link to vie the full profile information.

![alt text](images/image-20.png)

- By default it will map the logged in user to Viewer role. (because of the setting in the grafana.ini file)

![alt text](images/image-21.png)

## Create Admin Role in Keycloak
- Now we will create the Admin role in Keycloak and map it to the user.
- Login to Keycloak as admin and route to grafana realm.

- Create a Client scope. name it as `realm-roles-userinfo`
- Set the Type to default and click save

![alt text](images/image-22.png)

- Go to Mapper and select `Add predefined mapper`.

- Search for `user realm role` and check `realm roles` mapper

![alt text](images/image-23.png)

- This will ensure the logged in user will have the realm level roles associated with the tokens.

- Now bind the role to the client `grafana`. Click clients and select `grafana`.

![alt text](images/image-24.png)

- Click `Add client scope` button and select `realm-roles-userinfo`. On clicking Add, select `Default`.

![alt text](images/image-25.png)

## Add the Role to the access token, ID token and userinfo endpoints
- On Add to ID token and Add to userinfo.

![alt text](images/image-26.png)

## Add realm role.
- In the grafana realm, click `Realm roles`.

![alt text](images/image-27.png)

- Click `Create role` and specify the role name as `Admin`.

![alt text](images/image-28.png)

## Map role to User.

- Go to Users and click the user.
- On Role mapping tab, click `Assign role`

![alt text](images/image-29.png)

- Click `Filter by realm roles` in the dropdown to list the realm associated roles. Select admin and Assign

![alt text](images/image-30.png)

## Test the newly Assigned role using Postman endpoints and jwt.io

![alt text](images/image-31.png)

![alt text](images/image-32.png)

![alt text](images/image-33.png)

- Info in Id token:

![alt text](images/image-34.png)

## Sign-in from Grafana and test

![alt text](images/image-36.png)

- Check the profile and ensure the Admin role is mapped.

![alt text](images/image-35.png)

## Endnote

This implementation demonstrates a complete end-to-end setup of Keycloak SSO integration with Grafana using OIDC protocol. Key accomplishments include:

### Authentication Flow
- Successfully configured Keycloak as an Identity Provider
- Implemented OIDC-based authentication
- Established secure token-based communication between Keycloak and Grafana

### User Management
- Demonstrated user creation and management in Keycloak
- Implemented role-based access control
- Successfully mapped Keycloak roles to Grafana permissions

### Security Features
- Secure token handling with JWT
- Protected client authentication
- Role-based authorization
- User profile information mapping

### Testing and Verification
- Comprehensive testing using Postman
- JWT token verification using jwt.io
- User profile and role verification in both Keycloak and Grafana

This setup provides a foundation for implementing SSO in enterprise environments where centralized authentication and authorization are required. The documentation covers all essential aspects from initial setup to advanced role management, making it suitable for both development and production environments.

### Next Steps
Consider exploring:
- Adding more complex role mappings
- Implementing custom authentication flows
- Setting up multi-factor authentication
- Configuring additional client scopes and protocols
- Implementing group-based access control

For any issues or improvements, please feel free to contribute or raise an issue in the repository.
