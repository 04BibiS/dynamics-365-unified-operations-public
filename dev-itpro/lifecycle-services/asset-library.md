
# Asset Library
This wiki describes the Asset Library functionality in Lifecycle Services.

Asset Library is a store for all the different assets associated with a tenant in LCS. There are two kinds of Asset Libraries available in LCS:
1) *Shared Asset Library* - This is used by Microsoft and Partners to share assets across multiple tenants, projects and environments in LCS. This is accessible to every user that signs into LCS. To get to the Shared Asset Library:
    a) Log into LCS.
    b) **Click** on **Shared Asset Library** tile. 
2) *Project level Asset Library* - This is a project level asset library to share assets across environments within a project in LCS. This is accessible to users within a project. To get to the Project Asset Library:
    a) Log into LCS.
    b) Click on a **specific project**. 
    c) In the hamburger menu at the top left, click on **Asset Library**. 

## Asset library supports multiple kinds of assets. Some common used assets are:
1) **Software deployable packages**  - This tab represents all the packages used for updating your environment with latest set of updates.
2) **Data Packages** - This tab stores assets used for configuration and data management. 
3) **GER Configuration** - Stores all localization and translation assets that are then applied to the Client. 
4) **Retail SDK** - Stores all the latest scripts for Retail SDK. 

## Each asset has multiple scopes:
1) **Me** - When an asset is uploaded, it is set to Me scope. This means that it is visible to only the person that uploaded the asset. 
2) **Project** - when a project is imported from Global scope to a project, the scope is set to Project scope. 
3) **Organization** - When an asset needs to be shared with multiple users within a tenant, the tenant admin can promote the asset to organization scope. 
4) **Global** - Only Microsoft can upload assets to the Global scope. These are assets that Microsoft wants to be made publicly available to all the LCS projects and users.  

## Each asset has a state:
1) **Draft** - This indicates the asset will be edited at some point and hence it is in the Draft state. 
2) **Published** - This indicates the asset is Published at an Org or Global level and edits are done. 

## Different actions that can be performed in the asset library:
1) **Uploading an asset to the asset library**: To upload an asset to the asset library follow the below steps:
    a) Select the tab where you want to upload the asset
    b) Click on **'+'** button. 
    c) Enter name and description for the asset. 
    d) Upload the file for the asset and click **Confirm**. 
    
    For uploading a new version for a specific asset:
    a) Select the asset in the asset library.
    b) Click on the 'Upload' icon in the asset library toolbar. 
    c) Follow the same steps as uploading a new asset. 
     This will upload a new asset in the asset library. To view multiple versions for a single asset, click on **Versions** in the toolbar. 

2) **Move assets from global asset library to project asset library**: To move asset from the global to the project scope, first go the Project Asset Library. Below are two options to move assets to a project scope:
  a) *Import from Global scope* - Used to import assets from global scope into project scope so it can be applied across environments. 
        1) In the project asset library, depending on the asset type that you want to import, select that specific tab.
        2) Click on Import. This will open list of assets from the Shared asset library. 
        3) Select the asset that you want to import and click **Pick**.
      This will import the selected asset and put it in the Published state in the project asset library. This is for packages that you do not plan to edit. If you want to edit an imported package, create a copy using the steps below which will put the package in Draft state. 
    
  b) *Copy from Global Scope* - Used to create a copy of the assets so they can be edited. 
        1) In the project asset library, depending on the asset type that you want to import, select that specific tab.
        2) Select the asset you want to copy.
        3) Click on **Copy** in the asset library toolbar. 
        This will create a copy of the Published asset and put it in Draft state. 
        
  c) *Save to My Library* - Used to move the edited asset back to the shared asset library so it can be promoted to the org scope to be shared with multiple customers. 
        1) In the project asset library, depending on the asset type that you want to import, select that specific tab.
        2) Select the asset you want to save.
        3) Click on **Save to My Library**.
      This will save the asset from the project asset library back to the shared asset library in the Me Scope. 
  




