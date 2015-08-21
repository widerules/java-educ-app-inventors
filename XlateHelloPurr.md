# Introduction #

This is a first step-to-step tutorial explaining how to translate by hand a source developed in App Inventor to a Java project for Eclipse IDE to either compile and run on an Android phone/tablet device or export an APK file for sharing purpose.

Many of learned App Inventor developers were already familiar with the first App Inventor example, HelloPurr, as shown in the feature below. This tutorial contains two main parts how to convert the source:

  1. Design part - components / properties
  1. Blocks part - code logic

![https://lh4.googleusercontent.com/-T19yZk7uskE/T2Oa_WZveuI/AAAAAAAAAFA/jG0pDhuPWFE/s573/design.png](https://lh4.googleusercontent.com/-T19yZk7uskE/T2Oa_WZveuI/AAAAAAAAAFA/jG0pDhuPWFE/s573/design.png)

# 1. Design Part #

The feature above shows a list of components at the right:


**List of components**
  * Screen1
    * Button1 (Type Button)
    * Label1 (Type Label)
    * Sound1 (Type Sound)

And, the properties of each component (not shown in the feature) that were changed as following:

**Properties**

For the button component:
| Name | Button1 |
|:-----|:--------|
| Type | Button  |
| Image | kitty.png |

For the label component
| Name | Label1 |
|:-----|:-------|
| Type | Label  |
| BackgroundColor | Blue   |
| FontSize | 30.0   |
| Text | Pet the kitty |
| TextAlignment | center |
| TextColor | Pink   |
| Width | Fill parent |

For the sound component:
| Name | Sound1 |
|:-----|:-------|
| Type | Sound  |
| Source | meow.mp3 |

**NOTE**: The first two entries of each are for reference only.

### Convert the design into Java code ###
NOTE: For those who are familiar with Eclipse IDE startup (see EclipseStartup.)
  1. By using Eciplse IDE, create a new Android project
    * Project name: HelloPurrExample
    * Select Build Target: Android 2.2
    * Package Name: com.javabridge.hellopurrexample
  1. Create a new folder named `libs`
  1. Add JavaBridge jar to the new folder
  1. Add build path to the jar
  1. Add media files to the assets folder:
    * kitty.png
    * meow.png
  1. Open the main Java code as shown below:

```
package com.javabridge.hellopurrexample;

import android.app.Activity;
import android.os.Bundle;

public class HelloPurrExampleActivity extends Activity {
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }
}
```

7. According to the list of components above, replace the class section with a new one below

```
public class Screen1 extends Form
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
}
```

**NOTE**: Be sure to update the import statements by holding `CTRL-SHIFT` down and pressing `O` keys.

8. Add $define method inside of the class section

```
public class Screen1 extends Form
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
    
    void $define()
    {
        // Place all properties here
    }
}
```

9. According to the tables above, convert all properties to Java code
```
public class Screen1 extends Form
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
    
    void $define() 
    {
        Button1 = new Button(this);
        Button1.Image("kitty.png");
        
        Label1 = new Label(this);
        Label1.BackgroundColor(COLOR_BLUE);
        Label1.FontSize(30.0F);
        Label1.Text("Pet the kitty");
        Label1.TextAlignment(CENTER);
        Label1.TextColor(COLOR_PINK);
        Label1.Width(FILL_PARENT);
        
        Sound1 = new Sound(this);
        Sound1.Source("meow.mp3");
    }
}
```

10. Compile and run the code to see all goes well.

# 2. Blocks Part #

### Event handling ###

Every source written in App Inventor blocks must have at least one event block (green) in order to invoke some action. In Java code, it is necessary to implement some event handlers as instructed below:

1. Add event handlers to Java code

```
public class Screen1 extends Form
        implements HandlesEventDispatching  // added here
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
    
    void $define() {

        ...
        
        // Place all events to register here                
        EventDispatcher.registerEventForDelegation(this, "Button1", "Click");
    }
}
```

**NOTE**: This example has only one event block, namely, Button1.Click.

2. Add dispatchEvent method following the $define method.

```
public class Screen1 extends Form
        implements HandlesEventDispatching
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
    
    void $define() {

        ...
        
        // Place all events to register here                
        EventDispatcher.registerEventForDelegation(this, "Button1", "Click");
    }
    
    @Override
    public boolean dispatchEvent(Component component, String id,
            String eventName, Object[] args) {

        // Add events blocks here

        return false;
    }
}
```


### Translate blocks to Java code ###

The click event block

![https://lh3.googleusercontent.com/-_pI_qDbliAQ/T2Oa_brKUFI/AAAAAAAAAFI/72RKFiofnkI/s249/button-click.png](https://lh3.googleusercontent.com/-_pI_qDbliAQ/T2Oa_brKUFI/AAAAAAAAAFI/72RKFiofnkI/s249/button-click.png)

3. Add if statement for each event block inside of the dispatchEvent method

```
    @Override
    public boolean dispatchEvent(Component component, String id,
            String eventName, Object[] args) {

        if (component.equals(Button1) && eventName.equals("Click")) {

            // Add additional blocks here
            
            return true;
        }

        return false;
    }
```

4. Add additional blocks to each event block

```
public class Screen1 extends Form
        implements HandlesEventDispatching  // add here
{
    private Button Button1;
    private Label Label1;
    private Sound Sound1;
    
    void $define() {

        ...
        
        // Place all events to register here                
        EventDispatcher.registerEventForDelegation(this, "Button1", "Click");
    }
    
    @Override
    public boolean dispatchEvent(Component component, String id,
            String eventName, Object[] args) {

        if (component.equals(Button1) && eventName.equals("Click")) {
            Sound1.Play();    // added the player to play
            return true;
        }

        return false;
    }
}
```

5. Compile and run the device

That's all there is to it!

### Uploaded Java code ###

You'll find the complete Java code available at Downloads page. Fetch [here](http://code.google.com/p/java-educ-app-inventors/downloads/detail?name=HelloPurr_1.0.zip)