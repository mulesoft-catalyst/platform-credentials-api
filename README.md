# Platform Credentials API

# Introduction

Organization credentials provide a way to uniquely identify a specific environment, an organization, or a business group when linking Mule runtime engine to an organization using Anypoint Platform. Credentials for environments, organizations, and business groups consist of two keys: the client ID and a client secret. Mule uses these credentials to connect to and access your organization.

Currently to get Organization credentials developers need to work with their Leads/Product Owners who have Org admin permissions. Since Product Owners in some organization may not be familiar with Anypoint platform it may be challenging for them to be able to provide developers with details they need to perform their day to day tasks. This API provides a self-service way for developers to obtain platform credentials.

# Prerequisite

The API has following prerequisites - 

- The developer should have Create Application permission to the environment
- The developer needs to pass Anypoint platform Bearer token

# How to obtain Anypoint platform Bearer token?

Some Organization uses SSO for logging into Anypoint platform, MuleSoft developer would have to exchange the SAML token for
Anypoint platform Bearer token. The steps are mentioned in below article 

[https://docs.mulesoft.com/access-management/saml-bearer-token](https://docs.mulesoft.com/access-management/saml-bearer-token)

Another way developer can obtain the Bearer token is using Developer Tools present in Browsers like Chrome and Firefox.
Below example uses Chrome 

![platform-creds-1](https://user-images.githubusercontent.com/44620039/190283066-2da17547-7aa6-4281-a99e-6290b354d7bd.png)

![platform-creds-2](https://user-images.githubusercontent.com/44620039/190283075-8f4bf0b4-cfa4-4e80-8155-e4a9dbd96d35.png)

Click on Runtime Manager and look at network traffic in Developer Tool to find one of the calls to [https://anypoint.mulesoft.com](https://anypoint.mulesoft.com) url. Under headers tab 

![platform-creds-3](https://user-images.githubusercontent.com/44620039/190283099-9cbd87f8-5227-4aae-9e33-d3af61246a7b.png)

# How this APP works?

This Mule app has two main components:
- The bearer token passed by the user in header is used to find the user's permissions. It looks for environments where user has access to create mule applications.
- The mule application needs a service account with access to retrieve client id and client secret for the environments for which user has create application access.
    - The service account details should be provided in encrypted form (Secure Properties) in secure-props.yaml file under src/main/resources folder
    ```
    auth:
        username: "[YOUR username]"
        password: "[YOUR password]"
    ```


# Sample Request and Response

Request

```
curl https://{{APP-URI-BASE-PATH}}/api/platform/credentials \
-H 'authorization: bearer [YOUR bearer_token]'
```

Response

```
[
  {
    "envDetails": {
      "environment_name": "Sandbox",
      "environment_id": "[YOUR env_id]",
      "organization_name": "Solution Engineering",
      "organization_id": "[YOUR org_id]"
    },
    "platformCredentials": {
      "environment": {
        "clientId": "[YOUR cliendId]",
        "clientSecret": "[YOUR clientSecret]"
      }
    }
  },
  {
    "envDetails": {
      "environment_name": "Non-Prod",
      "environment_id": "[YOUR env_id]",
      "organization_name": "Solution Engineering",
      "organization_id": "[YOUR org_id]"
    },
    "platformCredentials": {
      "environment": {
        "clientId": "[YOUR cliendId]",
        "clientSecret": "[YOUR clientSecret]"
      }
    }
  },
  {
    "envDetails": {
      "environment_name": "MS10-NonProd",
      "environment_id": "[YOUR env_id]",
      "organization_name": "Demo_BG",
      "organization_id": "[YOUR org_id]"
    },
    "platformCredentials": {
      "environment": {
        "clientId": "[YOUR cliendId]",
        "clientSecret": "[YOUR clientSecret]"
      }
    }
  },
  {
    "envDetails": {
      "environment_name": "MS11-NonProd",
      "environment_id": "[YOUR env_id]",
      "organization_name": "Demo_BG",
      "organization_id": "[YOUR org_id]"
    },
    "platformCredentials": {
      "environment": {
        "clientId": "[YOUR cliendId]",
        "clientSecret": "[YOUR clientSecret]"
      }
    }
  }
]
```
