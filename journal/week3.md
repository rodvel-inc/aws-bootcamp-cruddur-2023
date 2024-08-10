# Week 3 — Decentralized Authentication

Basically, it is the mechanism through which I am going to add the log-in, log-out, password reset functionalities to the Cruddur app. In the context of Amazon Cognito, I am going to add a **`User Pool`**.

The goal is, essentially, to integrate **Amazon Cognito** to the backend, with custom login pages, as oppossed to using the login page (a.k.a. **Hosted UI**) provided by AWS.

## Amazon Cognito User Pools

This service allows me to decide whether to use my application's new user pool as a **standalone user directory**, or if you will integrate it with a **federated identity provider**.

If I choose the first option, these are the functionalities that I will have at my disposal:

- I will choose how users will identify themselves; I can choose:
    - the user's email address, 
    - their phone number, or 
    - a unique name they enter for themselves.

If I choose the second option:

- the users can sign in using their existing identity data with Google, Facebook, Amazon, or Apple,
- they can also bring over user identity information from an internal directory that supports OIDC or SAML-based single sign-on.

### Sign-in experience

For the Bootcamp, I am going to create a **User Pool** as a **standalone user provider**, and I all configure it so that the users use their **username** and **email address** to sing-in.

### Sign-in security settings

I am using the default password policy, which is:

- Password minimum length: 
    - 8 character(s)
- Password requirements
    - Contains at least 1 number
    - Contains at least 1 uppercase letter
    - Contains at least 1 lowercase letter
- Temporary passwords set by administrators expire in
    - 7 day(s)

I am not goint to configure the MFA for my application users, although I have the choice to do it.

I am allowing my users to have the **Self-service account recovery** feature, and they will receive the **user account recovery messages** through **email only**.

### Sign-up experience

#### Self-registration

I have to determine how new users will verify their identities when signing up and which attributes should be required or optional during the user sign-up flow.

It is desirable for new users to self-register through the UI. I am choosing the **Enable self-registration** option. 

However, it is unclear whether this choice will allow me to include this feature on my application's UI, since I am not planning to use the Hosted UI. 

**`Important note`** The alternative to the self-registration option is to create new users from a federation, an administrative API from the federated user directory.

#### Attribute verification / user confirmation

I have to decide if Cognito will be in charge of verifying the user attributes for the account creation process. This is the required step before a user can actually sign-in.

I will use Cognito to do this for me, by having it automatically send email messages to verify and confirm.

When a user updates an email address or phone number attribute, Amazon Cognito marks it unverified until they verify the new value. 

Messages can’t be sent to an unverified email address or phone number. A user can’t sign in with an unverified alias attribute. 

I can choose how Amazon Cognito handles an updated email address or phone number after the update and before the verification. That is why I am choosing the **Keep original attribute value active when an update is pending** option. 

The active attribute that I will keep active is the user's email address.

Some user's attributes have to be required (i.e. not optional); I will have the users provide their names.

#### Configure message delivery

For a production environment, Amazon Cognito uses Amazon SES and Amazon SNS to send email and SMS messages to my app users. However, for development purposes I can send messages via Amazon Cognito instead, but I have to keep in mind that there is a 50 messages per day limit with this option.

#### Integrate my app

In this configuration step, I can set up the app integration for my user pool with Cognito's built-in authentication and authorization flows.

I am asked to configure an "Initial app client". What is this? The **`initial app client`** is like an intermediary that represents my web app, and it defines the way my web app will connect to the User Pool being created. This is essentia for the authentication and authorization processes.

There are two main options for the creation of this Initial app client: *public client* and *private client*.

**Public Client** 

- What It Is:
    - A "Public Client" is a type of client that cannot securely maintain secrets. 
    - This is typical for applications that run in the user’s browser, such as JavaScript web applications or mobile apps.

- Example Use Case:

    - JavaScript Web Application: 
        - Suppose you’re building a web application that runs in the user’s browser. 
        - This application communicates with Amazon Cognito’s backend to handle user sign-in and authentication. 
        - Since JavaScript code is visible to the user and can be tampered with, you cannot keep a secret securely in the client. 
        - Therefore, you choose a "Public Client."

- Characteristics:

    No Secret: No shared secret is used between the client and the server, as client-side code can be exposed and modified.
    Typical Use Cases: Single Page Applications (SPAs), JavaScript web apps, and mobile apps.

**Private client**

As oppossed to the Public Client, the Private Client can securely maintain secrets.

- Example Use Case:

    - Backend Application: 
        - Imagine you have a backend service that interacts with Amazon Cognito. 
        - In this case, you can securely store a secret (key) on your server, which is used to authenticate requests to Cognito. 
        - For example, a backend API that manages access tokens on behalf of your web application might use a "Confidential Client."

For the Bootcamp, I will choose the *Public Client*, since it fits my use case perfectly.

## Connecting Cruddur to Amazon Cognito

For this, I will use the **`AWS Amplify`** service.

### Why AWS Amplify?

- AWS Amplify is a set of tools and services (SDK, for instance) from AWS designed to help developers build and deploy web and mobile applications quickly. 

- It simplifies the development process by providing integrated solutions for both frontend and backend development.

- AWS Amplify helps me integrate the custom UI with an Amazon Cognito User Pool by providing tools and libraries to manage user authentication and sessions. 

- Amplify simplifies the connection between Cruddur's backend and Cognito, allowing me to focus on building my user experience rather than managing infrastructure.

- I will use Amplify Javascript library to accomplish this.

### AWS Amplify in Cruddur

Broadly speaking, these are the steps that should be followed in order to have Amplify help me connect the dots:

| **Step**                                | **Description**                                                                                                 |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| **1. Install Amplify CLI**              | Install AWS Amplify in the *frontend* folder:`npm i aws-amplify --save`              |
| **2. Import Amplify into the *app.js* file** | Import the object from the aws-amplify library: `import { Amplify } from 'aws-amplify';`              |
| **3. Connect Amplify with Cognito**        | Ensure that Amplify is correctly linked to my Cognito User Pool.                       |
| **4. Add the *auth* module to the *homefeedpage.js* file** | In order to handle authentication and authorization.        |
| **5. Create the sign-in page**         | Self-explanatory.                 |

### Summary of this section
At the frontend layer, at this point, I have implemented / tested successfully:
- A user pool in Cognito.
- A *public client* to integrate my app's frontend with the user pool. 
- A customized sign-up page.
- A customized sign-in page.
- The password recovery functionality.
- Storing locally the access token (jwToken,retrieved from Cognito).

### Next steps
Passing the jwToken, along with the API calls, to the backend layer.

## The logic behind this
In the file **`homefeedpage.js`** I have a configuration that is linked directly to the **/api/activities/home** API endpoint.

In this file, I am declaring that the jwToken must be passed along when the API endpoint call is made.

Other files, such as **`notificationsfeedpage.js`**, also have the same approach.

If a user is correctly authenticated by Cognito, the jwtoken is generated, and the content that is to be loaded when the authentication is successful, appears in the corresponding section in the frontend. 

But this content is loaded from the backend, requesting it with the 'GET' method.

How to connect the backend and the frontend?

### Enter CORS
One of the strangest concepts that I have come across in this bootcamp is CORS.

#### Cross-Origin Resource Sharing
- Imagine you are visiting a website, **`example.com`**.
- This site includes a script that wants to get data from another website, **`api.anothersite.com`**.
- Normally, browsers will block these kinds of cross-origin requests for security reasons to prevent malicious websites from stealing sensitive data.
- However, if **`api.anothersite.com`** wants to allow requests from **`example.com`**, it can do so by adding special headers to its responses.
- These headers tell the browser, "It is OK for **`example.com`** to access my data."

#### How it works:
1. Simple request:

If **`example.com`** makes a simple request to **`api.anothersite.com`**, and if this server has set the *Access-Control-Allow-Origin* header to **`*`** or to **`example.com`**, the browser will allow the request.

2. Preflight request:

- For more complex requests (e.g., ones with custom headers like **`Authorization`**), the browser first sends an "OPTIONS" request (a preflight request) to **`api.anothersite.com`** to check if it allows the actual request. 
- The server responds with headers like *Access-Control-Allow-Headers* and *Access-Control-Allow-Methods*, indicating which methods and headers are allowed.
- In my web application, respectively, I am allowing:
    - headers=['Content-Type', 'Authorization'],
    - methods="OPTIONS,GET,HEAD,POST"
- This means that every method will have to be authorized, by carrying the jwToken along with the API call, to be successful.











