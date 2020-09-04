# Address-Lookup - Google
Lookup address using the Google Places API

This component allows one to lookup addresses using the Google Maps APIs. Because it uses autocomplete to best guess addresses as you type, it doesn't make sense to have this as a service instance. This implementation is using JavaScript on a reusable view, with only some simple setup needed from the developer's side to get this going.

# Contents
The component is a single, self-contained view. It references the Google Places API, but it is all self contained in the view's initialize method. The only 3 parameters that you will need to pass in before the view is initialized are:
- APIKey - The key you got from your Google Account's Console. If you do not have a key yet, instructions can be found here: [Google Maps](https://developers.google.com/places/web-service/get-api-key)
- ViewName - Name of the View that the autocomplete textbox is located on
- ControlName - Name of the autocomplete textbox

Optional:
- sbmDebug - Parameter that displays a debug table on the component with all the fields that it populated

## Usage
There are two ways to use the component:
1. As Is - You simply drop the view on the form, initialize it and save the values you need to using the controls on the view. 
1. Use Your Own View - Use your own view and map the controls to the component, useful for when you want a different layout or perhaps different fields to be shown.

### Use the View As Is
If the view shows the standard fields you care about and you don't need any customization, simply:
1. Drag the view onto your form
1. Be sure to execute the View's Initialize method, passing in the name of the view and the control
![Mapping](https://github.com/K2-US-TS/Images/blob/master/Documentation/Google%20Address%20Lookup/AddressMapping.png?raw=true)
1. Also remember to provide the Google Places API key, for this example, it is passed in from the Form's parameters
1. Execute the *Initialize Address Lookup View* rule on the view

And that's it. The only thing that is usually required is to map the values that you care about into the method that saves the data. A working example can be found in the *Google Address Lookup Usage* form, located in *Common/Address Lookup/Google*.

### Use Your Own View
This is a common use case when you need a custom layout or show and save fields that aren't display by the standard view the component provided. The sequence is the same as for the standalone usage, however, you will be passing in the names of your view as well as the control name that will be doing the autocompletion.
1. Drag the view component onto your form
1. Set visible to be false
1. Drag the view you intend to use for capturing the address information on as well
1. Be sure to execute the View's Initialize method, passing in the name of **your** view and the control on **your** view, not the component's names
![Mapping](https://github.com/K2-US-TS/Images/blob/master/Documentation/Google%20Address%20Lookup/CustomViewMapping.png?raw=true)
1. Also remember to provide the Google Places API key, for this example, it is passed in from the Form's parameters
1. Execute the *Initialize Address Lookup View* rule on the view

Your own view and it's control will now be able to get populated by the Google Places API. 

Depending on your requirements, saving the address as is might be sufficient, or you would need to save the individual sections. For the latter, there is an event that you can create a rule on to map the individual parts to your own view's controls:<br>
*On TS.US.Common.Google.AddressLookup, when Hidden Data Label Loaded is Changed*.
This is changed after the autocomplete has pulled back the full details, so when this fires:<br>
![Event](https://github.com/K2-US-TS/Images/blob/master/Documentation/Google%20Address%20Lookup/Rule.png?raw=true)<br>
Map the values you need from the hidden component view to your view:
![Mappingt](https://github.com/K2-US-TS/Images/blob/master/Documentation/Google%20Address%20Lookup/OwnViewMapping.png?raw=true)

A working example can also be found in the *Google Address Lookup Using Custom View* form, located in *Common/Address Lookup/Google*.

The view uses the names of the properties returned from the API for the controls, so it may be a bit confusing initially to know what fields need to be passed back. As such, the hidden table has both the known name and the API name as a description available:
![Mappingt](https://github.com/K2-US-TS/Images/blob/master/Documentation/Google%20Address%20Lookup/Debug.png?raw=true)

There are also two sets of controls, one set that ends with *long_name* and one that ends with *short_name*. The only difference is the display, for example:
- **Aurora Avenue North** vs **Aurora Ave N**
- **Washington** vs **WA**
- **United States** vs **US**
