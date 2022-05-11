# SAML/SSO Integration
## Configure storefront SSO
### Setting up storefront SSO involves the following steps:

#### 1. Configure Commerce storefront SSO settings
            Method: PUT
            URL: /ccadmin/v1/merchant/samlSettings
            Header: {"X-CCAsset-Language":"en","x-ccsite":"500002"}
            Payload:
            {
                "nameIdPolicyFormat": "urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified",
                "requireEncryptedAssertions": false,
                "requireSignedResponse": false,
                "signAuthnRequest": false,
                "nameIdPolicyAllowCreate": true,
                "enabled": true
            }
#### 2. Download the service provider entity descriptors
            Method: GET
            URL: /ccadmin/v1/merchant/samlSettings
            Header: {"X-CCAsset-Language":"en","x-ccsite":"500002"}
            Response:
            {
                "spEntityDescriptor": "<service provider entity descriptor>",
                "links": [
                    {
                        "rel": "self",
                        "href": "http://myserver.example.com:7002/ccstore/v1/merchant/samlSettings"
                    }
                ]
            }

#### 3. Register the service providers with the identity provider
        a. Register the service providers either by uploading the service provider entity descriptor documents that you downloaded from Commerce in above step.
        b. “spEntityDescriptor” contains base64 encoded data. Decode it using base64 decoder and then upload it in IDCS.

#### 4. Download identity provider entity descriptor form IDCS
#### 5. Upload the identity provider entity descriptor to Commerce
            Method: PUT
            URL: /ccadmin/v1/samlIdentityProviders/default
            Header: {"X-CCAsset-Language":"en","x-ccsite":"500002"}
            Payload:
            {
                "roleAttributeName": "",
                "organizationAttributeName": "",
                "loginAttributeName": "login",
                "emailAttributeName": "email",
                "encodedIdpMetadata": <encoded identity provider entity descriptor>,
                "fallbackToB2cUserCreation": true,
                "requiredAttributeToPropertyMap": {
                    "login": "login",
                    "email": "email"
                },
                "optionalAttributeToPropertyMap": {
                    "firstName": "firstName",
                    "lastName": "lastName",
                    "diabeticInfoPromotionOptin": "cxde_receiveEmail",
                    "cxde_termsConditions": "cxde_termsConditions"
                }
            }
            
#### 6. Configure CORS
        Method: PUT
        URL: /ccadmin/v1/sites/500002
        Header: {"X-CCAsset-Language":"en","x-ccsite":"500002"}
        Payload:
            {
                "properties": {
                    "allowedOriginMethods": {
                        "http://www.myIdentityProvider.com": "POST"
                    }
                }
            }

#### 7. Storefront Widgets and JavaScript files need to be updated
    |SrNo                       |Widget
    |------------------------------|--------------------------------------------
    1 | widget\cxots-myaccount-profile-widget\js\cxots-myaccount-profile-widget.js
    2 | widget\OTS Header\js\otsheader.js
    3 | widget\OTS Header\instances\OTS Header\display.template
    4 | widget\cxots-secondary-navigation-widget\instances\cxots-Myaccount-Leftnav-Widget\display.template
    5 | widget\cxots-secondary-navigation-widget\js\cxots-secondary-navigation-widget.js
    6 | global\cxots_global_application.js
    7 | widget\cxots-cart-summary-promo-widget\js\cxots-cart-summary-promo-widget.js
    8 | widget\cxotpp-loginlanding-widget\instances\cxotpp_login_registration\display.template
    9 | widget\cxotpp-loginlanding-widget\js\cxotpp-loginlanding-widget.js
    10 | widget\cxots-myaccount-landing-widget\js\cxots-myaccount-landing-widget.js
    11 | widget\cxots-myaccount-landing-widget\instances\cxots-Myaccount-Landing-Widget\display.template

