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
* Identifier (Entity ID): http://localhost:5000/metadata/
* Reply URL (ACS): http://localhost:5000/acs/
* Sign-on URL: http://localhost:5000/login/

#### 4. Download Azure certificate, paste it into your settings.json under "idp" -> "x509cert"

#### 5. Assign users to the app under Users and groups
