<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128585198/16.2.3%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T395040)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)
<!-- default badges end -->
<!-- default file list -->
*Files to look at*:

* [DiagramData.xml](./CS/DXDiagram.CustomShapeProperties/DiagramData.xml) (VB: [DiagramData.xml](./VB/DXDiagram.CustomShapeProperties/DiagramData.xml))
* [MainWindow.xaml](./CS/DXDiagram.CustomShapeProperties/MainWindow.xaml) (VB: [MainWindow.xaml](./VB/DXDiagram.CustomShapeProperties/MainWindow.xaml))
* [MainWindow.xaml.cs](./CS/DXDiagram.CustomShapeProperties/MainWindow.xaml.cs) (VB: [MainWindow.xaml.vb](./VB/DXDiagram.CustomShapeProperties/MainWindow.xaml.vb))
* [ShapeClasses.cs](./CS/DXDiagram.CustomShapeProperties/ShapeClasses.cs) (VB: [ShapeClasses.vb](./VB/DXDiagram.CustomShapeProperties/ShapeClasses.vb))
<!-- default file list end -->
# How to: Create a DiagramShape Descendant with Editable and Serializable Properties


This example demonstrates how to serialize custom data using DiagramControl's serialization mechanism. In the example, the Content property of diagram shapes is loaded from data objects every time the diagram is shown. To associate shapes with data objects, the DatabaseObjectID property is added at the DiagramShape descendant level. To serialize this property along with standard DiagramShape properties, perform the following steps:<br><br>1) Mark your custom property with the **XtraSerializableProperty** attribute:<br>


```cs
[XtraSerializableProperty]
public int DatabaseObjectID { get; set; }
```


<p><br>2) Call the <strong>ItemTypeRegistrator.Register</strong> method to register your custom shape type for serialization. Custom shape types should be registered at the application start. If the custom shape is used in the Diagram Designer, it is recommended to also register it in the shape type's static constructor.</p>


```cs
DiagramControl.ItemTypeRegistrator.Register(typeof(DiagramShapeEx));
```
<p><br> 3) Make sure that your custom item class has a <strong>parameterless constructor</strong>
<br>
<p><strong>Note:</strong><br><em>In certain scenarios, it is easier to use the DiagramShape.Tag property to store custom data without creating DiagramShape descendants. In this case, no further steps are needed as the Tag property is serialized by default.<br><br></em></p>
<p>To allow end-users to edit your custom property in the Properties Panel, handle the CustomGetEditableItemProperties event.</p>


```cs
private void diagramControl_CustomGetEditableItemProperties(object sender, DiagramCustomGetEditableItemPropertiesEventArgs e) {
    if (e.Item is DiagramShapeEx) {
        e.Properties.Add(TypeDescriptor.GetProperties(typeof(DiagramShapeEx))["Description"]);
    }
}
```



<br/>


<!-- feedback -->
## Does This Example Address Your Development Requirements/Objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-create-diagramshape-descendant-with-editable-and-serializable-properties&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-create-diagramshape-descendant-with-editable-and-serializable-properties&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
