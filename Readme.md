<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128585198/22.2.2%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T395040)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)
<!-- default badges end -->

# WPF Diagram - Create a DiagramShape Descendant with Editable and Serializable Properties

This example demonstrates how to serialize and allow users to edit custom properties of [DiagramShape](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramShape) descendants. In the example, the **DiagramShapeEx** class contains the serializable **Description** property. Users can edit this property value in the [Properties Panel](https://docs.devexpress.com/WPF/116506/controls-and-libraries/diagram-control/diagram-designer-control/properties-panel).

![image](https://github.com/DevExpress-Examples/wpf-diagram-create-diagramshape-descendant-with-editable-and-serializable-properties/assets/65009440/f7e34156-60ae-4ac9-abe3-0c6772543d76)

## Implementation Details

1. Mark your custom properties with the `XtraSerializableProperty` attribute to enable their serialization:

    ```cs
    public class DiagramShapeEx : DiagramShape {
        [XtraSerializableProperty]
        public string Description { get; set; }
        static DiagramShapeEx() {
            DiagramControl.ItemTypeRegistrator.Register(typeof(DiagramShapeEx));
        }
    }
    ```

    Make sure that your descendant class has a **parameterless constructor**.

2. Call the [DiagramItemTypeRegistrator.Register](https://docs.devexpress.com/CoreLibraries/DevExpress.Diagram.Core.DiagramItemTypeRegistrator.Register(System.Type--)) method to register your descendant type and allow its serialization. You should register descendants at the application startup. If the custom item is used in the [Diagram Designer](https://docs.devexpress.com/WPF/115125/controls-and-libraries/diagram-control/diagram-designer-control/diagram-designer-control) or [Item Template Designer](https://docs.devexpress.com/WPF/117615/controls-and-libraries/diagram-control/data-binding/item-template-designer), it is necessary to also register it in the item static constructor:

    ```cs
    DiagramControl.ItemTypeRegistrator.Register(typeof(DiagramShapeEx));
    ```

3. Create and register a [FactoryItemTool](https://docs.devexpress.com/CoreLibraries/DevExpress.Diagram.Core.FactoryItemTool) that creates your custom shape when a user adds it to the canvas:

    ```cs
    var stencil = new DiagramStencil("CustomStencil", "Custom Shapes");
    var itemTool = new FactoryItemTool(
        "CustomShape",
        () => "Custom Shape", diagram => {
            DiagramShapeEx customShape = new DiagramShapeEx() { Width = 100, Height = 50 };
            return customShape;
        },
        new Size(100, 50),
        false
    );
    stencil.RegisterTool(itemTool);
    DiagramToolboxRegistrator.RegisterStencil(stencil);
    ```

4. To allow users to edit your custom property in the [Properties Panel](https://docs.devexpress.com/WPF/116506/controls-and-libraries/diagram-control/diagram-designer-control/properties-panel), handle the [DiagramControl.CustomGetEditableItemProperties](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramControl.CustomGetEditableItemProperties) event:

    ```cs
    private void diagramControl_CustomGetEditableItemProperties(object sender, DiagramCustomGetEditableItemPropertiesEventArgs e) {
        if (e.Item is DiagramShapeEx) {
            e.Properties.Add(TypeDescriptor.GetProperties(typeof(DiagramShapeEx))["Description"]);
        }
    }
    ```

## Files to Review

* [DiagramData.xml](./CS/DXDiagram.CustomShapeProperties/DiagramData.xml)
* [MainWindow.xaml](./CS/DXDiagram.CustomShapeProperties/MainWindow.xaml)
* [MainWindow.xaml.cs](./CS/DXDiagram.CustomShapeProperties/MainWindow.xaml.cs) (VB: [MainWindow.xaml.vb](./VB/DXDiagram.CustomShapeProperties/MainWindow.xaml.vb))
* [ShapeClasses.cs](./CS/DXDiagram.CustomShapeProperties/ShapeClasses.cs) (VB: [ShapeClasses.vb](./VB/DXDiagram.CustomShapeProperties/ShapeClasses.vb))

## Documentation

* [Shapes](https://docs.devexpress.com/WPF/116099/controls-and-libraries/diagram-control/diagram-items/shapes)
* [DiagramShape](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramShape)
* [Saving and Loading Diagrams](https://docs.devexpress.com/WPF/118180/controls-and-libraries/diagram-control/saving-and-loading-diagrams)
* [DiagramControl.CustomGetEditableItemProperties](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramControl.CustomGetEditableItemProperties)

## More Examples

* [WPF DiagramControl - Register FactoryItemTools for Regular and Custom Shapes](https://github.com/DevExpress-Examples/wpf-diagram-register-factoryitemtools-for-shapes)
<!-- feedback -->
## Does This Example Address Your Development Requirements/Objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-create-diagramshape-descendant-with-editable-and-serializable-properties&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-create-diagramshape-descendant-with-editable-and-serializable-properties&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
