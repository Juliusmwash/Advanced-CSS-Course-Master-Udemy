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

**How CSS is Parsed, Part 2: Value Processing**
How CSS values are processed:
    (1) Declared value
    (2) Cascaded value
    (3) Specified value
    (4) Computed value
    (5) Used value
    (6) Actual value

How units are converted from relative to absolute (px):

### 1. ***%*** for fonts
-When ***%*** is used for font-size, it is relative to the parent element's font size

```CSS
.parent {
  font-size: 20px;
}

.child {
  font-size: 150%;
}
```

The calculation is:
    150% × 20px = 30px

So the child's font size becomes 30px

**Formula**
    absolute font size = percentage × parent's font size

For example:
    120% of 16px = 19.2px
    200% of 16px = 32px
    50% of 20px = 10px

### 2. ***%*** for lengths
For properties such as:

```CSS
width: 50%;
height: 50%;
margin: 10%;
padding: 5%;
```

the percentage is generally calculated relative to a containing/reference box, not the font size.

For example:

```CSS
    .container {
  width: 800px;
}

.child {
  width: 50%;
}
```
The child's width is:

    50% × 800px = 400px

So:

    width: 50%;

becomes:

    width: 400px;

> [!IMPORTANT]
> The reference for ***%*** depends on the property.
> For example, width: 50% is normally relative to the containing block's width, while percentage values for some other properties have different reference rules.

### 3. ***em*** for fonts
When em is used for font-size, it is relative to the parent's font size.

```CSS
.parent {
  font-size: 20px;
}

.child {
  font-size: 1.5em;
}
```

Calculation:
    1.5 × 20px = 30px

Therefore:
    font-size: 1.5em;

becomes:
    font-size: 30px;

**Formula**
    absolute font size = em value × parent's font size

For example:
    1em   × 16px = 16px
    1.5em × 16px = 24px
    2em   × 16px = 32px

### 4. ***em*** for lengths

This is where ***em*** becomes particularly useful.

When ***em*** is used for other properties, such as:

    padding: 2em;
    margin: 1em;
    width: 20em;

it is relative to the font size of the element itself.

For example:

```CSS
.box {
  font-size: 20px;
  padding: 2em;
}
```

Calculation:
    2 × 20px = 40px

So:
    padding: 2em;

becomes:
    padding: 40px;


**The key distinction**
font-size: 2em - Parent's font size
padding: 2em - Element's font size
margin: 2em	- Element's font size
width: 20em	- Element's font size


### 5. rem
***rem*** means **root em**.
It is always relative to the font size of the root element, usually <html>.

```CSS
html {
  font-size: 16px;
}

.box {
  width: 20rem;
}
```

Calculation:
    20 × 16px = 320px

So:
    width: 20rem;

becomes:
    width: 320px;

Unlike ***em***, ***rem*** does not keep changing based on nested parent font sizes.

Example:

```CSS
html {
  font-size: 16px;
}

.parent {
  font-size: 30px;
}

.child {
  width: 10rem;
}
```

The child's width is:
    10 × 16px = 160px

Not:
    10 × 30px = 300px

That's because ***rem*** looks at the root font size.

### 6. ***vh***
***vh*** means viewport height.
The viewport is basically the visible area of the browser window.

```CSS
.box {
  height: 50vh;
}
```

If the viewport is:
    height = 800px

then:
    50vh = 50% × 800px
        = 400px

So:
    height: 50vh;

becomes approximately:
    height: 400px;

**Formula**
    1vh = 1% of viewport height

Therefore:
    50vh = 0.50 × viewport height

### 7. ***vw***

***vw*** means viewport width.

```CSS
.box {
  width: 50vw;
}
```

If the viewport is:
    width = 1200px

then:
    50vw = 50% × 1200px
        = 600px

So:
    width: 50vw;

becomes approximately:
    width: 600px;

**Formula**
    1vw = 1% of viewport width


**Putting everything together**
Suppose we have:

```CSS
html {
  font-size: 16px;
}

.parent {
  font-size: 20px;
  width: 800px;
}

.child {
  font-size: 150%;
  padding: 2em;
  width: 50%;
  height: 50vh;
  margin: 1rem;
}
```

Assume the viewport is:
    1200px wide
    800px tall

Then:

| CSS | Reference | Calculation | Absolute value |
| font-size: 150% | Parent font size| 1.5 × 20px | 30px |
| padding: 2em | Child font size | 2 × 30px | 60px |
| width: 50% | Containing block width | 0.5 × 800px | 400px |
| height: 50vh | Viewport height | 0.5 × 800px | 400px|
| margin: 1rem | Root font size | 1 × 16px | 16px|


Notice something important:
**The same relative unit doesn't necessarily use the same reference.**

For example:
    % → depends on the property
    em → font-size relationship
    rem → root font-size
    vh → viewport height
    vw → viewport width

**Easy way to remember**
Think of the units as asking different questions:
    %    → "50% of what?"
    em   → "How big is the relevant font?"
    rem  → "How big is the root font?"
    vh   → "How tall is the viewport?"
    vw   → "How wide is the viewport?"

Once CSS finds the thing they're relative to, it performs the calculation and obtains a **used/computed length in CSS pixels (px)** for layout/rendering.


**CSS value processing: What you need to know**
- Each property has an initial value, used if nothing is declared (and if there is no inheritance);
- Browsers specify a **root font-size** for each page (usually 16ps);
- Percentages and relative values are always converted to pixels;
- Percentages are measured relative to their parent's font-size, if used to specify font-size;
- Percentages are measured relative to their parent's width, if used to specify lengths;
- em are measured relative to their parent font-size, if used to specify font-size;
- em are measured relative to the current font-size, if used to specify lengths;
- rem are always measured relative to the document's root font-size;
- vh and vw are simply percentage measurements of the viewport's height and width.


**Inheritance: What you need to know**
- Inheritance passes the values for some specific properties from parents to children - **more maintainable code**;
- Properties related to text are inherited: font-family, font-size, color, etc;
- The computed value of a property is what gets inherited, not the declared value.
- Inheritance of a property only works if no one declares a value for that property;
- The ***inherit*** keyword forces inheritance on a certain property;
- The ***initial*** keyword resets a property to its initial value.