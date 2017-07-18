# Marking a field as mandatory using workspace classes
Support have been added for inferring properties like mandatory, field length and type when you use Operations mobile app designer to select fields for Actions.

You can later use the workspace classes to update these properties.
Using workspace classes you can mark/un-mark a field as mandatory.

Consider an action for creating customer. The designer have automatically inferred that "Name" is a mandatory field.

| ![alt text](media/workspace-api/MarkFieldAsMandatoryDesigner.png "Action showing fields")  | ![alt text](media/workspace-api/MarkFieldAsMandatoryAction.png "Action with mandatory field marked")|
|--|--|

Now consider you would the field "Delivery terms" to be mandatory?
You can use the workspace class to mark "Delivery terms" field as mandatory using the following steps.

1. Get the control name using app designer. For our demo the control name is "DynamicDetail_DlvTerm"
2. Add the following code to set the mandatory property for the control. Note that we are using the reflection based setProperty method to set mandatory property.

![alt text](media/workspace-api/MarkFieldAsMandatoryCode.png "Changes needed in workspace class")

3. Build the solution and then refresh app metadata on the mobile app.

As you can see below, the Delivery terms field is now marked as Mandatory.

![alt text](media/workspace-api/MarkFieldAsMandatoryFinal.png "Delivery terms field marked as mandatory")
