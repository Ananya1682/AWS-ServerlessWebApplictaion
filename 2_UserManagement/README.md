# Module 2: User Authentication and Registration with Amazon Cognito User Pools

In this module you'll create an [Amazon Cognito][cognito] user pool to manage your users' accounts. You'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

## Architecture Overview

When users visit your website they will first register a new user account. For the purposes of this workshop we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console if you want to use fake email addresses for testing.

After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.

![Authentication architecture](../images/authentication-architecture.png)

## Implementation Instructions

:heavy_exclamation_mark: Ensure you've completed the [Static Web hosting][static-web-hosting] step before beginning
the workshop.

Each of the following sections provides an implementation overview and detailed, step-by-step instructions. The overview should provide enough context for you to complete the implementation if you're already familiar with the AWS Management Console or you want to explore the services yourself without following a walkthrough.

### 1. Create an Amazon Cognito User Pool

#### Background

Amazon Cognito provides two different mechanisms for authenticating users. You can use Cognito User Pools to add sign-up and sign-in functionality to your application or use Cognito Identity Pools to authenticate users through social identity providers such as Facebook, Twitter, or Amazon, with SAML identity solutions, or by using your own identity system. For this module you'll use a user pool as the backend for the provided registration and sign-in pages.

Use the Amazon Cognito console to create a new user pool using the default settings. Once your pool is created, note the Pool Id. You'll use this value in a later section.

**:white_check_mark: Step-by-step directions**

Set Up Amazon Cognito for User Authentication
Where to Perform: AWS Management Console

Log in to the AWS[Amazon Cognito Console][cognito-console]

Log in to the AWS Management Console
Search for Cognito in the services search bar and select it.
Create a User Pool:

Click on **Manage User Pools**.
Click **Create a user pool**.
Name your pool (ToDoAppUserPool).
### Configure Sign-Up and Sign-In:

Attributes: Ensure Email is selected as a required attribute.
Policies: Set password strength as per your requirements.
MFA and Verification: Configure Multi-Factor Authentication if desired.
App Clients:
Add an app client without generating a client secret (suitable for frontend applications).
Note down the App Client ID for later use.
Review and Create:

Review your settings and click "Create pool".
### Domain Setup:

In your user pool, navigate to **Domain name** under "App integration".
Set a domain prefix (e.g., todousers) to create a hosted UI for authentication.
Note Important Information:
You can find the value for `userPoolClientId` by selecting **App clients** from the left navigation bar. Use the value from the **App client id** field for the app you created in the previous section.

    ![Pool ID](../images/client-id.png)

    The value for `region` should be the AWS Region code where you created your user pool. E.g. `us-east-1` for the N. Virginia Region, or `us-west-2` for the Oregon Region. If you're not sure which code to use, you can look at the Pool ARN value on the Pool details page. The Region code is the part of the ARN immediately after `arn:aws:cognito-idp:`.

    The updated config.js file should look like this. Note that the actual values for your file will be different:
    ```JavaScript
    window._config = {
        cognito: {
            userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
            userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
            region: 'us-west-2' // e.g. us-east-2
        },
        api: {
            invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod,
        }
    };
    ```
1. Save the modified file making sure the filename is still `config.js`.
1. Commit the changes to your git repository:
    ```
    $ git add js/config.js 
    $ git commit -m "configure cognito"
    $ git push
    ...

User Pool ID: Found under "General settings".
App Client ID: As noted earlier.
Hosted UI URL: Constructed using your domain prefix, region, and app client ID.
### Summary:
Action: Create and configure an Amazon Cognito User Pool.
Location: AWS Management Console.
Outcome: Enables user authentication for your application.

static-web-hosting]: ../1_StaticWebHosting/
[amplify-console]: https://aws.amazon.com/amplify/console/
[cognito]: https://aws.amazon.com/cognito/

[serverless-backend]: ../3_ServerlessBackend/