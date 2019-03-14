<!-- The purpose of this spec is to describe a new feature and
its APIs that make up a new feature in WinUI. -->

<!-- There are two audiences for the spec. The first are people
that want to evaluate and give feedback on the API, as part of
the submission process.  When it's complete
it will be incorporated into the public documentation at
docs.microsoft.com (http://docs.microsoft.com/uwp/toolkits/winui/).
Hopefully we'll be able to copy it mostly verbatim.
So the second audience is everyone that reads there to learn how
and why to use this API. -->

# Background
<!-- Use this section to provide background context for the new API(s) 
in this spec. -->

<!-- This section and the appendix are the only sections that likely
do not get copied to docs.microsoft.com; they're just an aid to reading this spec. -->

<!-- If you're modifying an existing API, included a link here to the
existing page(s) -->

<!-- For example, this section is a place to explain why you're adding this API rather than
modifying an existing API. -->

<!-- For example, this is a place to provide a brief explanation of some dependent
area, just explanation enough to understand this new API, rather than telling
the reader "go read 100 pages of background information posted at ...". -->

The Tab control is a way to display a set of tabs and their respective content. Tab controls are useful for displaying several pages (or documents) of content while giving a user the capability to rearrange, open, or close new tabs. 

![Tabs in Edge](https://user-images.githubusercontent.com/25991996/52679758-f6d6ad80-2eea-11e9-9955-fd111c26d982.png)

Tab-like UI comes in two distinct styles that affect not only the visualization of the control, but also the user experience:
* Static tabs
    * Generally display a set number of pages
    * User can change between pages, but cannot open new pages, close pages, or rearrange pages
* Document tabs
    * Display a variable number of pages
    * User can add, remove, and rearrange pages
    * User can "tear off" tabs into their own window or "recombine" tabs from one group into another group

The Xaml platform already provides several controls to represent "static-style tabs" including Pivot and top NavigationView. However, these controls do not well support "document-style" tabs due to a variety of limitations, including:
* Significant effort up to (and including) retemplating to build simple tabs
* No built-in support for closing tabs
* No built-in support for drag/drop into new windows
* No built-in support for moving a tab from a window and combining it with another window
* Limited keyboard and accessibility support

This spec therefore covers "document-style tabs" specifically as this platform gap isn't easily solved with an app-side workaround. 

# Description
<!-- Use this section to provide a brief description of the feature.
For an example, see the introduction to the PasswordBox control 
(http://docs.microsoft.com/windows/uwp/design/controls-and-patterns/password-box). -->

The Tab control is a collection of tabs that each represents a new page or document in your app. Tab controls are useful when your app has several pages of content and the user expects to be able to add, close, and rearrange the pages.

**Is this the right control?**

Use a TabControl to help the user manage multiple app pages or documents within the same window. 

Do not use a TabControl to display a static set of tabs that the user cannot rearrange, open, or close. Use a Pivot or top NavigationView instead. 

# Examples
<!-- Use this section to explain the features of the API, showing
example code with each description. The general format is: 
  feature explanation,
  example code
  feature explanation,
  example code
  etc.-->

<!-- As an example of this section, see the Examples section for the PasswordBox control 
(https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/password-box#examples). -->

To replicate the behavior of Microsoft Edge:

``` xml
<TabControl TabWidthBehavior="Equal"
            CanCloseTabs="True"
            CloseButtonOverlay="OnHover"
            CanDragItems="True"
            CanReorderItems="True"
            TabDraggedOutside="OpenTabInNewWindow">
    <TabControl.TabFooter>
        <Button Content="+" Click="NewTab_Click" />
    </TabControl.TabFooter>
    ...
</TabControl>
```

The TabControl also supports databinding: 

``` xml
<TabControl ItemsSource="{x:Bind TabItemCollection}" />
```

**TODO**: Example of putting tabs in the titlebar area

[Title bar customization](https://docs.microsoft.com/en-us/windows/uwp/design/shell/title-bar)

**TODO**: Tab tear out example
``` xml
<TabControl TabDraggedOutside="OpenTabInNewWindow" />
```
``` csharp
public void OpenTabInNewWindow(Args e)
{
  // TODO: Open tab in new CoreWindow or new AppWindow, depending on version... 
}
```

# Remarks
<!-- Explanation and guidance that doesn't fit into the Examples
section.  For example, see the Remarks for the MediaPlayerElement 
(https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement#remarks). -->


# API Notes
<!-- Give a one or two line description of each API (type
and member), or at least the ones that aren't obvious
from their name.  These descriptions are what show up
in IntelliSense. -->

### TabControl properties and events

| Property | Type | Description |
|:-------- |:---- |:----------- |
| CanCloseTabs | bool | Default value for the item if it doesn't specify a IsClosable value. |
| CloseButtonOverlay | enum | Describes the behavior of the close button. Values are {Auto, OnHover, Persistent} |
| ItemHeaderTemplate | DataTemplate | Default template for the item if no template specified. |
| SelectedTabWidth | double | The size of the selected tab header. |
| TabHeader | object | Content to the left of the tab strip. |
| TabHeaderTemplate | DataTemplate | Template for the Header. |
| TabFooter | object | Content to the far right of the tab strip. |
| TabFooterTemplate | DataTemplate | Template for the Footer. |
| TabActionContent | object | Content immediately to the right of the tabs |
| TabActionContentTemplate | DataTemplate | Template for the ActionContent. |
| TabWidthBehavior | enum | Specifies how the tabs should be sized. Values are {Actual, Equal, Compact} |

| Event | Description |
|---|---|
| TabClosing | Fires when a tab is about to be closed. Can be cancelled to prevent closure. |
| TabDraggedOutside | Fires when a Tab is dragged outside of the Tab bar. |

### TabItem properties and events

| Property | Type | Description |
|:-------- |:---- |:----------- |
| Content | object | The main content that appears in the tab. |
| Header | object | The content that appears inside the tab itself.  |
| HeaderTemplate | DataTemplate | Template for the header object. |
| Icon | IconElement | Icon for the tab. |
| IsClosable | bool | Determines if the tab shows a close button. |

| Event | Description |
|---|---|
| TabClosing | Fires when a tab's close button is clicked. |

# API Details
<!-- The exact API, in MIDL3 format (https://docs.microsoft.com/en-us/uwp/midl-3/) -->

# Appendix
<!-- Anything else that you want to write down for posterity, but 
that isn't necessary to understand the purpose and usage of the API.
For example, implementation details. -->

![Parts of a tab item](https://user-images.githubusercontent.com/25991996/53277900-35beed00-36bb-11e9-910e-36119f7ad4af.png)

![Position of TabHeader TabFooter and TabActionContent](https://user-images.githubusercontent.com/25991996/53277918-4c654400-36bb-11e9-9d0d-4fac948bc9f0.png)

# Open Questions
1. Should Tab Tearoff be something that the control handles or that the app handles?
1. Should there be a built-in "Add new tab" button? 
1. How can an app customize that the Selected tab looks like (ie. in Edge)?
1. The API currently takes a lot of inspiration from the Toolkit. Are there any chances to iterate/improve?
1. What visual affordance should we use when not all tabs fit? Bumpers or dropdown+flyout?