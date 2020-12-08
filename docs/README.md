# Configuring Yandex Authenticator

This page provides instructions on how to configure the Yandex 
authenticator and the WSO2 Identity Server using a sample application

>The Yandex Authenticator is supported from WSO2 Identity Server 5.10.0 onwards.

Follow the steps given below to set this up.

* [Step 1 - Register WSO2 IS in Yandex](#step-1---register-wso2-is-in-yandex)
* [Step 2 - Deploy the Pickup Dispatch Sample Web App](#step-2---deploy-the-pickup-dispatch-sample-web-app)
* [Step 3 - Configure the Identity Provider (IdP)](#step-3---configure-the-identity-provider-idp)
* [Step 4 - Configure the Service Provider](#step-4---configure-the-service-provider)
* [Step 5 - Test the Sample Application](#step-5---test-the-sample-application)

## Step 1 - Register WSO2 IS in Yandex

1. Go to https://oauth.yandex.ru/client/new and login with your Yandex credentials
2. Enter the following details and obtain client ID and client Secret.
![yandex-app-creation-01](img/yandex-app-creation-01.png)
![yandex-app-creation-02](img/yandex-app-creation-02.png)
![yandex-app-creation-03](img/yandex-app-creation-03.png)
![yandex-app-creation-04](img/yandex-app-creation-04.png)
![yandex-app-creation-05](img/yandex-app-creation-05.png)


    
## Step 2 - Deploy the Pickup Dispatch Sample Web App

The next step is to deploy the Pickup Dispatch sample web app in order to use it in this scenario.

To configure this, see [saml2-web-app-pickup-dispatch 
webapp](https://is.docs.wso2.com/en/5.9.0/learn/deploying-the-sample-app/#deploying-the-saml2-web-app-pickup-dispatch-webapp).

## Step 3 - Configure the Identity Provider (IdP)

Next, add a new identity provider in WSO2 Identity Server, which was started in step 01 by [adding a new identity 
provider](https://is.docs.wso2.com/en/5.9.0/learn/adding-and-configuring-an-identity-provider/#adding-and-configuring-an-identity-provider).

1. Log in to the [Management Console](https://is.docs.wso2.com/en/5.9.0/setup/getting-started-with-the-management-console/) 
as an administrator.

2. In the **Identity Providers** section under the **Main** tab of the management console, click **Add**.

3. Enter a suitable name as the **Identity Provider Name** (e.g., YandexIDP).
    >As our resident Identity Provider is
 WSO2 IS, the Alias will appear as follows - `https://(host-name):(port)/oauth2/token`
   
4. Navigate to the **Yandex Configurations** under **Federated Authenticators**.

5. Enter the IdP related details as follows.

| Field | Description | Sample Value |
| ------------- | ------------- |  ------------- | 
|Enable	|Selecting this option enables Yandex Identity Provider to be used as an authenticator for users provisioned to the Identity Server|Enable|
|Default|Selecting the Default checkbox signifies that Yandex is the main/default form of authentication. This removes the selection made for any other Default checkboxes for other authenticators.|Enable|
|Client Id	|This is a unique public identifier for apps which is usually given as a 32-character hex string. Enter the client ID of the app that you created in IdP.|81b05d91toz66e|
|Client Secret|	This is a secret known only to the application and the authorization server. Enter the client secret of the app that you created in IdP.|otYR21HMW1PchfwZ |
|Callback URL|	This is the URL to which the browser should be redirected after the authentication is successful. It should have this format: https://(host-name):(port)/commonauth	| https://localhost:9444/oauth2/authorize|

    
![is-5100-add-idp-page](img/is-5100-add-idp-page.png)
    
* Click on **Register**.

You have now added the identity provider.

## Step 4 - Configure the Service Provider

The next step is to configure the service provider in WSO2 IS.

>  If you have created a service provider already in Step 2, Skip to step 5.f.

1. Return to the management console.

2. In the **Service Providers** section under the **Main** tab, click **Add**.

3. As you are using **saml2-web-app-pickup-dispatch** as the sample, enter **saml2-web-app-pickup-dispatch.com** in the **Service Provider
 Name** text box and click **Register**.

4. In the **Inbound Authentication Configuration** section, click **SAML2 Web SSO Configuration**, and then click **Configure**.

5. Add the service provider details as follows.
    
    a. Select Mode: Manual Configuration
       
      > For more information on the SAML2 Web Single-Sign-On Configuration methods, see [Configuring SAML2 Web Single
       -Sign-On](https://is.docs.wso2.com/en/latest/learn/configuring-single-sign-on-saml/) in the WSO2 IS 5.10.0
       guide.
    
    b. Issuer: saml2-web-app-pickup-dispatch
    
    c. Assertion Consumer URL: Enter `http://localhost.com:8080/saml2-web-app-pickup-dispatch.com/home.jsp` and click
     **Add**.
    
    d. Select the following check-boxes.
    
        * Enable Response Signing.
        * Enable Single Logout.
        * Enable Attribute Profile.
        * Include Attributes in the Response Always.
    
   ![is-5100-add-sp-conf-page](img/is-5100-add-sp-conf-page.png)
    
    e. Click **Register** to save the changes. Now you will be sent back to the **Service Providers** page.
    
    f. Go to the **Local and Outbound Authentication Configuration** section.
    
    g. Configure the Local and Outbound Authentication for Yandex.
    
    > For more information, see Configuring [Local and Outbound Authentication for a Service 
    Provider](https://is.docs.wso2.com/en/latest/learn/configuring-local-and-outbound-authentication-for-a-service
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        >-provider/) 
    in the WSO2 Identity 5.10.0 guide.

        I. Click on the **Federated Authentication** radio button.
        II. Select the identity provider you created from the drop-down list under **Federated Authentication**. 
        III. Select the following options.
            * Use tenant domain in local subject identifier.
            * Use user store domain in local subject identifier.
            
    ![is-5100-add-sp-local-out-auth-page](img/is-5100-add-sp-local-out-auth-page.png)
            
    h. Click **Update** to save the changes.
    

## Step 5 - Test the Sample Application

1. To test the sample, go to the following URL: 
`http://<TOMCAT_HOST>:<TOMCAT_PORT>/saml2-web-app-pickup-dispatch.com/index.jsp`.
    
    Example: `http://localhost:8080/saml2-web-app-pickup-dispatch.com/index.jsp`
    
    ![saml2-web-app-pickup-dispatch-login-page](img/pickup-dispatch-login-page.png)

2. Click the link to log in with SAML from WSO2 Identity Server. You can use either the **Redirect Binding** or the **Post
 Binding** option.
 
3. You will be redirected to the Yandex sign in page. Enter your Yandex account credentials.
    
4. Give the permission to the created Yandex application to access the yandex account and get the required claims.

5. Give the consents to get the displayed claims to WSO2 Identity Server and Approve it.
    
6. After you are successfully authenticated, you will be taken to the home page of the saml2-web-app-pickup-dispatch.com
 app.

    ![yandex-login](img/yandex-login.jpeg)