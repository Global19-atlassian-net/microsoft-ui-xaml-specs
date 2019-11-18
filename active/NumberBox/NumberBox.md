# Number box

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

## Background
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

Xaml has a TextBox control for text input, which can be used for numeric input, but numeric input scenarios benefit from custom features, such as buttons to increment/decrement. The NumberBox control in this spec provides these features. This will ship as part of the [WinUI package](https://www.nuget.org/packages/Microsoft.UI.Xaml), not as part of the Windows OS.


## Description
<!-- Use this section to provide a brief description of the feature.
For an example, see the introduction to the PasswordBox control 
(http://docs.microsoft.com/windows/uwp/design/controls-and-patterns/password-box). -->

<!-- Borrowing text from TextBox -->
Represents a control that can be used to display and edit numbers. This supports validation, increment stepping, and computing inline calculations of basic equations, such as multiplication, division, addition, and subtraction.

## Is this the right control? 

You can use a NumberBox control to capture and display mathematic input. If you need an editable text box that accepts more than numbers, use the [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) control. If you need an editable text box that accepts passwords or other sensitive input, see [PasswordBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox). If you need a text box to enter search terms, see [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox). If you need to enter or edit formatted text, see [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox).

## Examples
<!-- Use this section to explain the features of the API, showing
example code with each description. The general format is: 
  feature explanation,
  example code
  feature explanation,
  example code
  etc.-->
  
<!-- Code samples should be in C# and/or C++/WinRT -->

<!-- As an example of this section, see the Examples section for the PasswordBox control 
(https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/password-box#examples). -->

<!-- Todo: Needs an input validation example, and probably a section in the Remarks about input validation. -->

### Create a simple NumberBox

XAML

<!-- todo: need a picture that matches the markup (the placeholder suggests a calculation, but this sample doesn't enable calculations-->

<!-- todo: Can we remove these names? They don't serve any purpose here, and just add noise. And we should x:Bind the value, so that the example shows how you get the value out. -->

```XAML
<NumberBox x:Name="EquationInputNumberBox" Header="Enter equation:" />
```
![](images/numberbox-placeholder-text.png)

### Enable calculation support

To let the user enter equations, use the `AcceptsCalculation` property. This default to false.

<!-- todo: Need to briefly tell the story of calculations here, I'm not sure how it works. When the user hits enter, does it do the calculation? Does the user have to enter the '='? Does it keep showing the equation after it's done the calculation or does setting this property just allow me to type non-numerics into the box?

A basic description can be here, if it gets too involved put the rest in the Remarks section. 
-->

XAML
```XAML
<NumberBox x:Name="EquationInputNumberBox"
    Header="Enter equation:" 
    PlaceholderText="A + B * C" 
    AcceptsCalculation="True" />
```

![](images/numberbox-acceptscalculation.png)

### Add increment and decrement stepping

Use the `SpinButtonPlacementMode` property to enable buttons in the NumberBox control that can be clicked to increment or decrement the value in the NumberBox. This defaults to `Hidden`, set to `Inline` to enable the buttons. The amount of increment/decrement is specified with the `StepFrequency` property, which defaults to 1.

<!-- todo: What happens if I set this as well as AcceptsCalculation? Is this ignored? -->

XAML
```XAML
<NumberBox x:Name="ItemQuantityNumberBox" 
    Header="Quantity"
    StepFrequency="2"
    SpinButtonPlacementMode="Inline" />
```

![](images/numberbox-spinnerbutton.png)

## Remarks
<!-- Explanation and guidance that doesn't fit into the Examples section. -->

<!-- APIs should only throw exceptions in exceptional conditions; basically,
only when there's a bug in the caller, such as argument exception.  But if for some
reason it's necessary for a caller to catch an exception from an API, call that
out with an explanation either here or in the Examples -->

<!-- todo: Calculations probably require a more involved description, this is the place to put it. -->

## API Notes
<!-- Option 1: Give a one or two line description of each API (type
and member), or at least the ones that aren't obvious
from their name.  These descriptions are what show up
in IntelliSense. For properties, specify the default value of the property if it
isn't the type's default (for example an int-typed property that doesn't default to zero.) -->

<!-- Option 2: Put these descriptions in the below API Details section,
with a "///" comment above the member or type. -->


<!-- todo: The behavior WRT InputScope should be described here. Here, rather than the table below, because InputScope isn't a NumberBox property, but it's NumberBox behavior on an inner TextBox. -->

### Notable Properties  

| Name | Description |
|:-:|:--|
| StepFrequency | Gets or sets the value part of a value range that steps should be created for. |

## API Details
<!-- The exact API, in MIDL3 format (https://docs.microsoft.com/en-us/uwp/midl-3/) -->

<!-- todo: There's a bunch of API in here that's not described in the Examples, Remarks, or API Notes. That's OK for obvious APIs like Header (although it's good that that's demonstrated in an Example already.) But for example there's been no mention of HyperScroll. -->

<!-- todo: We're missing some basic text capabilities: SelectionFlyout, SelectionHighlightColor, TextReadingOrder, PreventKeyboardDisplayOnProgrammaticFocus, Description. These are basic feature that I think any text control should have. -->

<!-- todo: Missing some APIs in here, like TemplatSettings class and event args classes -->

```c++ 
enum NumberBoxSpinButtonPlacementMode
{
    Hidden,
    Inline,
};

enum NumberBoxValidationMode
{
    InvalidInputOverwritten,
    IconMessage,
    TextBlockMessage,
    Disabled,
};

runtimeclass NumberBoxValueChangingEventArgs
{
    Boolean IsContentChanging;
};

runtimeclass NumberBoxValueChangedEventArgs
{
    System.Windows.Controls.UndoAction Action;
    System.Collections.Generic.ICollection<System.Windows.Controls.ValueChange> Changes;
};

unsealed runtimeclass NumberBox : Windows.UI.Xaml.Controls.Control
{
    NumberBox();

    Double Minimum;
    Double Maximum;
    Double Value;
    Double StepFrequency;

    String Header;
    DataTemplate HeaderTemplate;
    String Text; 
    String PlaceholderText;
  
    NumberBoxValidationMode ValidationMode;

    Boolean AcceptsCalculation;

    NumberBoxSpinButtonPlacementMode SpinButtonPlacementMode{ get; set; };

    Boolean IsHyperScrollEnabled;
    Boolean IsHyperDragEnabled;
    Boolean IsWrapEnabled;

<!-- NumberFormatter is worth an example -->
    Windows.Globalization.NumberFormatting.INumberFormatter2 NumberFormatter;

<!-- todo: Do we need ValueChanging? Assume we do, the Remarks or API Notes section should explain it's behavior. Does this respond to TextBox's TextChanging or BeforeTextChanging events? -->

    event Windows.Foundation.TypedEventHandler<NumberBox, NumberBoxValueChangedEventArgs> ValueChanged;
    event Windows.Foundation.TypedEventHandler<NumberBox, NumberBoxValueChangingEventArgs> ValueChanging;
    
    static Windows.UI.Xaml.DependencyProperty MinimumProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty MaximumProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty ValueProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty StepFrequencyProperty{ get; };
    
    static Windows.UI.Xaml.DependencyProperty Header{ get; };
    static Windows.UI.Xaml.DependencyProperty HeaderTemplate{ get; };
    static Windows.UI.Xaml.DependencyProperty Text{ get; };
    static Windows.UI.Xaml.DependencyProperty PlaceholderText{ get; };
    
    static Windows.UI.Xaml.DependencyProperty ValidationModeProperty{ get; };
    
    static Windows.UI.Xaml.DependencyProperty AcceptsCalculationProperty{ get; };
    
    static Windows.UI.Xaml.DependencyProperty SpinButtonPlacementModeProperty{ get; };
    
    static Windows.UI.Xaml.DependencyProperty IsHyperDragEnabledProperty{ get; };
    static Windows.UI.Xaml.DependencyProperty IsHyperScrollEnabledProperty{ get; };   
    static Windows.UI.Xaml.DependencyProperty IsWrapEnabled{ get; };
    
    static Windows.UI.Xaml.DependencyProperty NumberFormatter{ get; };
}
```

## Appendix
<!-- Anything else that you want to write down for posterity, but 
that isn't necessary to understand the purpose and usage of the API.
For example, implementation details. -->

### Behavioral Components

<!-- A bunch of this stuff belongs in DMC, and therefore belongs above the Appendix -->

| Property | Notes |
|:---:|:---|
| InputScope | "Number" will be used for the InputScope. This may be overwritten by the developer but alternative InputScope types will not be explicitly supported. | 
| AcceptsCalculation | NumberBox will provide computation support for multiplication, division, addition, and subtraction across parenthetical order with standard operator precedence; i.e., [ 0-9()+-/* ] |
| Validation | * If BasicValidationMode="Disabled", no automatic validation will occur. This setting allows developers to configure custom validation via [Input Validation]( https://github.com/microsoft/microsoft-ui-xaml-specs/blob/user/lucashaines/inputvalidation/active/InputValidation/InputValidation.md). <br><br> * If BasicValidationMode="TextBlockMessage" or BasicValidationMode="IconMessage", input that is outside the bounds of Minimum/Maximum or non-numerical/formulaic will trigger a validation warning consistent with [Input Validation]( https://github.com/microsoft/microsoft-ui-xaml-specs/blob/user/lucashaines/inputvalidation/active/InputValidation/InputValidation.md). A Minimum error will display "Minimum is [Minimum]." A Maximum error will display "Maximum is [Maximum]." Input errors will display "Only use numbers and ()+-/* ." Division by zero will return "Division by 0 unsupported." <br><br> * If BasicValidationMode="InvalidInputOverwritten", input that is non-numerical/formulaic will automatically be overwritten with the last legal value. Input that is stepped outside the bounds of Minimum/Maximum will be coerced to the respective bound. Manually entered input that is outside outside the bounds of Minimum/Maximum will be reverted to the last valid input. |
| Events | * Loss of focus, "Enter", and stepping [SEE API NOTES > SPINBUTTON && HYPER SCROLL && HYPER DRAG && KEYBOARD STEPPING] will trigger evalution. <br><br> * When Text (derived from TextBox) is changed by codebehind or user input on the evaluation triggers noted above, the TextChanging event will be fired. After, if BasicValidationEnabled="True", validation will be performed [See API NOTES > VALIDATION]. Text will then be updated and the TextChanged event will be fired. Text will then be converted to a Double and the ValueChanging event event will be fired. After, Value will be updated and the ValueChanged event will be fired. <br><br> * When Value is changed by codebehind, the ValueChanging event will be fired. After, if BasicValidationEnabled="True", validation will be performed [See API NOTES > VALIDATION]. Value will then be updated and the ValueChanged event will be fired. Value will then be converted to a String and the TextChanging event event will be fired.  After, Text will be updated and the TextChanged event will be fired. <br><br> * Requesting Value will forcibly trigger calculation. If Value is in an error state (NumberBoxBasicValidationMode != InvalidInputOverwritten), return Double.NaN (as Null can have meaning). |
| SpinButton | * If a calculation is stepped, it will be calculated before the "step" is applied. <br><br> SpinButton will disable increment/decrement buttons when the limits imposed by Maximum or Minimum would not allow another respective step. <br><br> * If IsWrapEnabled = "true", a step will not stop at the Minium or Maxmum, it will wrap instead. For example, if Minimum="0", Maximum="100", StepFrequency="5", and Value="98", and IsWrapEnabled="True", an incremental step results in Value="3".  |
| Hyper Scroll | * Focus and hover required for hyper scroll behavior to take place as to not reduce quality of experience on scrollable surfaces. <br><br> * If a calculation is stepped, it will be calculated before the step is applied.|
| Hyper Drag | * Align to WinRT XAML Toolkit's NumericUpDown implementation. <br><br> * If a calculation is stepped, it will be calculated before the step is applied. |
| Keyboard Stepping | * Up and Down arrow keys will increment and decrement the Text/Value when NumberBox is in focus. <br><br> * If a calculation is stepped, it will be calculated before the step is applied. |

### Inputs and Accessibility

#### UI Automation Patterns 

#### Keyboard Navigation 
| State | Action |
|:---|:---|
| Focus is on item in tab stop order before NumberBox | Tab moves focus into NumberBox's editable text field. |
| Focus is on NumberBox's editable text field. | Tab triggers evaluation and moves focus to validation error message if one is returned. Otherwise, tab moves focus to SpinButton (Decrement). |
| Focus is on NumberBox's validation error message. | Tab moves focus to NumberBox's SpinButton (Decrement). |
| Focus is on NumberBox's SpinButton (Decrement). | Tab moves focus to SpinButton (Increment). |
| Focus is on NumberBox's SpinButton (Increment). | Tab moves focus out of NumberBox and to the next item in the tab stop order. |

#### Narrator

| State | Action |
|:---|:---|
| Focus is moved to NumberBox's editable text field. | AutomationProperty.Name, Header, and Text properties are read. |
| Evaluation is triggered. | Evaluation is announced. |
| Validation Error Message is returned. | Error message is announced. |
| Focus is moved to NumberBox's validation error message. | Error message is announced. |
| Focus is moved to NumberBox's SpinButton (Decrement). | Property name is announced. |
| Focus is moved to NumberBox's SpinButton (Increment). | Property name is announced.  |


#### Gamepad  

| State | Action |
|:---|:---|
| Focus is near NumberBox | Spatial navigation may be used to access NumberBox where NumberBox's editable text field first receives focus. |
| Focus is on NumberBox's editable text field. | Spatial navigation: <br> Spatial navigation will navigate between the SpinButton, the editable textfield, or away from NumberBox. <br><br> A: <br> Will enter input mode. Calculation will be triggered on exit of input mode. <br><br> B (while in input mode): <br> Will trigger evaluation and exit input mode. |
| Focus is on NumberBox's SpinButton (Decrement). | Spatial navigation: <br> Spatial navigation will navigate between the SpinButton, the editable textfield, or away from NumberBox. <br><br> A: <br> Invoke decrement SpinButton action. |
| Focus is on NumberBox's SpinButton (Increment). | Spatial navigation: <br> Spatial navigation will navigate between the SpinButton, the editable textfield, or away from NumberBox. <br><br> A: <br> Invoke increment SpinButton action. |

### Data and Intelligence Metrics
##### P0: Feature Key Performance Indicators

Feature has received enough app/developer validation and feedback for it to exit preview.

* KPI: Three or more first party applications are validating the feature in prerelease.
    * Measurement: Occurrence of NumberBox type in apps on developer devices via telemetry (running under debugger). **Note for telemetry sampling considerations:** Need to be cautious for WinUI as it does not work the same way as with OS SDK. If the solution is not carefully considered, it will result in too little dev data (at 2% sampling) or too much end-user data (at 100% sampling) and hence throttling (telemetry data loss). Requires further investigation. 
    * Measurement: Count of users providing feedback on GitHub/UserVoice/Feedback Hub/SIUF.
    
Developers feel delighted and satisfied with the feature. 

* KPI: Dev NSAT is positive (min), >20 (aspirational) as we exit 2019.
    * Measurement: Developer survey via SIUF, e.g.: "How useful is the feature?"
    
##### P1: Feature Performance Indicators

Calculation support is leveraged by developers + appropriately surfaced to end users. 

* KPI: ~4% of unique instances of NumberBox surface an equation evaluation. 
    * Measurement: Ratio of occurances of NumberBox triggering evaluations compared to total count of NumberBox instances. 
    
##### P2+: Trailing Indicators

N/A

## TODO

* Finalize designs and replace mock ups (with particular regard to SpinButtons). 

* Valiate localization options. 

* Align hyperdrag/hyperscroll behavior with VS Blend implementation.

* DOCS: Discern V2 validation API additions. 

* DOCS: Add API use examples. 

## Open Questions

* Is there value in creating a preview for calculation results? @mdtauk created a few example visualizations: 

![NumberBox with a tool tip above to show a preview of the calculation results](https://user-images.githubusercontent.com/16964652/58919441-fbfe7900-86e2-11e9-8d2b-dd4dadfa74c5.png)

![NumberBox with a calculation in progress and highlight text previewing the calculation results](https://user-images.githubusercontent.com/7389110/58920708-5b807700-872b-11e9-9924-21a7b7d37e68.png)

* Should/can touch/virtual keyboards intelligently adapt to numeral-formulaic only input?