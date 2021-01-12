# Image Icon

## Background
Today, WinUI has a way to add icons in different image formats from the controls that inherit from IconElement. This includes BitmapIcon, FontIcon, SymbolIcon, PathIcon, and IconSourceElement. If a developer wanted to add an icon that is of type PNG, JPEG, or SVG, WinUI does not have a straight forward way of adding it. The developer could use BitmapIcon but the issue is that BitmapIcon and BitmapIconSource only accept URIs and the regular BitmapImage you would use with an image control only accepts a stream. This will prevent apps from being able to use icons from streams in their menu flyous and other controls that only accept images of ype IconElement.

To solve this issue, we will create a control that will be a superset of BitmapIcon and potentially PathIcon and will accept an ImageSource icon. The ShowAsMonochrome property in BitmapIcon will not be part of ImageIcon as it is not a requirement from design to have all icons be monochrome. This will give the developer the flexibility to add an icon of any image type that ImageSource accepts.  The image types are: 
- PNG
- JPEG
- BMP
- GIF
- TIFF
- SVG

This will also be a good steppingstone for cleaning up our Icon controls and consolidating them to a smaller set. With this new control, we can deprecate BitmapIcon and we will only have the following icon controls in WinUI: 
- FontIcon
- SymbolIcon
- ImageIcon
- PathIcon


## Description
ImageIcon is a UI component that accepts icons of any image type that ImageElement supports today. An ImageIcon will inherit from IconElement so it can be used anywhere an icon control that inherits from IconElement can be used today. ICO image types are not supported by this control.

**Important APIs:** [ImageIcon class](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ImageIcon)

## Is this the right control? 
Use an ImageIcon control when you want to display an image as your icon. Do not use an ImageIcon if you want to display an icon from a font. 

## Examples
Below is an example of how you can use ImageIcon in an App Bar Button Item. This example will also be added to the App Bar Button Item page with the rest of the icon controls. 

### Add a PNG icon as an app bar button Icon
XAML
```xml
<AppBarButton Label="ImageIcon" Click="AppBarButton_Click">
    <AppBarButton.Icon>
        <ImageIcon Source="Wheel.png"/>
    </AppBarButton.Icon>
</AppBarButton>
```

# API Notes
### Notable Properties  
| Name	| Type | Description | Default |
|:--- | :--- | :--- | :--- |
|Source | ImageSource | Gets and sets the icon source file that will be used in the application. | Null |


# API Details
```c++
namespace Microsoft.UI.Xaml.Controls
{
    unsealed runtimeclass ImageIcon : Windows.UI.Xaml.Controls.IconElement
    {
        ImageIcon();

        Windows.UI.Xaml.Media.ImageSource Source{ get; set; };

        static Windows.UI.Xaml.DependencyProperty SourceProperty{ get; };
    }
}
```

# Appendix
## Scope
|Capability	| Priority |
| Icon can be used anywhere a BitmapIcon is accepted today | Must|
| PNG, JPEG, and SVG images are types the control will accept | Must|
| Icon must respond to user theming changes (Light, Dark, and High Contrast) | Must |
| ICO icons will be accepted by the control | Could |
| SVG image support will be improved | Not Priority |


## Accessibility
This control will follow the same accessibility requirements as the other icon controls under IconElement. That includes the following: 
- When the user switches their theme to light or dark mode, the icon will adapt to the new theme. 
- When the user switches to high contrast mode, the icon will support the correct high contrast ratio. 

## Open Questions
- Should ICOs be supported in the future? 
- Should he ShowAsMonochrome property be added in the future? This is somehing that was added because of a design requirement back when BitmapIcon was first designed but it is no longer a hard design requirement. 

