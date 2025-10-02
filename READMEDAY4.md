### Simple App with SAML Authentication Via Azure Entra
#### 1.  Install flask and python saml tool kit using pip
`pip install flask`  
`pip install python3-saml`  

#### 2. Clone the Offline Application  
`git clone https://github.com/devopswork4u/saml-demo-app.git`  

* Replace the certs under cert with new cert by creating below  
  `cd saml-demo-app/saml/certs`  
  `openssl req -newkey rsa:2048 -nodes -keyout sp.key -x509 -days 365 -out sp.crt`  

#### 3. Azure Entra (Azure AD) Configuration Steps
##### A. Create Enterprise Application
* Go to Azure Portal → Entra ID → Enterprise Applications
* Click + New Application
* Choose Non-gallery application, name it Flask SAML Demo
##### B. Configure Single Sign-On
* Select Single sign-on → SAML
* Basic SAML Configuration:
* Identifier (Entity ID): http://localhost:4242/metadata/
* Reply URL (ACS): http://localhost:4242/acs/
* Sign-on URL: http://localhost:4242/login/

Note: Update the port and URL in https://github.com/devopswork4u/saml-demo-app/blob/main/saml/settings.json

#### 4. Download Azure certificate, paste it into your settings.json under "idp" -> "x509cert"

#### 5. Assign users to the app under Users and groups


### Flow Authentioncation Flow

sequenceDiagram
    participant User
    participant App as Application (SP)
    participant Azure as Azure (IdP)

    App: 1. User tries to access a secured page on the application.
      App->>User: 2. Redirects to Azure for authentication.
        User->>Azure: 3. Sends a SAML authentication request to Azure.
          Azure->>User: 4. Prompts user for credentials (e.g., username and password).
            User->>Azure: 5. User provides credentials.
              Azure->>Azure: 6. Validates credentials and checks security policies (e.g., MFA).
                Azure->>App: 7. Sends a signed SAML response containing an assertion and user attributes.
                  App->>App: 8. Validates the SAML assertion with Azure's public certificate.
                    App->>User: 9. Grants access to the user and redirects to the original requested page.

