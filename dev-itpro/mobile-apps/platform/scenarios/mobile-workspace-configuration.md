---
# required metadata

title: Configure a workspace by using the SysAppWorkspace class
description: This topic explains how you can use the SysAppWorkspace class to configure and publish workspaces on the server. 
author: makhabaz
manager: AnnBe
ms.date: 07/01/2017
ms.topic: article
ms.prod: 
ms.service: Dynamics365Operations
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: robinr
ms.search.scope: Operations, Platform
# ms.tgt_pltfrm: 
ms.custom: 255544
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: makhabaz
ms.search.validFrom: 2017-07-20
ms.dyn365.ops.version: Platform update 3

---

# Configure a workspace by using the SysAppWorkspace class
You can use the **SysAppWorkspace** class to configure and publish workspaces on the server.

## Create a new workspace class
To use the **SysAppWorkspace** class for your workspace, you must create a new class for the workspace by extending the **SysAppWorkspace** class. You can then use the new class to modify workspace metadata. The new class also provides hooks for life cycle management of the mobile app.

Follow these steps to create a new workspace class for your workspace.

1. Create a new class for your workspace, and extend it from the **SysAppWorkspace** class.

    ![Workspace class](media/workspace-api/WorkspaceClass.png)

2. Add the **SysAppWorkspaceAttribute** attribute to the new class, and provide the **AppID** value of your workspace. You can find the app ID for your app on the **Summary** page in the mobile app designer.

    ![App ID in the workspace summary](media/workspace-api/workspaceSummary.png)

    ![AppID value in the workspace class](media/workspace-api/WorkspaceClassWithAppId.png)

3. Optional: If your workspace is an Application Object Tree (AOT) resource, provide the AOT resource name as the second parameter for the **SysAppWorkspaceAttribute** constructor.

    ![AOT resource name in the workspace class](media/workspace-api/WorkspaceClassWithAOTResource.png)

## Workspace life cycle methods
The workspace class provides the following methods that can be overridden.

### getWorkspaceMetadata
The **getWorkspaceMetadata** method is called to retrieve workspace metadata. You can override this method to update the workspace metadata. For more information, see the "Workspace metadata classes" section later in this topic. Here are some of the ways that you can use the metadata:

- Update the workspace, page, action, and control metadata.
- Provide custom configurations.
- Hide pages, actions, and controls in a workspace.
- Mark fields as mandatory.

### isWorkspaceHidden
The **isWorkspaceHidden** method can be overridden to hide or show the workspace on the mobile client. You can use custom parameters to determine whether the workspace must be hidden for a specific user. For more information, see [Secure a mobile app workspace](secure-mobile-workspace.md).

### onBeginJob
The **onBeginJob** method is called before a job request is run on the server. You can override this method to change the request parameters. You have access to the page or action name that the job request is for. By using the **onBeginJob** method, you can perform these tasks:

- Change the filter context for the job. For example, if a scenario requires that one record be sent, you can change the filter context to that record.
- You can change the job values. Job values are sent for a job that is initiated by actions on the mobile client. You can change or inspect values that are sent to start a job on the server.

### onEndJob
The **onEndJob** method is called after a job has been run on the server. You can override this method to change the result that is sent back to mobile client. You have access to the page or action name that the job result is for. By using the **onEndJob** method, you can perform these tasks:

- Change the job result. For example, you can change the values or add a new set of values.
- You can ignore the values that are returned from the server and send some other values back instead.

## Use the workspace class to publish workspaces from AOT resources
Workspaces can reside in the database. They can also reside in the AOT as resources. To provide visibility into workspaces that are stored in AOT resources, you must create a workspace class and point it to the name of the AOT resource that contains the workspace. Workspaces that are stored as AOT resources can't be edited or deleted by using the mobile app designer. Those workspace can only be exported.

Follow these steps to publish a workspace that resides in an AOT resource.

1. When you're developing a workspace that is stored in the database, you must export it from the mobile app designer so that it can be stored as an AOT resource. The workspace is exported as an XML file.

    ![Export a workspace](media/workspace-api/ExportWorkspace.png)

2. Delete the workspace from the mobile app designer. You will load it from an AOT resource later.

    ![Delete a workspace](media/workspace-api/DeleteWorkspace.png)

3. Create a new AOT resource, and select the exported workspace for the resource.

    ![Workspace as resource](media/workspace-api/WorkspaceAsResource.png)

4. Create a new class for your workspace. This class should extend **SysAppWorkspace**. Apply the **SysAppWorkspaceAttribute** attribute to the class, and provide the app ID and the AOT resource name that contains the resource.

    ![New workspace class](media/workspace-api/NewWorkspaceClass.png)

5. Build the class, and reopen the mobile app designer.

    The workspace is now published. It appears in the designer, but can't be edited or deleted. Note that the workspace is loaded from metadata.

    ![Workspace in metadata](media/workspace-api/WorkspaceInMetadata.png)

## Update a workspace that has already been published
If your workspace is part of an AOT resource, you can't edit it by using the mobile app designer. In the following example, a workspace that is named **MyWorkspace** exists in the AOT, and it has a backing class that is named **WorkspaceInAOT**.

![Workspace in the AOT](media/workspace-api/UpdateWorkspaceInAOT.png)

![Workspace in the AOT and published](media/workspace-api/UpdateWorkspaceInAOTAndPublished.png)

Follow these steps to edit the workspace.

1. Export the workspace by using the mobile app designer. The designer automatically creates new app IDs for workspaces that are stored in the AOT.
2. Import the newly exported workspace by using the mobile app designer.

    1. Optional: Change the name so that the newly added workspace can be distinguished from other workspaces.
    2. Copy the app ID of the newly created workspace.

    ![New workspace in database](media/workspace-api/UpdateWorkspaceNewWorkspace.png)

    ![New workspace details](media/workspace-api/UpdateWorkspaceNewWorkspaceDetails.png)

3. Create a new class that extends your backing class, apply the **SysAppWorkspaceAttribute** attribute, and specify the new app ID.

    ![Workspace in metadata](media/workspace-api/UpdateWorkspaceNewWorkspaceClass.png)

You can now continue to work with your new workspace and the backing class. After you've finished making your changes, you can merge them with the AOT-based workspace.

## Delete a workspace that is an AOT resource
When a mobile workspace is stored as an AOT resource, you can't delete it by using the mobile app designer. Follow these steps to delete a workspace that exists as an AOT resource.

1. Delete the AOT resource that contains the workspace.

    ![Workspace to delete](media/workspace-api/WorkspaceAsResourceToBeDeleted.png)

2. Delete the workspace class that was created for the workspace.

    ![Workspace class to delete](media/workspace-api/WorkspaceClassToBeDeleted.png)

3. Do a full model build that contains the AOT resource and the class. The following illustrations shows a full build of the Application Foundation model. The Application Foundation model also contains the AOT resource and workspace class. To speed up the full build, you can clear all the selections on the **Options** tab.

    ![Full build menu item](media/workspace-api/FullBuildMenuItem.png)

    ![Full build](media/workspace-api/FullBuild.png)

4. When the build is completed, reopen the mobile app designer, and verify that the workspace is no longer there.

## Workspace metadata classes
You can use the workspace metadata classes to inspect and update metadata that is related to the mobile workspace.

### SysAppWorkspaceMetadata
The **SysAppWorkspaceMetadata** class represents the whole mobile workspace metadata. You can use this class to perform the following tasks:

- Update the workspace title.
- Update the workspace description.
- Get the pages and actions that belong to the workspace.
- Add custom config objects that will be passed to the mobile workspace.

### SysAppPageMetadata
The **SysAppPageMetadata** class represents page metadata in a mobile workspace. You can use this class to perform the following tasks:

- Update the page title.
- Update the page description.
- Order a page. You can specify the order of the page relative to other pages that are shown in the workspace.
- Hide the page from the workspace.
- Get control metadata for controls on the page.

### SysAppActionMetadata
The **SysAppActionMetadata** class represents action metadata for a mobile workspace. You can use this class to perform the following tasks:

- Update the action title.
- Update the action description.
- Order an action. You can specify the order of the action relative to other actions that are shown on the workspace and on the pages.
- Hide the action.
- Get control metadata for controls in the action.

### SysAppControlMetadata
The **SysAppControlMetadata** class represents control metadata for controls on a page or action. You can use this class to perform the following tasks:

- Update the control label.
- Order a control. You can specify the order of the control relative to other controls on a page or action.
- Hide a control.
- Mark a control as mandatory.
