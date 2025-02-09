# DevOps with Azure DevOps (.NET)

## Overview

In this lab, you will create a Azure DevOps repository, commit and push your code, create a Continuous Integration pipeline, and test your cloud-based application.

## Objectives

In this hands-on lab, you will learn how to:

* Create an ADO Git repository.
* Add your code to the ADO Git repository.
* Create a Continuous Integration (CI) pipeline.
* Deploy a built application to an Azure Web App from ADO.

## Prerequisites

* The source for the starter app is located in the [start](start) folder.
* There will be no code changes required so the the [end](end) folder will remain empty.
* Deployed the starter ARM Template [HOL 1](../01-developer-environment).
* Completion of the [HOL 3](../03-azuread-office365).

> &#x1F53A; **Note**: If you did not complete the previous labs, the project in the [start](start) folder is cumulative. But you need to add the previous HOL's settings to the `Web.config` file and make all necessary changes to Azure. &#x1F53A;
>
> &#x1F53A; If you did complete HOL 3 just continue with the same solution you have been using. &#x1F53A;

## Exercises

This hands-on-lab has the following exercises:

* [Exercise 1: Create ADO Git repository](#ex1)
* [Exercise 2: Add application to ADO Git](#ex2)
* [Exercise 3: Create a Continuous Integration pipeline](#ex3)
* [Exercise 4: Deploy code to an Azure Web App](#ex4)

---

## Exercise 1: Create ADO Git repository<a name="ex1"></a>

ADO gives us the option to use Git or [TFVC](https://www.visualstudio.com/en-us/docs/tfvc/overview) as our project's repository.
For this exercise we will use Git, and then clone the repository to our dev machine.

> Note that if you acquired these lab materials via a `git clone` of the workshop repo then you should select a folder somewhere else on your dev machine.
  This will minimize conflicts between the two separate repositories.

1. We will ignore the `devcamp123` project we created in HOL 1.
   Starting at your ADO account's landing page (`https://dev.azure.com/[YOUR_ACCOUNT_NAME]`), click `New Project`.

    ![image](./media/2019-10_01_01_ADO_home.png)

2. Enter a project name such as **DevCamp**, ensure `Version control` is set to `Git` and then click `Create`.

    ![image](./media/2019-10_01_02_ADO_new_proj.png)

3. Wait for the project to be created. 
   This process may take up to 60 seconds.
   When finished you will be redirected to the project page
    
    ![image](./media/2019-10_01_03_ADO_proj_hp.png)

4. Click `Dashboards` and explore your pre-built dashboard. Familiarize yourself with the variety of widgets available, and the customization options.

    ![image](./media/2019-10_01_04_ADO_proj_dashboard.png)

You have now created a project in ADO with a Git repository. Next we'll clone the repository locally to your developer machine and upload code from our machine to ADO.

---

## Exercise 2: Add application to ADO Git<a name="ex2"></a>

1. Click `Repos` on the left toolbar to navigate to the Code screen.

    ![image](./media/2019-10_02_01_ADO_proj_repos.png)

1. Click the `Clone in Visual Studio` button.

    ![image](./media/2019-10_02_02_ADO_proj_clone_vs.png)

    > **Note** if you are using Chrome, you may receive a pop-up message. 
      The Clone in Visual Studio option uses a custom protocol handler to open in the client. Select `Open Microsoft Visual Studio Handler Selector`.
    >
    > ![image](./media/2019-10_02_03_chrome_pick_app.png)
    >
    > The Internet Explorer will display a security warning. Click `Allow` to continue:
    >
    > ![image](./media/2017-06-21_14_08_30.png)

1. When Visual Studio launches, you will be prompted in the `Team Explorer` window to create a local folder to clone into. Select the `...` next to the local address:

    ![image](./media/2019-10_02_03_vs_team_explorer.png)

1. Create a new local folder ***outside*** of the GIT folder you have been using for the other HOLs. In the example below, the folder was created as `DevCampVSO`:

    ![image](./media/2017-06-21_14_14_00.png)

1. Click `OK` and `Clone`.

1. Open Windows Explorer and copy the files from the `start` folder in this HOL (you should have it at `C:\DevCamp\HOL\dotnet\04-devops-ci\start`) and copy the contents into the folder you created above.

      ![image](./media/2017-06-21_14_17_00.png)

1. Open the **copied solution** file in Visual Studio.

1. Ensure the `Web.config` settings are set to the values for your application (you can copy them from your working copy back from HOL 3 - or 2 if you completed the HOL 3 using the HOL 2 solution).

    ```xml
    <!--HOL 2-->
    <add key="INCIDENT_API_URL" value="API URL" />
    <add key="AZURE_STORAGE_ACCOUNT" value="STORAGEACCOUNT" />
    <add key="AZURE_STORAGE_ACCESS_KEY" value="STORAGEKEY" />
    <add key="AZURE_STORAGE_BLOB_CONTAINER" value="images" />
    <add key="AZURE_STORAGE_QUEUE" value="thumbnails" />
    <add key="REDISCACHE_HOSTNAME" value="" />
    <add key="REDISCACHE_PORT" value="6379" />
    <add key="REDISCACHE_SSLPORT" value="6380" />
    <add key="REDISCACHE_PRIMARY_KEY" value="" />
    <!--HOL 3-->
    <add key="AAD_APP_ID" value="" />
    <add key="AAD_APP_SECRET" value="" />
    <add key="AAD_APP_REDIRECTURI" value="" />
    <add key="AAD_INSTANCE" value="https://login.microsoftonline.com/{0}/{1}" />
    <add key="AAD_AUTHORITY" value="https://login.microsoftonline.com/common/" />
    <add key="AAD_LOGOUT_AUTHORITY" value="https://login.microsoftonline.com/common/oauth2/logout?post_logout_redirect_uri=" />
    <add key="AAD_GRAPH_SCOPES" value="openid email profile offline_access Mail.ReadWrite Mail.Send User.Read User.ReadBasic.All" />
    <add key="GRAPH_API_URL" value="https://graph.microsoft.com" />
    ```

1. You need to enable SSL again in the new project, and this will give you a different port number that you need to update both in the `Web.config` and in you application registration on the portal.

1. Compile the solution and make sure it works.

1. On the Team Explorer tab, select the changes view by clicking on `Changes`. You may be prompted to confirm your user name.

    ![image](./media/2017-06-21_14_21_00.png)

1. In the changes view, you will see a list of changes that are ready to commit to the local repository. In our case it will be the copied solution.

    ![image](./media/2017-06-21_14_24_00.png)

    > If you made changes to the files, and you do not see them in the list, ensure that you have cloned the repository from the repo.

1. Right click on the top level folder and select `Stage`:

    ![image](./media/2017-06-21_14_26_00.png)

1. Enter a comment for the check-in and click `Commit Staged`. Not that the comment is mandatory and the button becomes active only after you have entered a comment.

    ![image](./media/2017-06-21_14_28_00.png)

1. Because Git is a distributed source control system, the changes we commit are not visible to anyone else. Making our changes visible will require that we ***synchronize*** the repositories. This process is both a Git Pull (to receive changes from the remote repo to your local repo) and a Git Push to send changes from the local repository. We will perform a sync to pull the changes and push our changes. Click `Sync` in the commit message.

     ![image](./media/2017-06-21_14_29_00.png)

1. On the next screen, we will push our changes to the server. Since this is the first check-in, there are no incoming commits. Click the `Push` link below `Outgoing Commits`.

     ![image](./media/2017-06-21_14_30_00.png)

1. In your browser, navigate to the Azure DevOps site and view the committed files.

     ![image](./media/2019-10_02_04_ADO_committed.png)

1. If you want to return to the changes view of the Team Explorer to stage more commits, click the headline to open a drop-down of all the views.

     ![image](./media/2017-06-22_08_55_00.png)

---

## Exercise 3: Create a Continuous Integration pipeline<a name="ex3"></a>

With application code now uploaded to ADO, we can begin to create builds via a Build Definition. Navigate to the `Build` tab from the top navigation. We will use the hosted agent within ADO to process our builds in this exercise.

1. From the `Pipelines` menu select the `Builds` tab, and select the `New pipeline` button:

    ![image](./media/2019-10_03_01_Pipelines_Build_New.png)

2. Select `Azure Repos Git` as code hosting platform

    ![image](./media/2019-10_03_02_Pipelines_Build_New_Connect.png)

3. Then select `DevCamp` as repository

    ![image](./media/2019-10_03_03_Pipelines_Build_New_Select.png)

4. There are pre-built definitions for a variety of programming languages and application stacks. For this exercise select `.NET Desktop` and click `Apply`:

    ![image](./media/2019-10_03_04_Pipelines_Build_New_Configure.png)

5. The build tasks are created for us as part of the template and showed as yaml file in the `Review` step

    ![image](./media/2019-10_03_05_Pipelines_Build_New_Review.png)


6. Navigate to the `VSBuild@1` task. 
   Add the following in the `inputs` to create a web deployment package as part of the build:

    ```xml
    msbuildArgs: /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="bin\deploymentpackage"
    ```
    ![image](./media/2019-10_03_06_Pipelines_Build_New_VBuild.png)

7. Click on the `Show Assistant` on the right

    ![image](./media/2019-10_03_07_Pipelines_Build_New_Assistant.png)

8. Use the search box to search `copy` and select the `Copy Files` task

    ![image](./media/2019-10_03_08_Pipelines_Build_New_CopyFiles.png)

9. Fill the form using the following values and finally select `Add`:
   - Source Folder: `$(build.sourcesdirectory)`
   - Contents: `**\bin\**`
   - Target Folder: `$(build.artifactstagingdirectory)`

    ![image](./media/2019-10_03_09_Pipelines_Build_New_CopyFiles_Filled.png)

10. Add a `Publish build artifacts` Task after the `Copy Files` one
    
    ![image](./media/2019-10_03_10_Pipelines_Build_New_PublishBuildArtifacts.png)

11. Accept default and press `Add`

    ![image](./media/2019-10_03_11_Pipelines_Build_New_PublishBuildArtifacts_Add.png)

12. Click `Save and run`. Our saved Build Definition is ready to be processed by the Hosted Build Agent.

    ![image](./media/2019-10_03_12_Pipelines_Build_New_SaveAndRun.png)

13. Accept the defaults and click `Save and run`. 
    Your build will then be queued until the Hosted Build Agent can pick it up for processing. 
    This typically takes less than 60 seconds to begin.

    ![image](./media/2019-10_03_13_Pipelines_Build_New_SaveAndRun_Confirm.png)

14. You should be now automatically redirected to the started build.

    ![image](./media/2019-10_03_14_Pipelines_Build_Started.png)

15. Once your build completes, click each step and inspect the output.

    ![image](./media/2019-10_03_15_Pipelines_Build_Result.png)

    > ***Note:*** If your build fails go back to Visual Studio and make sure the solution can be build. Make any neccessary fixes and commit the changes by staging the changes, synchronizing and pushing.

16. Let's inspect the output artifacts that were published. 
    Click the `Artifacts` button in the top right angle of the screen the list of artifacts produced by the pipeline and click on `drop` (that is the name of the produced artifact).
    
    ![image](./media/2019-10_03_16_Pipelines_Build_Result_Artifact.png)

19. Expand the `drop` folder and view the build artifacts. Click `Close` when complete.

    ![image](./media/2019-10_03_17_Pipelines_Build_Result_ArtifactExplorer.png)

20. Click the `..` near the `drop` folder and select `Download as zip`

    ![image](./media/2019-10_03_18_Pipelines_Build_Result_ArtifactExplorer_Download.png)

21. Unzip `drop.zip` to see the application files created by the build agent. This artifact will be deployed to an Azure Web App in a later exercise.

We now have a Build Pipeline that will compile the application and create a package for deployment anytime code is pushed into the repository, or a manual build is queued.

---

## Exercise 4: Deploy code to an Azure Web App<a name="ex4"></a>

In the ARM Template that was originally deployed, a web app was created as a development environment to hold a deployed .NET application. We will use this web app as a deployment target from ADO. First, we need to prepare this web app for our application code.

1. Visit the Azure Web App by browsing to the [Azure Portal](http://portal.azure.com), opening the `Corso-MS-Cloud` Resource Group, and select the Azure Web App resource that begins with `dotnetapp[YOUR_NAME]` before the random string.

    ![image](./media/2019-10_04_01_AzurePortal_dotnetapp.png)

1. Once the blade expands, select `Browse` from the top toolbar:

    ![image](./media/2017-06-22_11_29_00.png)

1. A new browser tab will open with a splash screen visible. It will look similar to this image (it gets updated regularly with new information) and tell you that the app service has been created:

    ![image](./media/2019-10_04_02_Web_Service_Empty.png)

1. Now that we have a build being created and a website to deploy into, let's connect them.
   In ADO, navigate to the `Pipelines` tab.

1. Click on `Releases`.

1. Click the `New Pipeline` button to create a new release definition:

    ![image](./media/2019-10_04_03_ADO_Pipelines_Releases.png)

1. Select `Azure App Service deployment` and click `Apply`.

    ![image](./media/2019-10_04_04_ADO_Pipelines_Releases_New.png)

1. Click `Add artifact`. Ensure the `Source (Build pipeline)` is set to the Build Pipeline name used in the earlier exercise.
   Then click `Add` to finish creating the Release Pipeline.

    ![image](./media/2019-10_04_05_ADO_Pipelines_Releases_AddAnArtifact.png)

1. Now click on the Tab `Tasks` that should present a circular alert icon

    ![image](./media/2019-10_04_06_ADO_Pipelines_Releases_Tasks.png)

2. We need to connect your VS agent with your Azure subscription so it can deploy resources. Select `Tasks` from the menu. 

    1. In the dropdown near `Azure Subscription` Select the voice `Microsoft Azure Sponsorship`, click on the arrow next to `Authorize` and select `Advanced options` from the drop-down:

        ![image](./media/2018-07-16_10_03_28.png)

    2. Select `Corso-MS-Cloud` as resource group and click `Ok`:

        ![image](./media/2019-10_04_07_ADO_Pipelines_AzureResourceManagerServiceConnection.png)

    3. Select your `dotnetapp...` Azure Web app resource from the `App Service name` drop-down:

        ![image](./media/2019-10_04_08_ADO_Pipelines_Stage1.png)

    4. Move to step 11.

3. If the drop-down next to `Azure subscription` does not offer you your subscription or the drop-down next to `App Service name` does not offer you your Azure Web app resource (give it a moment after selecting the subscription:

    1. click on `Manage`:

        ![image](./media/2017-06-22_11_35_00.png)

    2. This will open a screen where you can connect to the ARM service endpoint. Select `New Service Endpoint` -> `Azure Resource Manager`.

        ![image](./media/image-042.gif)

    3. Provide a connection name and select your subscription then click `OK`.

        ![image](./media/image-025.gif)

        > If your subscription is not in the dropdown list, click the link at the bottom of the window, and the window
        > format will change to allow you to enter connection information on your subscription:

    4. Another option is to create a service principal. The steps to create a service principal is below.

        > If you have not created a service principal for the subscription, you will have to follow the
        > [instructions](https://go.microsoft.com/fwlink/?LinkID=623000&clcid=0x409) to do so.  This process will
        > provide the information to enter in this dialog:
        >

        1. Open [this PowerShell script](https://raw.githubusercontent.com/Microsoft/ADO-rm-documentation/master/Azure/SPNCreation.ps1) in your browser. Select all the content from the window and copy to the clipboard.

        2. Open a PowerShell ISE window. In the text window, paste the PowerShell script from the clipboard.

            ![image](./media/image-044a.gif)

        3. Click the green arrow to run the PowerShell script.

            ![image](./media/image-045a.gif)

            > If the PowerShell gives an error at runtime regarding a missing AzureRM module, please install it by executing the following command in a PowerShell window with admin privileges: `Install-Module AzureRM`. Then run `Set-ExecutionPolicy RemoteSigned -Scope process` to adjust the execution level

        4. The PowerShell script will ask for your **subscription name** and a **password**. This password is for the service principal only, not the password for your subscription. So you can use whatever password you would like, just remember it.

            ![image](./media/image-046a.gif)

        5. You will then be asked for your Azure login credentials. Enter your Azure username and password. The script will print out several values that you will need to enter into the `Add Azure Resource Manager Service Endpoint` window. Copy and paste these values from the PowerShell window:

            * Subscription ID
            * Subscription Name
            * Service Principal Client ID
            * Service Principal Key
            * Tenant ID

            1. Also, enter a user-friendly name to use when referring to this service endpoint connection.

                ![image](./media/image-047a.gif)

        6. Click `Verify connection`, and ensure that the window indicates that the connection was verified. Then Click `OK` and `Close`.

            ![image](./media/image-048a.gif)

        7. If this is the first time you are connecting to this subscription, you will need to authorize ADO to have access to deploy to Azure. After you select your subscription, click `Authorize`.

        ![image](./media/image-067.gif)

    5. Navigate back to the ADO build tab in the browser and click the click the `Refresh` icon to refresh the connections. The `Azure` connection that we setup should now appear. Select it.

    6. Next, for `App Service Name` choose the name of the .NET Azure Web App. It may take a moment to populate.

        ![image](./media/image-062.gif)

4. From the menu bar select `Save` to save the Release Definition, and select `Release` -> `Create Release`.

    ![image](./media/2019-10_04_09_ADO_Pipelines_Release.png)

5. Enter the release information and select the build to deploy. Ensure that the latest successful build is selected from the drop-down box. Click `Create`.

    ![image](./media/2019-10_04_10_ADO_Pipelines_Create_Release.png)

6. Click on the release number in navigation header. This will allow you view the current release information.

    ![image](./media/2019-10_04_11_ADO_Pipelines_Release_Created.png)

7. After a successful build you should see the application deployed to your web app.

    ![image](./media/2019-10_04_12_ADO_Pipelines_Release_Released.png)

8. Update the application configuration to match the current settings you have deployed.
   Copy the values from the `Web.config` to the Application configuration in Azure if they do not match.
   Open the [Azure portal](https://www.portal.azure.com) and find the .NET web application.

9.  In the `dotnetapp[YOU_NAME]...` details on [Azure portal](https://www.portal.azure.com), select the `Configuration` tab and update the values

     ![image](./media/2019-10_04_13_AzurePortal_dotnetapp_Configuration.png)

10. Copy the values from the `Web.config` into the application settings (you also need to include a new key named `AAD_APP_REDIRECTURI` and set the value to the URL on the application in Azure `https://dotnetapp[YOUR_ACCOUT_NAME][...].azurewebsites.net/`). If you do not have values for these settings, please review the previous labs for the correct values.

11. Click `Save`.

    ![image](./media/2019-10_04_14_AzurePortal_dotnetapp_Configuration_Save.png)

12. Navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).

13. Open the configuration for your application and add the Azure web application URL to the list of Redirect URLs. Click `Save`.

    ![image](./media/2019-10_04_15_AzurePortal_CityPowerAuthentication_AddURI.png)

    > Note: Be sure to include the trailing slash **/** in the URL and ensure this address is using **https**.

14. Open a browser and navigate to the site. You should see the running site on Azure.

    ![image](./media/2019-10_04_16_AzurePortal_CityPowerDeployed.png)

    > If your browser displays the error `Could not load file or assembly 'System.IdentityModel.Tokens.Jwt' [...]` you have to downgrade the NuGet package `System.IdentityModel.Tokens.Jwt` to 4.0.3 in Visual Studio.
---

## Summary

In this hands-on lab, you learned how to:

* Create a ADO Git repository that you utilized to synchronize your source code on your machine and in the cloud.
* Add your code to the ADO Git repository.
* Create a Continuous Integration pipeline that you used to automatically compile your application and create packages for deployment anytime code is checked into the repository.
* Deploy a built application to an Azure Web App from ADO and thus automating the final steps of your deployment process.

After completing this module, you can continue on to Module 5: ARM.

### View Module 5 instructions for [.NET](../05-arm-cd)

---
Copyright 2018 Microsoft Corporation. All rights reserved. Except where otherwise noted, these materials are licensed under the terms of the MIT License. You may use them according to the license as is most appropriate for your project. The terms of this license can be found at <https://opensource.org/licenses/MIT>.