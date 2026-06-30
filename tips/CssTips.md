# CSS Development Tips

### This file contains tips and tricks to help you write CSS selectors more effectively.  While some of the tips are generic and apply to all BBj applications, many of them apply to running your BBj application in a web browser via BBj's Dynamic Web Client (DWC).



## 1.  Scoping your CSS 
### Using the @scope rule and :has() pseudo class
Assume that you've created a BBj GUI application that you intend to run in the DWC that creates a top level window similar to:
```java
        REM Create the main window and ensure that the title bar is showing ($01000000$ hides the title bar)
        winMainFlags$ = $00181093$
        #winMain! = #sysGui!.addWindow("My App's Main Window", winMainFlags$)
        #winMain!.addClass("winMain")
```
The important note here is that we added the '**winMain**' class to our app's main window.  We can then use custom CSS to modify the main window's titlebar text and buttons.

```css
  @scope (dwc-frame:has(dwc-window-center.winMain)) {

    --app-color-translucent-black: hsla(0,0%,0%,.5);
    --app-color-translucent-white: hsla(0,0%,100%,.5);
    --app-color-translucent-red:   hsla(0,100%,40%,.5);

    /* This sets the background color of the titlebar and removes the border radius */
    dwc-frame-titlebar {
      background: steelblue;
      border-radius: 0;
    }

    /* This modifies the titlebar text style */
    dwc-frame-titlebar-text {
      font-size: 1.1em;
      font-weight: bold;
      color: azure;
      text-shadow: 1px 1px 2px var(--app-color-translucent-black);
    }

    /* This sets the background and the color for the window decoration buttons */
    /* Note: You could also define the following CSS custom properties for the buttons:
      --dwc-window-minimize-background, --dwc-window-maximize-background, --dwc-window-close-background,
      which is a much better way to accomplish the task of changing their colors.
     */
    button.dwc-minimize,
    button.dwc-maximize,
    button.dwc-close {
      background: var(--app-color-translucent-black) !important;
      color: orange;
    }
    button.dwc-close {
      background: var(--app-color-translucent-red) !important;
    }

    /* Just because we can, we can hide the BBj icon in the titlebar  */
    dwc-frame-titlebar img {
      opacity: 0;
    }

  }

```

The most interesting part of the CSS is this line:
`@scope (dwc-frame:has(dwc-window-center.winMain)) {`

That line accomplishes two separate things:
1. We can use the `@scope` rule to apply styles only within the context of the given scope (the `dwc-frame` of the main window)
2. We define the scope by specifying the selector for the main window's frame using the `:has()` pseudo-class.

### Note: 
If the selector was just `dwc-frame`, the styles would apply to ***every*** DWC window frame, not just the main window's frame.
We are limiting the scope of the styles to only the main window's frame, because the selector will only target `dwc-frame` elements
that have a `dwc-window-center` element with the class `winMain` as a descendant.  The `dwc-window-center` element is created
automatically by BBj when constructing the top level window, and it has the class `winMain` because we specified it in the
`addClass("winMain")` method call in the BBj code above.  Therefore, all selectors and defined styles within this `@scope` rule
will only apply to the main window's frame.