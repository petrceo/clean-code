# CSS Clean Code

## Table of Contents

  1. [Introduction](#introduction)
  2. [About BEM](#about-bem)
  2. [Use only class selectors](#use-only-class-selectors)
  3. [Block and elements](#block-and-elements)
  4. [Modifiers](#modifiers)
  5. [Mixes](#mixes)

## Introduction

CSS Code architecture is most important when we want to write clean, readable, maintainable and organised code. The most popular CSS design patterns are **SMACSS** and **BEM**.

**[⬆ back to top](#table-of-contents)**

## About BEM

**BEM** (Block, Element, Modifier) is a component-based approach to web development. The idea behind it is to divide the user interface into independent blocks. This makes interface development easy and fast even with a complex UI, and it allows reuse of existing code without copying and pasting. BEM makes your code scalable and reusable, thus increasing productivity and facilitating teamwork. Even if you are the only member of the team, BEM can be useful for you. 

**[⬆ back to top](#table-of-contents)**

## Use only class selectors

One of the basic rules of the BEM methodology is to use only class selectors. In this section, we’ll explain why.

- Why don’t we use IDs?
- Why don’t we use tag selectors?
- Why don’t we use a universal selector?
- Why don’t we use nested selectors?
- Why don’t we combine a tag and a class in a selector?
- Why don’t we use combined selectors?
- Why don’t we use attribute selectors?

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE IDS (ID SELECTORS)

The ID provides a unique name for an HTML element. If the name is unique, you can’t reuse it in the interface. This prevents you from reusing the code.

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE TAG SELECTORS

HTML page markup is unstable: A new design can change the nesting of the sections, heading levels (for example, from h1 to h3) or turn the p paragraph into the div tag. Any of these changes will break styles that are written for tags. Even if the design doesn’t change, the set of tags is limited. To use an existing layout in another project, you have to solve conflicts between styles written for the same tags.

An extended set of semantic tags can’t meet all layout needs, either.

An example is when the page header contains a logo. A click on the logo opens the main page of the site (index). You can mark it up with tags by using the <img> tag for the image and the <a> tag for the link.

```html
<header>
  <a href="/">
    <img src="img.logo.png" alt="Logo">
  </a>
</header>
```

To distinguish between the logo link and an ordinary link in the text, you need extra styles. Now remove underlining and the blue color from the logo link:

```css
header a {
  ...
}
```

The logo link doesn’t need to be shown on the main page, so change the index page markup:

```html
<header>
  <!-- the <a> tag is replaced with <span> -->
  <span>
    <img src="img.logo.png" alt="Logo">
  </span>
</header>
```

You don’t need to remove the underlining and the blue color for the <span> tag. So let’s make general rules for the logo link from different pages:

```css
header a,
header span {
  ...
}
```

At first glance, this code seems all right, but imagine if the designer removes the logo from the layout. The selector names don’t help you understand which styles should be removed from the project with the logo. The “header a” selector doesn’t show the connection between the link and the logo. This selector could belong to the link in the header menu or, for example, to the link to the author’s profile. The “header span” selector could belong to any part of the header.

To avoid confusion, just use the logo class selector to write the logo styles:

```css
.logo {
  ...
}
```

**Bad:**

```html
<header>
  <a href="/">
    <img src="img.logo.png" alt="Logo">
  </a>
</header>
```

```css
header a {
  ...
}
```

**Good:**

```html
<header>
  <a class="logo" href="/">
    <img src="img.logo.png" alt="Logo">
  </a>
</header>
```

```css
.logo {
  ...
}
```

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE THE UNIVERSAL SELECTOR (*)

The universal selector indicates that the project features a style that affects all nodes in the layout. This limits reuse of the layout in other projects:

- You have to additionally transfer the styles with an asterisk to the project. But in this case, the universal selector might affect the styles in the new project.
- The styles with an asterisk must be added to the layout you are transferring.
In addition, a universal selector can make the project code unpredictable. For example, it can affect the styles of the universal library components.

Common styles don’t save you time. Often developers start by resetting all margins for components (* { margin: 0; padding: 0; }), but then they still set them the same as in the layout (for example, margin: 12px; padding: 30px;).

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE NESTED SELECTORS

Nested selectors increase code coupling and make it difficult to reuse the code.

The BEM methodology doesn’t prohibit nested selectors, but it recommends not to use them too much. For example, nesting is appropriate if you need to change styles of the elements depending on the block’s state or its assigned theme.

```css
.button_hovered .button__text {
  text-decoration: underline;
}

.button_theme_islands .button__text {
  line-height: 1.5;
}
```

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE COMBINED SELECTORS

Combined selectors are more specific than single selectors, which makes it more difficult to redefine blocks.

Consider the following code:

```html
<button class="button button_theme_islands">...</button>
```

Let’s say you set CSS rules in the .button.button_theme_islands selector to do less writing. Then you add the “active” modifier to the block:

```html
<button class="button button_theme_islands button_active">...</button>
```

The .button_active selector doesn’t redefine the block properties written as .button.button_theme_islands because .button.button_theme_islands is more specific than .button_active. To redefine it, combine the block modifier selector with the .button selector and declare it below the .button.button_theme_islands because both selectors are equally specific:

```css
.button.button_theme_islands {}

.button.button_active {}
```

If you use simple class selectors, you won’t have problems redefining the styles:

```css
.button_theme_islands {}

.button_active {}

.button {}
```

**Bad:**

```css
.button.button_theme_islands {}

.button.button_active {}
```

**Good:**

```css
.button_theme_islands {}

.button_active {}

.button {}
```

**[⬆ back to top](#table-of-contents)**

### WE DON’T COMBINE A TAG AND A CLASS IN A SELECTOR

Combining a tag and a class in the same selector (for example, button.button) makes CSS rules more specific, so it is more difficult to redefine them.

Consider the following code:

```html
<button class="button">...</button>
```

Let’s say you set CSS rules in the button.button selector. Then you add the active modifier to the block:

```html
<button class="button button_active">...</button>
```

The .button_active selector doesn’t redefine the block properties written as button.button because button.button is more specific than .button_active. To make it more specific, you should combine the block modifier selector with the button.button_active tag.

As the project develops, you might end up with blocks with input.button, span.button or a.button selectors. In this case, all modifiers of the button block and all its nested elements will require four different declarations for each instance.

### Possible Exceptions

In rare cases, the methodology allows combining tag and class selectors. For example, this can be used for setting the comments style in CMS systems that can’t generate the correct layout.

You can use the comment to write a text, insert images, or add markup. To make them match the site design, the developer can pre-define styles for all tags available to the user and cascade them down to the nested blocks:

```html
<div class="content">
  ... <!-- the user’s text -->
</div>
```

```css
.content a {
  ...
}

.content p {
  font-family: Arial, sans-serif;
  text-align: center;
}
```

**[⬆ back to top](#table-of-contents)**

### WE DON’T USE ATTRIBUTE SELECTORS

Attribute selectors are less informative than class selectors. As proof, consider an example with a search form in the header:

```html
<header>
  <form action="/">
    <input name="s">
    <input type="submit">
  </form>
</header>
```

Try using selector attributes to write the form styles:

```css
header input[type=submit],
header input[type=checkbox] {
  width: auto;
  margin-right: 20px;
}

header input[type=checkbox] {
  margin: 0;
}
```

In this example, you can’t tell for sure from the selector name that the styles belong to the search form. Using classes makes it clearer. Classes don’t have restrictions that prevent you from writing clearly.

**Summary:** class is the only selector that allows you to isolate the styles of each component in the project; increase the readability of the code and do not limit the re-use of the layout.

CSS styles isolation is the most frequent start point of the BEM journey. But this is the least that BEM can give you. To understand how isolated independent components are arranged in BEM, you need to learn the basic concepts, i.e. Block, Element, Modifier, and Mix. Let’s do this in the next section.

**[⬆ back to top](#table-of-contents)**

## Block and elements

The BEM methodology is a set of universal rules that can be applied regardless of the technologies used, such as CSS, Sass, HTML, JavaScript or React.

BEM helps to solve the following tasks:

- Reuse the layout;
- Move layout fragments around within a project safely;
- Move the finished layout between projects;
- Create stable, predictable and clear code;
- Reduce the project debugging time.
- In a BEM project, the interface consists of blocks that can include elements. Blocks are independent components of the page. An element can’t exist outside the block, so keep in mind that each element can belong to one block only.

The first two letters in BEM stand for Blocks and Elements. The block name is always unique. It sets the namespace for elements and provides a visible connection between the block parts. Block names are long but clear in order to show the connection between components and to avoid losing any parts of these components when transferring the layout.

To see the full power of BEM naming, consider this example with a form. According to the BEM methodology, the form is implemented using the form block. In HTML, the block name is included in the class attribute:

```html
<form class="form" action="/">
```

All parts of the form (the form block) that don’t make sense on their own are considered its elements. So the search box (search) and the button (submit) are elements of the form block. Classes also indicate that an element belongs to the block:

```html
<form class="form" action="/">
  <input class="form__search" name="s">
  <input class="form__submit" type="submit">
</form>
```

Note that the block’s name is separated from the element’s name with a special separator. In the BEM classic naming scheme, two underscores are used as a separator. Anything can work as a separator. There are alternative naming conventions, and each developer chooses the one that suits them. The important thing is that separators allow you to distinguish blocks from elements and modifiers programmatically.

Selector names make it clear that in order to move the form to another project, you need to copy all of its components:

```css
.form__search {}

.form__submit {}
```

Using blocks and elements for class names solves an important problem: It helps us get rid of nested selectors. All selectors in a BEM project have the same weight. That means it is much easier to redefine styles written according to BEM. Now, to use the same form in another project, you can just copy its layout and styles.

The idea of the naming of BEM components is that you can explicitly define the connection between the block and its elements.

**[⬆ back to top](#table-of-contents)**

## Modifiers

A modifier defines the look, state and behavior of a block or an element. Adding modifiers is optional. Modifiers let you combine different block features, as you can use any number of modifiers. But a block or an element can’t be assigned different values of the same modifier.

Let’s explore how modifiers work.

Imagine the project needs the same search form as in the example above. It should have the same functions but look different (for example, the search forms in the header and in the footer of the page should differ). The first thing you can do to change the appearance of the form is to write additional styles:

```css
header .form {}

footer .form {}
```

The header .form selector has more weight than the form selector, which means that one rule will override the other one. But as we have discussed, nested selectors increase code coupling and make reuse difficult, so this approach doesn’t work for us.

In BEM, you can use a modifier to add new styles to the block:

```html
<!-- Added the form_type_original modifier-->
<form class="form form_type_original" action="/">
  <input class="form__search" name="s">
  <input class="form__submit" type="submit">
</form>
```

The line <form class="form form_type_original"></form> indicates that the block was assigned a type modifier with the original value. In a classic scheme, the modifier name is separated from the block or element name with an underscore.

The form can have a unique color, size, type, or design theme. All these parameters can be set with a modifier:

```html
<form class="form form_type_original form_size_m form_theme_forest">
```

The same form can look different but stay the same size:

```html
<form class="form form_type_original form_size_m form_theme_forest"></form>
<form class="form form_type_original form_size_m form_theme_sun"></form>
```

But the selectors for each modifier will still have the same weight:

```css
.form_type_original {}

.form_size_m {}

.form_theme_forest {}
```

**Important:** A modifier contains only additional styles that change the original block implementation in some way. This allows you to set the appearance of a universal block only once, and add only those features that differ from the original block code into the modifier styles.

```css
.form {
  /* universal block styles */
}

.form_type_original {
  /* added styles */
}
```

This is why a modifier should always be on the same DOM node with the block and the element it is associated with.

```html
<form class="form form_type_original"></form>
```

You can use modifiers to apply universal components in very specific cases. The block and element code doesn’t change. The necessary combination of modifiers is created on the DOM node.

**[⬆ back to top](#table-of-contents)**

## Mixes

A mix allows you to apply the same formatting to different HTML elements and combine the behavior and styles of several entities while avoiding code duplication. They can replace abstract wrapper blocks.

A mix means that you host several BEM entities (blocks, elements, modifiers) on a single DOM node. Similar to modifiers, mixes are used for changing blocks. Let’s look at some examples of when you should use a mix.

Blocks can differ not only visually but also semantically. For example, a search form, a registration form and a form for ordering cakes are all forms. In the layout, they are implemented with the “form” block but they don’t have any styles in common. It is impossible to handle such differences with a modifier. You can define common styles for such blocks but you won’t be able to reuse the code.

```css
.form,
.search,
.register {
  ...
}
```

You can use a mix to create semantically different blocks for the same form:

```html
<form class="form" action="/">
  <input class="form__search" name="s">
  <input class="form__submit" type="submit">
</form>
```

The .form class selector describes all styles that can be applied to any form (order, search or registration):

```css
.form {}
```

Now you can make a search form from the universal form. To do this, create an additional search class in the project. This class will be responsible only for the search. To combine the styles and behavior of the.formand.search classes, place these classes on a single DOM node:

```html
<form class="form search" action="/">
  <input class="form__search" name="s">
  <input class="form__submit" type="submit">
</form>
```

In this case, the .search class is a separate block that defines behavior. This block can’t have modifiers responsible for the form, themes, and sizes. These modifiers already belong to the universal form. A mix helps to combine the styles and behavior of these blocks.

Let’s take one more example where the component’s semantics is changed. Here is a navigation menu in the page header in which all entries are links:

```html
<nav class="menu">
  <a class="link" href=""></a>
  <a class="link" href=""></a>
  <a class="link" href=""></a>
</nav>
```

The link functionality is already implemented in the link block, but the menu links have to differ visually from the links in the text. There are several ways to change the menu links:

**Create a menu entry modifier that turns the entry into a link:**

```html
<nav class="menu">
  <a class="link" href=""></a>
  <a class="link" href=""></a>
  <a class="link" href=""></a>
</nav>
```

In this case, to implement the modifier, you should copy the `link` block behavior and styles. This will lead to code duplication.


**Use a mix of the `link` universal block and the `item` element of the `menu` block:**

```html
<nav class="menu">
  <a class="link menu__item" href=""></a>
  <a class="link menu__item" href=""></a>
  <a class="link menu__item" href=""></a>
</nav>
```

With the mix of the two BEM entities, you can now implement the basic link functionality from the `link` block and additional CSS rules from the `menu` block, and avoid code duplication.

**[⬆ back to top](#table-of-contents)**

### External Geometry And Positioning: Giving Up Abstract HTML Wrappers

Mixes are used to position a block relative to other blocks or to position elements inside a block. In BEM, styles responsible for geometry and positioning are set in the parent block. Let’s take a universal menu block that has to be placed in the header. In the layout, the block has to have a 20px indent from the parent block.

This task has several solutions:

**Write styles with indents for the menu block:**

```css
.menu {
  margin-left: 20px;
}
```

In this case, the "menu" block isn’t universal anymore. If you have to place the menu in the page footer, you will have to edit styles because the indents will probably be different.

**Create the menu block modifier:**

```html
<div class="header">
  <ul class="menu menu_type_header">
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
  </ul>
</div>
```

```css
.menu_type_header {
  margin-left: 20px;
}

.menu_type_footer {
  margin-left: 30px;
}
```

In this case, the project will include two kinds of menus, although this is not the case. The menu stays the same.

**Define the external positioning of the block:  nest the `menu` block in the abstract wrapper (for example, the `wrap` block) setting all indents:**

```html
<div class="wrap">
  <ul class="menu">
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
  </ul>
</div>
```

To avoid the temptation to create modifiers and change the block styles to position the block on the page, you need to understand one thing: The indent from a parent block isn’t a feature of the nested block. It is a feature of the parent block. It has to know that the nested block has to be indented from the border by a certain number of pixels.

**Use a mix. The information about nested block positioning is included in the parent block elements. Then the parent block element is mixed into the nested block. In this case, the nested block doesn’t specify any indents and can be easily reused in any place.**

Let’s go on with our example:

```html
<div class="wrap">
  <ul class="menu header__menu">
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
    <li class="menu__item"><a href=""></a></li>
  </ul>
</div>
```

In this case, external geometry and positioning of the menu block are set through the header__menu element. The menu block doesn’t specify any indents and can be easily reused.

The parent block element (in our case it is header__menu) performs the task of the wrapper blocks responsible for external positioning of the block.

**[⬆ back to top](#table-of-contents)**
