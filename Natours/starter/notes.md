# Section 3: How CSS Works: A Look Behind the Scenes

**Three pillars to write good HTML and CSS**
    (1) Responsive Design
        - Fluid layouts
        - Media queries
        - Responsive images
        - Correct units
        - Desktop-first vs mobile-first

    (2) Maintainable and Scalable code
        - Clean
        - Easy to understand
        - Growth
        - Reusable
        - How to organize files
        - How to name classes
        - How to structure HTML
    
    (3) Web Performance
        - Less HTTP requests
        - Less code
        - Compress code
        - Use a CSS preprocessor
        - Less images
        - compress images

**How CSS is Parsed, Part1: The Cascade and Specificity**
- Cascade means the process the browser uses to decide which CSS rule should win when multiple rules apply to the same element.

***Example***
```CSS
p {
    color: red;
}

p {
    color: blue;
}
```
- Both rules target the same <p>. Since they have the same specificity, the later rule wins:

```HTML
<p>Hello</p>
```
- The text will be *blue*.
- This is called the *cascade*

    ### IMPORTANCE > SPECIFICITY > SOURCE ORDER

    (1) Importance:
        User *! important* declarations
        Author *! important* declarations
        Author declarations
        User declarations
        Default browser declarations

***Same importance?***

    (2) Specificity:
        Inline styles
        IDs
        Classes, pseudo-classes, attribute
        Elements, pseudo-elements

***Same specificity?***

    (3) Source Order
        The last decalaration in the code will override all other declarations and will be applied.

> [!IMPORTANT]
> CSS declarations marked with *! important* have the highest priority;
> But, only use *! important* as a last resource. It's better to use correct specificities - **more maintainable code**!
> Inline styles will always have priority over styles in external stylesheets;
> A selector that contains 1 ID is more specific than one with 1000 classes;
> A selector that contains 1 class is more specific than one with 1000 elements;
> The universal selector * has no specificity value (0,0,0,0);
> Rely more on **specificity** than on the **order** of selectors;
> But, rely on order when using 3rd-party stylesheets - always put your author stylesheet last.