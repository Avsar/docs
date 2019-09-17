---
title: "Mendix Single Sign-On"
parent: "mendix-cloud-deploy"
menu_order: 70
description: "Use the MendixSSO module to add Single Sign-on to your app using the user's Mendix credentials"
tags: ["SSO", "Single Sign On", "Mendix credentials"]
#Ownership claimed by Identity Services Team.
#Needs to be rewritten to remove AppCloud references and just concentrate on SSO. Also needs to be tested.
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## 1 Introduction

{{% todo %}}[Add AppStore Link - several instances]{{% /todo %}}

The [MendixSSO module](https://appstore.home.mendix.com) allows you to specify that users of your app can be authenticated using their Mendix account credentials when your app is deployed to Mendix Cloud v4.

Using Mendix accounts means that you do not need a special authorization module to support resetting and changing passwords, and makes it easy to create multiple applications which all use the same sign-on mechanism.

Mendix starter apps come with the Mendix SSO module already available. All you have to do is to set your security level to **Production** and your end-users will be able to sign on.

If your app does not have the MendixSSO module, it is available from the Mendix App Store here: [Mendix SSO](https://appstore.home.mendix.com). Follow the instructions in [How To Use App Store Content](/developerportal/app-store/app-store-content) to import it into your app and then follow the instructions in [Setting Up Mendix Single Sign-On](#setting-up), below.

If you need a newer version of the Mendix SSO module (for example, to use a new feature), then it is also available from the App Store via the same link.

You can see if your app has the Mendix SSO module by looking in the **App Store modules** section in the **Project Explorer** for your app project.

![](attachments/mendix-sso/mxsso-app-store-module.png)

## 2 Using Mendix Single Sign-On

{{% alert type="warning" %}}
Mendix Single Sign-On is activated when your app is deployed to Mendix Cloud v4. When you run your app locally, or on another cloud, you will need to use local credentials.
{{% /alert %}}

### 2.1 Signing On as an End-user

When you start an app as an end-user, and do not have a current token for access to Mendix *or* have not signed in to this environment before, you will see the Mendix SSO sign-on screen. You can choose to sign-on with local credentials, but to use Mendix SSO, click **Or sign in with** > **Mendix Account**.

![Mendix SSO sign-on screen](attachments/mendix-sso/sso-sign-on.png)

For each environment (for example, myapp running in acceptance), the first time you sign on as an end-user using Mendix SSO you will be asked to authorize access. This means that the app can access information held in your Mendix profile.

![Authorization screen](attachments/mendix-sso/authorize-access.png)

You have to click **Authorize** to continue using Mendix SSO.

If you are already signed-in to Mendix, the app will automatically sign you in.

If you are not already signed-in to Mendix, you will need to sign in to Mendix.

### 2.2 Assigning End-user Roles

Giving end-users access to your app is done through the [Manage App Users](/developerportal/settings/general-settings#managing-app-users) page of the Developer Portal. You can get to this page in the Developer Portal from the **General** page of your app.

![General page for the app in Developer Portal](attachments/mendix-sso/manage-app-users.png)

{{% alert type="warning" %}}
Do not attempt to add or delete Mendix SSO users using administration functions within the app. If user access is not modified through the Developer Portal, then user access to your app will not be changed.
{{% /alert %}}

## 3 Removing Mendix Single Sign-On

If you have an app which already has Mendix SSO activated, you may wish to remove it.

### 3.1 Deactivating Mendix Single Sign-On

You can deactivate Mendix SSO in two simple steps. This will remove the ability for end-users to sign on with their Mendix account, but will leave the local user administration functions of MendixSSO intact.

The two steps are:

1. Rename the original login file (by default **login.html.old**) in the **theme** folder of your project to **login.html** – this removes the single sign-on button from your login screen
    * Open your project directory in File Explorer by selecting the menu item **Project** > **Show Project Directory in Explorer**

        ![Show project directory](attachments/mendix-sso/show-project-directory.png)

    * Go to the **theme** folder
    * Rename **login.html** to **login-with-sso.html**
    * Rename **login.html.old** to **login.html**

        ![File explorer showing two login files](attachments/mendix-sso/theme-folder-remove.png)

2. Remove the microflow **AfterStartup_MendixSSO** as the **After startup** microflow.
    * Open **Project Settings** from the **Project Explorer**
    * Click the **Runtime** tab
    * Click **Select…** for the **After startup** microflow
    * Click **None**
        ![Setting after startup microflow to none](attachments/mendix-sso/after-startup-remove.png)
    * Click **OK** to close the **Project Settings**
    {{% alert type="info" %}}If there is a different After startup microflow, you should not remove it, but rather remove the AfterStartup_MendixSSO microflow which is an action in the existing microflow{{% /alert %}}

Mendix SSO is now deactivated.

### 3.2 Removing Mendix Single Sign-On



## 4 Setting Up Mendix Single Sign-On{#setting-up}

{{% alert type="info" %}}
These instructions are for apps which did not originally have the MendixSSO module. For example, if you have an existing app which did not have the MendixSSO App Store module.

You do not have to follow these steps for apps (for example, starter apps) which already have Mendix SSO, or if you are upgrading an existing MendixSSO module to a newer version.
{{% /alert %}}

To enable Mendix SSO in your app, if it does not have it set up already, you need to follow the following steps:

1. Import the MendixSSO module from the [Mendix App Store](https://appstore.home.mendix.com).

2. Add the microflow **AfterStartup_MendixSSO** to the **After startup** microflow.
    * Open **Project Settings** from the **Project Explorer**
    * Click the **Runtime** tab
    * Click **Select…** for the **After startup** microflow
    * Choose the microflow **App Store modules** > **MendixSSO** > **Public** > **Default Implementation** > **Microflows** > **AfterStartup_MendixSSO** (you can use the filter to find it quickly) and click **Select**
        ![](attachments/mendix-sso/after-startup.png)
    * Click **OK** to close the **Project Settings**
    {{% alert type="info" %}}If there is already an After startup microflow, you should not replace it, but rather add the AfterStartup_MendixSSO microflow as the first action in the existing microflow{{% /alert %}}

3. Add the pages **MyAccountViewEdit** and **UserOverview** to the app navigation.
    * Open **Navigation** from the **Project Explorer**
    * Click **New item** to add a new navigation item
    * Enter the following values and click **OK**
        * **Caption** – *My Account*
        * **Icon** – *Glyphicon 'user'* (click **Select…** and search for `user`)
        * **On click** – *Show a page* **App Store modules** > **MendixSSO** > **Public** > **Default Implementation** > **Pages** > **User** > **MyAccountViewEdit**
    * Repeat the above to add another **New item** with the values
        * **Caption** – *User Overview*
        * **Icon** – *Glyphicon 'lock'*
        * **On click** – *Show a page* **App Store modules** > **MendixSSO** > **Public** > **Default Implementation** > **Pages** > **Admin** > **UserOverview**

4. Turn on **Production** security level and give **User roles** *User* and *Administrator* access to the **MendixSSO** module.
    * Open **Project Security** from the **Project Explorer**
    * Set **Security level** to **Production**
    * Switch to the **User roles** tab
    * Select the **Administrator** user role and click **Edit**
    * Click **Edit** next to **Module roles**
    * Select the **Administrator** module role for **App Store modules** > **MendixSSO**
        ![Set Administrator module role](attachments/mendix-sso/set-module-role.png)
    * Click **OK** twice to return to **Project Security**
    * Repeat the steps above to add the MendixSSO.User module role to the **User** user role

        The Project security settings now contains these two additional module roles:

        ![Confirmation of user roles](attachments/mendix-sso/module-user-roles.png)

5. Rename the file **login-with-sso.html** in the **theme** folder of your project to **login.html** – this adds the single sign-on button to your login screen
    * Open your project directory in File Explorer by selecting the menu item **Project** > **Show Project Directory in Explorer**
        ![Show project directory](attachments/mendix-sso/show-project-directory.png)
    * Go to the **theme** folder
    * Rename **login.html** to something else (for example *login.html.old*)
    * Rename **login-with-sso.html** to **login.html**
        ![file explorer showing two login files](attachments/mendix-sso/theme-folder.png)

Your app is now configured to use Mendix Single Sign-on when it is deployed to the Cloud.

## 5 Modifying Mendix Single Sign-On

The administrative functions provided in the MendixSSO module are sufficient for administering most apps. However, you can create your own administration suite by adapting it.

For more information see [Modifying Mendix SSO](modifying-mendix-sso).

This document also contains more information about token management, to help you to debug any issues and provide information to Mendix support if necessary.